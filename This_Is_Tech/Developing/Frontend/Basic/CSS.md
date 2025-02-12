---
tags:
  - frontend
icon: SiCsswizardry
---

# CSS

## Introduction

CSS(*Cascading Style Sheet*)是一种描述HTML文档样式的语言，描述了如何在屏幕、纸张或其它媒体上显示HTML元素

HTML从未打算包含用于格式化网页的标签，创建其目的是**描述网页**的内容而不是其格式，因此为了避免大型网站开发时将大量的格式和样式信息包含如HTML中，CSS出现了

> [!info] 完整教程
> [W3school](https://www.w3school.com.cn/css/index.asp)

---

## Basic

CSS规则集由**选择器**和**声明块**组成，其中

- 选择器指向需要设置样式的HTML元素
- 声明块包含一条或多条**用分号分隔**的声明
	- 每条声明都包含一个CSS属性名称和一个值，用**冒号**分隔

> [!example] 基础语法
> ```css
> /* 这是一条注释 */
> p {
> 	color: red;
> 	text-align: center;
> }
> ```
> 在这个实例中，所有的`<p>`元素都将居中对齐，并带有红色文本颜色

### 选择器

CSS选择器用于查找或选取需要设置样式的HTML元素

#### 元素选择器

元素选择器根据元素名称选择HTML元素

> [!example] 元素选择器
> ```css
> p {
> 	text-align: center;
> 	color: red;
> }
> ```
> 在这个实例中，页面上所有的`<p>`元素将居中对齐，并带有红色文本颜色

#### ID选择器

ID选择器使用HTML元素的ID属性来选择特定元素

元素的ID在页面中是唯一的，因此用ID选择器选择唯一一个元素

要选取具有特定ID的元素，使用`#`

> [!example] ID选择器
> ```css
> #para1 {
> 	text-align: center;
> 	color: red;
> }
> ```
> 在这个实例中，`id=para1`的元素将被应用CSS样式

> [!warning] 注意
> `id`不能以数字开头

#### 类选择器

类选择器用于选择具有特定`class`属性的HTML元素

使用`.`后紧跟类名的方法

> [!example] 类选择器1
> ```css
> .center {
> 	text-align: center;
> 	color: red;
> }
> ```
> 在这个实例中，所有带有`class="center"`的HTML元素都将红色且居中对齐

> [!example] 类选择器2
> ```css
> p.center {
> 	text-align: center;
> 	color: red;
> }
> ```
> 在这个实例中，所有带有`class="center"`的`<p>`元素都将红色且居中对齐

> [!tip] 多类使用
> HTML元素也可以引用多个类
> > [!example] 多类使用实例
> > ```html
> > <p class="center large">这个段落引用两个类。</p>
> > ```
> > 在这个实例中，引用了两个类`class="center"`和`class="large"`

> [!warning] 注意
> 类名不能以数字开头

#### 通用选择器

通用选择器用于选取页面上所有的HTML元素

> [!example] 通用选择器
> ```css
> * {
> 	text-align: center;
> 	color: blue;
> }
> ```
> 在这个实例中，页面上的所有HTML元素都受到影响

#### 分组选择器

分组选择器用于选取所有具有相同样式定义的HTML元素

> [!example] 分组选择器
> ```css
> h1, h2, p{
> 	tet-align: center;
> 	color: blue;
> }
> ```

---

## Usage

有三种插入样式表的方法：

- 外部CSS
- 内部CSS
- 行内CSS

### 外部CSS

通过使用外部样式表，只需要修改一个文件即可改变整个网站的外观

> [!note] 注意
> 每张HTML页面必须在`head`部分的`<link>`元素内包含对外部样式表文件的引用

```html title:"index.html"
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
<body>
<h1>This is a heading</h1>
<p>This is a paragraph</p>
</body>
</html>
```

```css title:"mystyle.css"
body {
	background-color: lightblue;
}
h1 {
	color: navy;
	margin-left: 20px;
}
```

### 内部CSS

如果一张HTML拥有唯一的样式，可以使用内部样式表

内部样式是在`head`部分的`<style>`中进行定义

```html
<!DOCTYPE html>
<html>
<head>
<!-- 内部样式表 -->
<style>
body {
  background-color: linen;
}

h1 {
  color: maroon;
  margin-left: 40px;
} 
</style>
<!-- 内部样式表 -->
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html>
```

### 行内CSS

行内CSS可用于为单个元素应用唯一的样式

使用`style`属性添加到相关元素

```html
<!DOCTYPE html>
<html>
<body>

<h1 style="color:blue;text-align:center;">This is a heading</h1>
<p style="color:red;">This is a paragraph.</p>

</body>
</html>
```

### 多样式表

如果在不同样式表中为同一选择器定义了一些属性，则将使用最后读取的样式表中的值

### 层叠顺序

当某个HTML元素制定了多个样式时，会根据以下规则层叠为新的虚拟样式表

1. 行内样式
2. 外部和内部样式表
3. 浏览器默认样式

其中行内样式具有最高优先级，并且高级覆盖低级

---

## Color

CSS支持的颜色包括预定义的颜色名称，[RGB(RGBA)值](https://www.w3school.com.cn/css/css_colors_rgb.asp)，[HEX值](https://www.w3school.com.cn/css/css_colors_hex.asp)，[HSL(HSLA)值](https://www.w3school.com.cn/css/css_colors_hsl.asp)

> [!info] 预定义的颜色名称
> ```txt
> Tomato
> Orange
> DodgerBlue
> MediumSeaGreen
> Gray
> SlateBlue
> Violet
> LightGray
> ```

### 颜色属性

> [!example] 背景色
> ```html
> <h1 style="background-color:DodgerBlue;">China</h1>
> <p style="background-color:Tomato;">China is a great country!</p>
> ```
> 实现效果：
> <h1 style="background-color:DodgerBlue;">China</h1>
> <p style="background-color:Tomato;">China is a great country!</p>

> [!example] 文本色
> ```html
> <h1 style="color:Tomato;">China</h1>
> <p style="color:DodgerBlue;">China is a great country!</p>
> <p style="color:MediumSeaGreen;">China, officially the People's Republic of China...</p>
> ```
> 实现效果：
> <h1 style="color:Tomato;">China</h1>
> <p style="color:DodgerBlue;">China is a great country!</p>
> <p style="color:MediumSeaGreen;">China, officially the People's Republic of China...</p>

> [!example] 边框色
> ```html
> <h1 style="border:2px solid Tomato;">Hello World</h1>
> <h1 style="border:2px solid DodgerBlue;">Hello World</h1>
> <h1 style="border:2px solid Violet;">Hello World</h1>
> ```
> 实现效果：
> <h1 style="border:2px solid Tomato;">Hello World</h1>
> <h1 style="border:2px solid DodgerBlue;">Hello World</h1>
> <h1 style="border:2px solid Violet;">Hello World</h1>

### 其它颜色

> [!example] 其它颜色
> ```html
> <h1 style="background-color:rgb(255, 99, 71);">...</h1>
> <h1 style="background-color:#ff6347;">...</h1>
> <h1 style="background-color:hsl(9, 100%, 64%);">...</h1>
> <h1 style="background-color:rgba(255, 99, 71, 0.5);">...</h1>
> <h1 style="background-color:hsla(9, 100%, 64%, 0.5);">...</h1>
> ```
> 实现效果：
> <h1 style="background-color:rgb(255, 99, 71);">...</h1>
> <h1 style="background-color:#ff6347;">...</h1>
> <h1 style="background-color:hsl(9, 100%, 64%);">...</h1>
> <h1 style="background-color:rgba(255, 99, 71, 0.5);">...</h1>
> <h1 style="background-color:hsla(9, 100%, 64%, 0.5);">...</h1>
