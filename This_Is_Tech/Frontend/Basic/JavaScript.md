---
tags:
  - frontend
icon: FabJs
---

# JavaScript

## Introduction

JavaScript是全球最流行的编程语言，是Web开发者必学的三种语言之一，其对网页行为进行编程

> [!info] 参考教程
> [W3school](https://www.w3school.com.cn/js/index.asp)

---

## Usage

### 内部引用

在HTML中，JS代码必须位于`<script>`和`</script>`之间

```html
<script>
document.getElementById("demo").innerHTML = "我的第一段 JavaScript";
</script>
```

### 外部引用

脚本可以放置在外部文件中

> [!example] 外部引用JS
> ```js title:"myScript.js"
> function myFunction() {
> 	document.getElementById("demo").innerHTML = "段落被更改";
> }
> ```
> 
> ```html title:"index.html"
> <script src="myScript.js"></script>
> ```
