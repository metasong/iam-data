# css center align
*summery*

https://segmentfault.com/a/1190000013565024
 
```html
<div class="parent">
    <div class="child>DEMO</div>
</div>
```
## horizonal center alignment

```css
// 1
.child{
    display:inline-block;
}
.parent{
    text-align:center;
}
// 2
.child {
    display:table;
    margin:0 auto;
}
// 3
.parent {
    position:relative;
}
.child {
    position:absolute;
    left:50%;
    transform:translateX(-50%);
}
// 4
.parent {
    display:flex;
}
.child {
    margin:0 auto;
}
// 5
.parent {
    display:flex;
    justify-content:center;
}
// 6

```
## vertical center alignment 
```html
<div class="parent">
    <div class="child>DEMO</div>
</div>
```
```css
// 1
.parent {
    display:table-cell;
    vertical-align:middle;
}
// 2
.parent {
    position:relative;
}
.child {
    position:absolute;
    top:50%;
    transform:translateY(-50%);
}
// 3
.parent {
    position:flex;
    align-items:center;
}

// 4

```
## vertical & horizontal alignment

```html
<div class="parent">
    <div class="child>DEMO</div>
</div>
```

```css
// 1
.parent {
    position:relative;
}
.child {
    position:absolute;
    left:50%;
    top:50%;
    transform:tranplate(-50%,-50%);
}

// 2
.parent {
    text-align:center;
    display:table-cell;
    vertical-align:middle;
}
.child {
    display:inline-block;
}

// 3
.parent {
    display:flex;
    justify-content:center;
    align-items:center;
}

// 4

```

