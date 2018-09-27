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
* TextNode.nodeName ==== '#text', HTMLHeadingElement.nodeName === 'H1'... (up case)
* use Node.childNodes[0] to get it's text content. (i.e. `<h1>aaa</h1>`)



