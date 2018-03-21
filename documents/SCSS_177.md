# SCSS
*summery*

https://sass-lang.com/documentation/file.SASS_REFERENCE.html
### Referencing Parent Selectors: &
```css
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
  &-sidebar { border: 1px solid; }
}

```
### Nested Properties
```css
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
// The property namespace itself can also have a value.
.funky {
  font: 20px/24px fantasy {
    weight: bold;
  }
}
```