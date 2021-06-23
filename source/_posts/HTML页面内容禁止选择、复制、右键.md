---
title: html内容禁止选择、复制、右键等
date: 2021-06-23 10:09:55
tags:
- Html
categories:
- 前端

---
html内容禁止选择、复制、右键...
<!--more-->

#### HTML页面内容禁止选择、复制、右键

```html
<body leftmargin=0 topmargin=0 oncontextmenu='return false' ondragstart='return false' onselectstart ='return false' onselect='document.selection.empty()' oncopy='document.selection.empty()' onbeforecopy='return false' onmouseup='document.selection.empty()'>
</body>
```

#### 禁止网页另存为：在body后面加入以下代码： 

```html
<noscript> 
	<iframe src="*.htm"></iframe> 
</noscript> 
```

