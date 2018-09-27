# Using Google IDOM in MarkdownIt
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---
## IDOM intruction

### Angular Ivy change dection execution
https://blog.angularindepth.com/angular-ivy-change-detection-execution-are-you-prepared-ab68d4231f2c#7ff5

[source code](https://jsfiddle.net/yurzui/hqhq4khc)

#### learning note
* Text->CharacterData->Node->EventTarget->Object
* HTMLHeadingElement->HTMLElement->Element->Node->EventTarget->Object.
* nodeName is defined on Node, 
* CharacterData has a data attribute to store it's text content.
* Text.nodeName ==== '#text', HTMLHeadingElement.nodeName === 'H1'... (up case)
* use Node.childNodes[0] to get it's text content. (i.e. `<h1>aaa</h1>`)

```js
const NODE_DATA_KEY = '__ID_Data__';

// The current nodes being processed
let currentNode = null;
let currentParent = null;

class NodeData {
  // key
  // attrs
  
  constructor(name) {
    this.name = name;
    this.text = null;
  }
}

function getData(node) {
  if (!node[NODE_DATA_KEY]) {
    node[NODE_DATA_KEY] = new NodeData(node.nodeName.toLowerCase());
  }

  return node[NODE_DATA_KEY];
}

function enterNode() {
  currentParent = currentNode;
  currentNode = null;
}

function nextNode() {
  currentNode = currentNode ? currentNode.nextSibling : currentParent.firstChild;
}

function exitNode() {
  currentNode = currentParent;
  currentParent = currentParent.parentNode;
}

const matches = function(matchNode, name/*, key */) {
  const data = getData(matchNode);
  return name === data.name // && key === data.key;
};

function renderDOM(name) {
  if (currentNode && matches(currentNode, name/*, key */)) {
    return currentNode;
  }

  const node = name === '#text' ? 
  	document.createTextNode('') :
    document.createElement(name);

  currentParent.insertBefore(node, currentNode);

  currentNode = node;

  return node;
}

function elementOpen(name) {
  nextNode();
  const node = renderDOM(name);
  enterNode();

  // check for updates, i.t attributes
  const data = getData(node);

  return currentParent;
}

function elementClose(node) {
  exitNode();

  return currentNode;
}

function text(value) {
  nextNode();
  const node = renderDOM('#text');

  // checks for text updates
  const data = getData(node);
  if (data.text !== value) {
    data.text = (value);
    node.data = value;
  }

  return currentNode;
}


function patch(node, fn, data) {
  currentNode = node;

  enterNode();
  fn(data);
  exitNode();
};


function render(data) {
  elementOpen('h1');
  {
    text('Hello, ' + data.user)
  }
  elementClose('h1');
  elementOpen('ul')
  {
    elementOpen('li'); 
    {
      text('Counter: ')
      elementOpen('span'); 
      {
        text(data.counter);
      }
      elementClose('span');
    }
    elementClose('li');
  }

  elementClose('ul');
}


document.querySelector('button').addEventListener('click', () => {
   data.counter ++;
   patch(document.body, render, data);
});
document.querySelector('input').addEventListener('input', (e) => {
   data.user = e.target.value;
   patch(document.body, render, data);
});

const data = {
  user: 'Alexey',
  counter: 1
};

patch(document.body, render, data);


```
