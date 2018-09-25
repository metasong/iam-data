# The Shapes of CSS
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---

## Circle

<style>
.circle {
	width: 100px;
	height: 100px;
	background: red;
	border-radius: 50px;
}

#yin-yang {
	width: 96px;
	height: 48px;
	background: #eee;
	border-color: red;
	border-style: solid;
	border-width: 2px 2px 50px 2px;
	border-radius: 100%;
	position: relative;
}

#yin-yang1:before {
	content: "";
	position: absolute;
	top: 50%;
	left: 0;
	background: #eee;
	border: 18px solid red;
	border-radius: 100%;
	width: 12px;
	height: 12px;
}

#yin-yang1:after {
	content: "";
	position: absolute;
	top: 50%;
	left: 50%;
	background: red;
	border: 18px solid #eee;
	border-radius:100%;
	width: 12px;
	height: 12px;
}

</style>
<div class="circle"></div>

```css
.circle {
	width: 100px;
	height: 100px;
	background: red;
	border-radius: 50px;
}

```
<div id="yin-yang"> </div>

```css

#yin-yang {
	width: 96px;
	height: 48px;
	background: #eee;
	border-color: red;
	border-style: solid;
	border-width: 2px 2px 50px 2px;
	border-radius: 100%;
	position: relative;
}

#yin-yang:before {
	content: "";
	position: absolute;
	top: 50%;
	left: 0;
	background: #eee;
	border: 18px solid red;
	border-radius: 100%;
	width: 12px;
	height: 12px;
}

#yin-yang:after {
	content: "";
	position: absolute;
	top: 50%;
	left: 50%;
	background: red;
	border: 18px solid #eee;
	border-radius:100%;
	width: 12px;
	height: 12px;
}
```

https://css-tricks.com/examples/ShapesOfCSS/