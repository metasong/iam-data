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
}

```