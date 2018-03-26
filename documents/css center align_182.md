# css center align
*summery*

https://segmentfault.com/a/1190000013565024
 
```html
<div class="parent">
    <div class="child>DEMO</div>
</div>
```
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
```