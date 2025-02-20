---
tags:
  - Sites
icon: SiMdnwebdocs
---

# MkDocs

## Introduction

### MkDocs

> [MkDocs](https://www.mkdocs.org/) is a fast, simple and downright gorgeous static site generator that’s geared towards building project documentation. Documentation source files are written in Markdown, and configured with a single YAML configuration file.

*MkDocs*是基于Markdown文档和YAML格式配置文件的快速静态站点生成器，主要用于构建项目文档，可以将Markdown文档转化为静态网站

*MkDocs*采用Python编写，使用[Python-Markdown](https://python-markdown.github.io/kk)解析Markdown文件，使用[Pygments](https://pygments.org/)语法高亮代码，使用[Jinja](https://jinja.palletsprojects.com/)模板引擎渲染Markdown文件

### MkDocs-Material

> [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) is a powerful documentation framework on top of MkDocs, a static site generator for project documentation.

MkDocs-Material是一个基于MkDocs的主题框架，它提供了一个简洁、现代的界面，支持多种语言，支持多种配置，并集成了一系列实用插件 👍

> [!info]- MkDocs-Material的[设计理念](https://squidfunk.github.io/mkdocs-material/philosophy/)
> * *It's just Markdown*: 你只需要了解Markdown语法和专注于你的内容，而无需关注其它的复杂事，让MkDocs-Material帮你完成HTML && CSS && JavaScript的工作
> * *Works on all devices*: 项目结构和文档在不同设备上自动调整，并完美适配各种类型和尺寸的显示屏
> * *Made to measure*: 优良的可扩展性，提供丰富的配置选择和行为表现
> * *Fast and lightweight*: 上层的性能表现，绝佳的用户反馈
> * *Accessible*: 各种设备都可以使用
> * *Open Source*: 开源，[MIT协议](https://squidfunk.github.io/mkdocs-material/license/)

---

## Installation

MkDocs由Python编写，因此使用Python包作为安装单元

> [!tip] 官方建议
> 推荐使用虚拟环境进行包管理

通过`pip`进行安装

```shell
pip install mkdocs
pip install mkdocs-material # Themes
```

使用如下命令查看是否安装完成：

```shell
mkdocs --version
```

显示版本号则安装成功，否则可以使用如下命令：

```shell
python3 -m mkdocs --version
```

将Python目录下的`Scripts`加入到环境变量`PATH`中即可实现上一条

```shell title:"Linux下支持的安装方式"
sudo apt install mkdocs
```

---

## Usage

### 创建站点

```shell title:"创建站点"
mkdocs new <dir_name>
```

*MkDocs* 在目标目录下创建项目文件夹

```shell title:"项目结构"
.
├── docs # 文档文件夹
│   └── index.md # 首页
└── mkdocs.yml # 配置文件
```

### 本地渲染

```shell
mkdocs serve
```

打开浏览器，输入`http://127.0.0.1:8000`即可本地访问站点

在运行站点的同时，也可以修改站点信息，都可以实时在浏览器显示更改

### 构建站点

经过构建之后就可以看到渲染

```shell
mkdocs build
```

构建完成后会出现一个新的目录`site/`

> [!tip] 使用建议
> 对于[远程部署](MkDocs.md#远程部署)来说，最好只上传原始的Markdown文档到服务器，然后在服务器端进行构建生成站点，因为MkDocs的版本变化可能导致本地构建的站点上传至服务器时无法正常显示
> ```gitignore
> site/
> ```

### 远程部署

为了能在远程看到站点渲染，需要进行远程部署

#### GitHub

通常来说，使用[GitHub Pages](https://pages.github.com/)构建是最为常见的

##### 项目站点

```shell
mkdocs gh-deploy
```

执行上述命令后*MkDocs*会

1. 创建远程分支`gh-pages`
2. 执行`mkdocs build`命令
3. 将当前目录下的`site/`中的内容推送到远程分支

> [!tip] 使用建议
> 可以在主分支上上传项目源文档，利用`gh-pages`分支上传构建后的站点文档

##### 用户站点

```shell
# 假设项目结构如下
.
├── Project
│   ├── docs
│   │   └── index.md
│   └── mkdocs.yml
└── User.github.io
```

则配置用户站点的方法：

```shell
cd User.github.io
mkdocs gh-deploy --config-file ../Project/mkdocs.yml --remote-branch master
```

这里需要显式指出配置文件的位置和推送分支

> [!note] 注意
 如果不指明推送分支，默认推送分支为`master`

---

## Configuration

进入`mkdocs.yml`进行站点配置

> [!warning] 注意
> 以下配置仅对主题MkDocs-Material有效，不同主题支持的配置不同，详见官方文档

### 站点信息

站点名是唯一的硬性配置要求

```yaml title:"mkdocs.yml"
# 项目信息
site_name: <site_name> # 唯一配置要求
site_url: https://example.com
site_description: >-
	an example website
site_author: Author_Name
site_dir: site # 文档存放位置
site_favicon: images/favicon.ico

# 代码仓库信息
repo_name: Your_Repo_Name
repo_url: Your_Repo_URL

# [版权信息](MkDocs.md#5.1.8%20底栏)
copyright: Copyright Info
```

### 导航栏

使用导航栏（*Navigator*）显示所有添加的页面

支持添加多级页面

所有的文档位置都采用与`docs/`的相对位置

```yaml title:"mkdocs.yml"
nav:
  - HOME: index.md
  - ABOUT: about.md
  - PAGES:
	- Page1: pages/page1.md
    - Page2:
      - Page2-1: pages/page2/page2-1.md
      - Page2-2: pages/page2/page2-2.md
    - Page3: pages/page3.md
  - License: License.md
  - Release Notes: notes.md
  - Blog:
	  - blog.index.md
```
> [!warning] 注意
> 添加的`.md`文档不应该以`.`开头，否则会被*MkDocs*忽略导致添加失败，如`.foo.md`

### 首页

通常根据习惯，浏览器都会把首页返回为一个index文件，所以*MkDocs*也采用`index.md`作为首页

然而有些展示仓库的网站会对README文件有特殊的展示（如GitHub），因此在*MkDocs*中如果把首页命名为`README.md`也会被其识别，但是在经过远程部署之后会被转化成`index.html`使得浏览器能够正确识别

> [!warning] 注意
> 如果`index.md`和`README.md`同时存在，则`README.md`会被忽略

### TOC

#### 标题链接

让每个标题带上链接

默认为`permalink=false`，即不显示

启用为`true`后显示`¶`，也可以改成其它字符如`#`

```yaml
markdown_extensions:
  - toc:
      permalink: true
```

#### 基准标题

默认情况下基准标题为`1`

更改配置使得基准标题变化

```yaml
markdown_extensions:
  - toc:
      baselevel: 2
      toc_depth: 3
```

#### 分词符

分词符为使用了标题链接后显示的ID中的分隔符

默认情况下分词符为`-`

```yaml
markdown_extensions:
  - toc:
      seperator: '_'
```

### 提示块

提示块扩展(*Admonition*, *Call-out*)非常好用

```yaml title:"mkdocs.yml"
markdown_extensions:
	- admonition
	- pymdownx.details # 提示块可折叠
	- pymdownx.superfences # 提示块可嵌套
```

#### 用法

```Markdown
<!-- 基本用法 -->
!!! AdmonitionType
	Your Text Here

<!-- 添加标题 -->
!!! AdmonitionType "YourTitleHere"
	Your Text Here

<!-- 去除标题 -->
!!! AdmonitionType ""
	Your Text Here

<!-- 可折叠块：默认折叠 -->
??? AdmonitionType
	Your Text Here
	
<!-- 可折叠块：默认打开 -->
???+ AdmonitionType
	Your Text Here

<!-- 行内提示块：左侧 -->
!!! AdmonitionType inline "YourTitleHere"
	Your Text Here

<!-- 行内提示块：右侧 -->
!!! AdmonitionType inline end "YourTitleHere"
	Your Text Here
```

#### 支持类型

> [!note]

> [!abstract]

> [!info]

> [!tip]

> [!success]

> [!question]

> [!warning]

> [!failure]

> [!danger]

> [!bug]

> [!example]

> [!quote]

### 代码块

#### 配置

```yaml title:"mkdocs.yml"
markdown_extensions:
	- pymdownx.highlight:
		# 行号
		linenums: true
		# 每行行号生成锚点链接
		anchor_linenums: true
		line_spans: __span
	- pymdownx.inlinehilite
	- pymdownx.snippets # 从其它文件嵌入页面
	- pymdownx.superfences # 代码块嵌套
theme:
	name: material
	features:
		- content.code.copy # 代码块复制按键
```

#### 用法

``````Markdown
<!-- Code Block -->
``` Language CodeBlockAttributes
Your code here
```

<!-- Inline Code -->
`#!Language YourCodeHere`
``````

|  用法   |                            属性                            |
| :---: | :------------------------------------------------------: |
| 添加标题  |                   `title="YourTitle"`                    |
| 起始行号  |                     `linenums="Num"`                     |
| 高亮特定行 | `hl_lines="line1 line2 ..."`<br>`hl_lines="line1-lineN"` |

### 列表

> [!note] 注意
> 多级列表需要4个空格才能识别

```yaml title:"mkdocs.yml"
markdown_extensions:
	# 定义列表
	- def_list
	# 任务列表
	- pymdownx.tasklist:
		custom_checkbox: true
```

> [!example]- 定义列表样例
> 用于下定义
> ```Markdown
> `Lorem ipsum dolor sit amet`
> 
> :   Sed sagittis eleifend rutrum. Donec vitae suscipit est. Nullam tempus
>     tellus non sem sollicitudin, quis rutrum leo facilisis.
> 
> `Cras arcu libero`
> 
> :   Aliquam metus eros, pretium sed nulla venenatis, faucibus auctor ex. Proin
>     ut eros sed sapien ullamcorper consequat. Nunc ligula ante.
> 
>     Duis mollis est eget nibh volutpat, fermentum aliquet dui mollis.
>     Nam vulputate tincidunt fringilla.
>     Nullam dignissim ultrices urna non auctor.
> ```

> [!example]- 任务列表样例
> ```Markdown
> - [x] Lorem ipsum dolor sit amet, consectetur adipiscing elit
> - [ ] Vestibulum convallis sit amet nisi a tincidunt
>     * [x] In hac habitasse platea dictumst
>     * [x] In scelerisque nibh non dolor mollis congue sed et metus
>     * [ ] Praesent sed risus massa
> - [ ] Aenean pretium efficitur erat, donec pharetra, ligula non scelerisque
> ```
> 效果如下：
> - [x] Lorem ipsum dolor sit amet, consectetur adipiscing elit
> - [ ] Vestibulum convallis sit amet nisi a tincidunt
>     * [x] In hac habitasse platea dictumst
>     * [x] In scelerisque nibh non dolor mollis congue sed et metus
>     * [ ] Praesent sed risus massa
> - [ ] Aenean pretium efficitur erat, donec pharetra, ligula non scelerisque

### Tabs

#### 配置

```yaml title:"mkdocs.yml"
markdown_extension:
	- pymdownx.superfences
	- pymdownx.tabbed:
		alternate_style: true
		# 生成tab链接
		slugify: !!python/object/apply:pymdownx.slugs.slugify
			kwds:
				case: lower
```

#### 用法

``````Markdown
=== "YourTitleHere"
	Your Text Here

<!-- 样例 -->
=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```
``````

### 表格

```yaml title:"mkdocs.yml"
markdown_extension:
	- tables
```

### Mermaid

```yaml title:"mkdocs.yml"
markdown_extensions:
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
```

> [!info] 支持的类型
> [Diagrams - Material for MkDocs](https://squidfunk.github.io/mkdocs-material/reference/diagrams/)

### 脚注

```yaml title:"mkdocs.yml"
markdown_extensions:
	- footnotes
```

### 数学公式

> [!info] 完整信息
> [Math - Material for MkDocs](https://squidfunk.github.io/mkdocs-material/reference/math/)

通常来说使用[KaTex](https://katex.org/)就足够站点使用

首先创建文件`docs/javascripts/katex.js`

```js title:"docs/javascripts/katex.js"
document$.subscribe(({ body }) => { 
  renderMathInElement(body, {
    delimiters: [
      { left: "$$",  right: "$$",  display: true },
      { left: "$",   right: "$",   display: false },
      { left: "\\(", right: "\\)", display: false },
      { left: "\\[", right: "\\]", display: true }
    ],
  })
})
```

然后在配置文件中添加

```yaml title:"mkdocs.yml"
markdown_extensions:
	- pymdownx.arithmatex:
		generic: true

extra_javascript:
	- javascripts/katex.js
	- https://unpkg.com/katex@0/dist/katex.min.js
	- https://unpkg.com/katex@0/dist/contrib/auto-render.min.js

extra_css:
	- https://unpkg.com/katex@0/dist/katex.min.css
```

### 格式化

[格式化](https://squidfunk.github.io/mkdocs-material/reference/formatting/#usage)支持文本的高亮、删除线、替换、上下标等

```yaml title:"mkdocs.yml"
markdown_extensions:
  - pymdownx.critic
  - pymdownx.caret
  - pymdownx.keys
  - pymdownx.mark
  - pymdownx.tilde
```

> [!example] 格式化样例
> ```Markdown
> <!-- 高亮文本变化 -->
> Text can be {--deleted--} and replacement text {++added++}. This can also be
combined into {~~one~>a single~~} operation. {==Highlighting==} is also
possible {>>and comments can be added inline<<}.
> 
> {==
> 
> Formatting can also be applied to blocks by putting the opening and closing tags on separate lines and adding new lines between the tags and the content.
> 
> ==}
> 
> <!-- 高亮文本 -->
> - ==This was marked (highlight)==
> - ^^This was inserted (underline)^^
> - ~~This was deleted (strikethrough)~~
> a
> <!-- 上下标 -->
> - H~2~O
> - A^T^A
> 
> <!-- 键盘 -->
> ++ctrl+alt+del++
> ```

### Emoji

```yaml title:"mkdocs.yml"
markdown_extensions:
  - attr_list
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
```

### 小提示

 可以给文本添加小提示，如缩略语等

> [!info] 完整信息
> [Tooltips - Material for MkDocs](https://squidfunk.github.io/mkdocs-material/reference/tooltips/)

### 站内跳转

使用Markdown的链接格式进行站内跳转

```markdown
[AltText](RelativePath/To/ThisFile)
[AltText](RelativePath/To/ThisFile#Section)
```

### 图片与媒体

把所有图片放到`docs/img/`下，并在你的文档中使用相对于`docs/`的路径使用图片链接格式即可

> [!info] 完整信息
> [Images - Material for MkDocs](https://squidfunk.github.io/mkdocs-material/reference/images/)

---

## Themes

> [!info] 默认配置
> *MkDocs*默认主题：`mkdocs`和`readthedoc`

> [!info] 第三方主题库
> [MkDocs Themes · mkdocs/mkdocs Wiki](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes)

### MkDocs-Material

#### 配色

##### 主题配色

MkDocs-Material支持2种颜色主题

|    属性值    | 内容   |
| :-------: | ---- |
| `default` | 亮色主题 |
|  `slate`  | 暗色主题 |

```yaml title:"mkdocs.yml"
theme:
	name: material
	palette:
		scheme: default
```

##### 内容配色

* 基础配色(*Primary Color*)用于在文档头、侧边栏、文本链接等显示
* 口音配色(*Accent Color*)用于在链接、按钮、滚动条等显示

```yaml title:"mkdocs.yml"
theme:
	name: material
	palette:
		primary: indigo
		accent: pink
```

> [!info]- 支持的颜色
> ```txt
> red
> pink
> purple
> deep purple
> indigo -> 默认值
> blue
> light blue
> cyan
> teal
> green
> light green
> lime
> yellow
> amber
> orange
> deep orange
> brown -> 仅限primary
> grey -> 仅限primary
> blue grey -> 仅限primary
> black -> 仅限primary
> white -> 仅限primary
> ```

##### 配色切换

可以让用户决定主题配色切换

```yaml title:"mkdocs.yml"
theme:
	name: material
	palette:
		# 亮色主题
		- scheme: default
		  primary: pink
		  accent: blue
		  toggle:
			icon: material/brightness-7
			name: Switch to dark mode
		# 暗色主题
		- scheme: slate
		  primary: red
		  accent: indigo
		  toggle:
			icon: material/brightness-4
			name: Switch to light mode	
```

其中，`icon`必须指向有效的图标路径

> [!info] 常用`icon`
> ```txt
> material/brightness-7  + material/brightness-4
> material/toggle-switch + material/toggle-switch-off-outline
> material/weather-night + material/weather-sunny
> material/eye           + material/eye-outline
> material/lightbulb     + material/lightbulb-outline
> ```

###### 跟随系统

与系统配色保持一致的配色风格

只需要在`scheme`属性添加`media`属性

```yaml title:"mkdocs.yml"
theme:
	name: material
	palette:
		# 亮色主题
	    - media: "(prefers-color-scheme: light)"
		  scheme: default
		  primary: green
		  accent: yellow
	      toggle:
			icon: material/brightness-7
		    name: Switch to dark mode
		# 暗色主题
		- media: "(prefers-color-scheme: dark)"
	      scheme: slate
		  primary: pink
		  accent: blue
	      toggle:
		    icon: material/brightness-4
	        name: Switch to light mode
```

###### 自动切换

一些新的操作系统允许在白天和夜晚自动切换配色

```yaml title:"mkdocs.yml"
theme:
	name: material
	palette:
		# 自动模式
		- media: "(prefers-color-scheme)"
		  toggle:
			icon: material/brightness-auto
			name: Switch to light mode
		# 亮色主题
	    - media: "(prefers-color-scheme: light)"
		  scheme: default
	      toggle:
			icon: material/brightness-7
	        name: Switch to dark mode
		# 暗色主题
		- media: "(prefers-color-scheme: dark)"
	      scheme: slate
	      toggle:
			icon: material/brightness-4
	        name: Switch to system preference
```

##### 自定义配色

> [!info] 完整内容
> [自定义配色](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/#customization)

#### 字体

> [!tip] 通常来说字体不用调

```yaml title:"mkdocs.yml"
theme:
	name: material
	font:
		text: Roboto
		code: Roboto Mono
```

> [!info] 完整信息
> [字体](https://squidfunk.github.io/mkdocs-material/setup/changing-the-fonts/#changing-the-fonts)

#### 语言

在配置文件中进行修改

```yaml title:"mkdocs.yml"
theme:
	name: material
	language: en
```

> [!tip] 常用的语言
> ```txt
> zh
> zh-TW
> zh-Hant
> en
> ```

> [!info] 完整信息
> [支持的语言和语言切换器](https://squidfunk.github.io/mkdocs-material/setup/changing-the-language/#changing-the-language)

#### 图标

##### logo

logo显示在侧边栏上

```yaml title:"mkdocs.yml"
theme:
	name: material
	logo: PathToYourLogoFile
```

##### 站点图标

站点图标显示在浏览器标签页上

```yaml title:"mkdocs.yml"
theme:
	name: material
	favicon: PathToYourIconFile
```

> [!info]- MkDocs原生图标支持
> *MkDocs*原生主题默认使用[MkDocs favicon](https://www.mkdocs.org/img/favicon.ico)图标，如果使用自定义图标，在`docs/`下创建子目录`img`，并将图标文件`.ico`放入其中即可，无需修改任何配置

##### 其它图标

```yaml title:"mkdocs.yml"
theme:
	name: material
	icon:
		logo: ... # 同[logo](MkDocs.md#logo)
		menu: ...
		alternate: ...
		search: ...
		share: ...
		close: ...
		top: ...
		edit: ... # 编辑此页图标
		view: ... # 查看页面源码图标
		repo: ... # 仓库图标
		admonition: ... # 提示块图标
		tag: ...
		previous: ... # 上一页
		next: ... # 下一页
```

> [!example] 图标实例
> > [!info]- 仓库图标
> > [完整信息](https://squidfunk.github.io/mkdocs-material/setup/adding-a-git-repository/#repository-icon)
> > ```txt title:"Material-RepoIcon"
> > fontawesome/brands/git
> > fontawesome/brands/git-alt
> > fontawesome/brands/github
> > fontawesome/brands/github-alt
> > fontawesome/brands/gitlab
> > fontawesome/brands/trash
> > ```
>  
> > [!info]- 代码图标
> > ```txt title:"Material-CodeAction"
> > material/pencil
> > material/eye
> > ```
>
> > [!info]- 提示块图标
> > [Admonition Icons](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#admonition-icons)

> [!info]- 自定义图标
> [自定义](https://squidfunk.github.io/mkdocs-material/setup/changing-the-logo-and-icons/#customization)

#### 数据隐私

> [!info] 完整信息
> [数据隐私](https://squidfunk.github.io/mkdocs-material/setup/ensuring-data-privacy/#ensuring-data-privacy)

#### 导航栏

##### 加载项

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.instant # 即时加载
		- navigation.instant.prefetch # 预加载
		- navigation.instant.progress # 显示进度条
```

> [!important] 提示
> 使用即时加载的相关项时必须使用`site_url`属性

> [!bug] 说明
> 参考[[工欲善其事，必先利其器] - 搭建技术博客/个人主页 - 使用MkDocs和Material - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/672743170)，即时加载最好去掉，因为在多语言切换中这个加载项会导致切回首页

##### 锚点

实现导航栏跟踪

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.tracking
```

##### 菜单栏

实现页面顶部菜单栏功能

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.tabs
		- navigation.tabs.sticky # 是否始终显示
```

##### 章节显示

显示每个页面的每个章节

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.sections # 是否显示章节
		- navigation.expand # 是否压缩显示
```

##### 路径

> [!info] 内测版专属

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.path
```

##### 项目优化

构建只可见的页面，可以优化导航栏

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.prune
```

##### 首页信息

是否单独显示首页

> [!tip] 为了能链接至某章节的某个页，在这个章节中添加这个章节的首页

```yaml title:"mkdocs.yml"
nav:
	- SectionName:
		- SectionFolder/index.md
		- Page 1: section/page-1.md
		- Page 2: section/page-2.md
		...
theme:
	name: material
	features:
		- navigation.indexes
```

##### TOC

TOC在这里指页面右侧的小目录

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- toc.follow # 右侧边栏的目录跟踪
		- toc.integrate # 目录跟踪集成到左侧边栏目录中
```

##### 回到顶部

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.top
```

##### 隐藏

可以在某些页面中隐藏导航栏和/或TOC，只需要在页面的元数据YAML中添加即可

```Markdown title:"page.md"
---
hide:
	- navigation
	- toc
---

# Page title

...
```

#### 顶栏

顶栏可以添加公告、防止仓库地址、搜索栏等等

```yaml title:"mkdocs.yml"
theme:
	name: material
	feature:
		- header.autohide # 自动隐藏顶栏
		- announce.dismiss # 可以叉掉公告
```

#### 底栏

底栏可以放置网站链接、社交频道、版权信息等等

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- navigation.footer
		...
extra:
	# 社交链接
	social:
		- icon: IconFile1
		  link: YourSocialLink1
		- icon: IconFile2
		  link: YourSocialLink2
		...
		- icon: IconFileN # 邮箱
		  link: mailto:<YourEmailAddress>
	# 有关MkDocs-Material的生成器
	generator: false
# 版权信息
copyright: >-
	Copyright &copy; 2024 YourName
```

> [!info] 社交链接图标
> > [!warning] 注意
> > 社交链接图标必须是与主题绑定的图标
> ```txt title:"Material-SocialIcons"
> fontawesome/brands/github
> fontawesome/brands/gitlab
> fontawesome/brands/x-twitter
> fontawesome/brands/docker
> fontawesome/brands/facebook
> fontawesome/brands/instagram
> fontawesome/brands/linkedin
> fontawesome/brands/slack
> fontawesome/brands/discord
> ```
> [icon-完整信息](https://squidfunk.github.io/mkdocs-material/setup/setting-up-the-footer/#+social.icon)

> [!tip] 技巧
> 可以在`Copyright`中添加HTML链接

如果要在某页中隐藏底栏，在页顶的YAML中设置

```Markdown title:"page.md"
---
hide:
	- footer
---

...
```

#### 评论区

> [!info] 完整信息
> [评论系统](https://squidfunk.github.io/mkdocs-material/setup/adding-a-comment-system/)

#### 页面源码

```yaml title:"mkdocs.yml"
theme:
	name: material
	features:
		- content.action.view # 显示页面文档源码
		- content.action.edit # 编辑此页
```

---

## Plugins

这里介绍一些实用的插件，**仅限MkDocs-Material主题**

> [!info] 所有插件
> ([Built-in plugins - Material for MkDocs](https://squidfunk.github.io/mkdocs-material/plugins/))

### Search

用于全站搜索信息使用

> MkDocs-Material的搜索功能非常完善，不需要借助任何第三方工具，甚至可以离线工作

```yaml title:"mkdocs.yml"
plugins:
	- search # 配置搜索插件
	...
theme:
	name: material
	features:
		- search.suggest # 搜索建议
		- search.highlight # 搜索高亮
		- search.share # 搜索分享
```

> [!tip] 逐出搜索范围
> 如果想要某些页面不被搜索到，可以在页面头的YAML中配置：
> ```Markdown title:"page.md"
> ---
> search:
> 	exclude: true
> ---
> 
> # Page title
> ...
> ```

### Blog

> Blogs are a great way to engage with your audience. Software developers can use a blog to announce new features, demonstrate their usage and provide background information.

Blog主要以时间线为线索，类似于日记

完成配置后Blogs根据创建时间顺序倒序排列显示

#### 配置

在配置文件中应用`blog`插件

```yaml title:"mkdocs.yml"
plugins:
	- blog
```

#### 使用

完成配置后，运行`mkdocs serve`以创建Blog

```shell title:"创建Blog后的文件结构"
.
├── docs
│   ├── blog # Blog文件夹
│   │   ├── index.md # Blog首页
│   │   └── posts # 存放Blog的文件夹
│   └── index.md
└── mkdocs.yml
```

之后将所有的blogs都放在`docs/blog/posts`下

Blog首页会包含所有的Blog及其摘要

#### 元数据

创建的Blog必须有一个页头，将Blog的元数据（如创建日期）写在Markdown文件头部的Yaml中

> [!warning] 必需条件
> 元数据中的创建日期`created`是唯一的Blog条件，必需写入
 
```Markdown title:"Blogs.md"
---
date:
	created: 2024-07-23 <!-- 创建日期，必需 -->
	updated: 2024-07-24 <!-- 更新日期 -->
readtime: 20 <!-- 阅读时间，不添加会自动生成 -->
pin: true <!-- 是否置顶 -->
draft: true <!-- 是否存入草稿而不发布 -->
categories: <!-- 日记类别 -->
	- CategoryName
links: <!-- 链接 -->
	- HomePage: index.md
	- Blog Index: blog/index.md
	- External links: <!-- 外部链接 -->
		- YourLinkName: https://example.com
---
 
# My Blog
 
...

<!-- more --> <!-- 摘要分栏 -->

...
```

* 作为草稿的Blog在构建站点时不会被构建，在本地渲染时可以看到一个`draft`标签
* Blog可以进行分类，使用`categories`属性标记类别
* 在Yaml部分到`<!-- more -->`之间的内容会作为Blog的摘要显示在首页和归档中，而后续内容则不会显示

#### 导航栏配置

完成Blog配置后搭配导航栏以显示blogs

```yaml title:"mkdocs.yml"
nav:
	...
	- Blog:
		- blog/index.md
```

> [!tip] 使用建议
> 为了避免Blog首页重复显示，可以在配置文件中这样修改
> ```yaml title:"mkdocs.yml"
> theme:
> 	name: material
> 	features:
> 		- navigation.indexes
> ```

#### 归档

Blogs默认情况下按照年份进行归档整理

也可以自定义设置归档格式

```yaml title:"mkdocs.yml"
plugins:
	- blog:
		archive_date_format: MMMM yyyy # y年M月d日
```

#### 分类

在[元数据](MkDocs.md#元数据)中提到使用分类(*Categories*)的方法对Blogs进行分类

可以在配置文件中修改允许显示的类别

```yaml title:"mkdocs.yml"
plugins:
	- blog:
		categories_allowed:
			- Categories 0
			- Categories 1
			...
```

#### 作者

创建文件`docs/blog/.authors.yml`来标明Blogs的作者

```yaml title:".authors.yml"
authors:
	Author1: # 作者ID
		name: YourName # 作者名
		description: YourDescription # 作者描述
		avatar: YourURL # 作者图标
	Author2:
		...
```

添加完作者配置文件后在Blogs头部的元数据中添加作者

```Markdown title:"Blog.md"
---
date:
	created: ...
authors:
	- Author1
	...
---

...
```

即可在Blogs中查看作者的信息

> [!info]- 添加作者个人主页
> MkDocs-Material内测版允许你在你的站点[添加作者个人主页](https://squidfunk.github.io/mkdocs-material/tutorials/blogs/navigation/#defining-authors)，完成赞助以启用这个功能

#### 分页

Blogs数量过多时可以使用分页(*Pagination*)功能

默认情况下的Blog分页为10Blogs每页

```yaml title:"mkdocs.yml"
plugins:
	- blog:
		...
		pagination_per_page: 10
		archive_pagination_per_page: 5
		categories_pagination_per_page: 5
```

#### 目录

当Blogs数量过多，可以启用Blogs目录完成信息分拣

```yaml title:"mkdocs.yml"
plugins:
	- blog:
		blog_toc: true
```

### 标签

标签也可以用来对文章分类

```yaml title:"mkdocs.yml"
plugins:
	- tags
```

#### 添加标签

在每页的头部YAML中加入tag

```Markdown title:"page.md"
---
tags:
	- YourTag1
	- YourTag2
	...
---

...
```

#### 隐藏标签

只需要在头部YAML中设置

```Markdown title:"page.md"
---
hide:
	- tags
---

...
```

#### 标签图标

在配置文件中设置标签的标识符和图标

```yaml title:"mkdocs.yml"
theme:
	name: material
	icon:
		tags:
			YourTagIdentifier1: YourTagIconFile1
			YourTagIdentifier2: YourTagIconFile2
			...
extra:
	tags:
		YourTag1: YourTagIdentifier1
		YourTag2: YourTagIdentifier2
		...
```

> [!info] Material主题提供的标签
> [Tag-Icon](https://squidfunk.github.io/mkdocs-material/setup/setting-up-tags/#__code_2_annotation_1)

#### 标签索引页

可以专门做一个标签索引页来记录所有的标签及其对应的页面

> [!important] 注意
> 记得把这个标签索引页加入导航栏中

然后在索引页中添加

```Markdown title:"tags.md"
# Tags

...

<!-- material/tags -->

...
```

标签插件会在`{Markdown}<!-- material/tags -->`位置插入标签索引页的内容

然后在配置文件中修改配置为

```yaml title:"mkdocs.yml"
plugins:
	- tags:
		tags_file: tags.md
```

> [!note] 注意
> 添加的文件路径为相对于`docs/`

### 社交卡片

> [!info] 完整信息
> [社交卡片](https://squidfunk.github.io/mkdocs-material/setup/setting-up-social-cards/#setting-up-social-cards)

> [!bug] 说明
> 参考[[工欲善其事，必先利其器] - 搭建技术博客/个人主页 - 使用MkDocs和Material - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/672743170)，社交卡片有一些bug，会导致报错

### RSS

> [!info] 完整信息
> [RSS](https://squidfunk.github.io/mkdocs-material/setup/setting-up-a-blog/#rss)

---

## Extra

### 站点分析工具

> [!info] 完整信息
> [Analytics](https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-analytics/#setting-up-site-analytics)

### 版本工具

有时候一个产品或站点有不同的版本，因此使用版本工具可以查看不同版本对应的站点内容

> [!info] 完整信息
> [Versioning](https://squidfunk.github.io/mkdocs-material/setup/setting-up-versioning/)

### 状态

可以设置文章的状态

> [!example] Material主题预定义的状态
> * 文章是最新添加的 `new`
> * 文章是过时弃用的 `deprecated`

先在配置文件中设置

```yaml title:"mkdocs.yml"
extra:
	status:
		<identifier>: <description>
```

然后在对应页面的头部YAML中描述

```Markdown title:"page.md"
---
status: <identifier>
---

...
```

---

## References

> [!tip]- 参考配置
> 参考[知乎](https://zhuanlan.zhihu.com/p/672743170)文章中的一份比较完整和实用的配置文件：
> ```yaml title:"mkdocs.yml"
> # 项目信息
> site_name: Eureka! # 项目名称
> site_url: https://localhost:8000/ # 我在nginx中使用的是8000端口，如果你使用的是80端口，可以直接写成https://localhost/。
> site_author: Shuaiwen Cui # 作者
> site_description: >- # 项目描述
>   Welcome to Shaun's rabbit hole. This site serves as a personal knowledge base for me to record my thoughts and ideas. It is also a place for me to share my knowledge and experience with the world. I hope you find something useful here. 
> 
> # 代码仓库信息
> repo_name: Shuaiwen-Cui/Infinity # 仓库名称
> repo_url: https://github.com/Shuaiwen-Cui/Infinity.git/ # 仓库地址
> 
> # 版权信息
> copyright: Copyright &copy; 2023 ~ now |   Shuaiwen Cui (Shaun)
> 
> # 配置
> theme:
>   custom_dir: material/overrides # 自定义文件夹，对于个别页面，如果你不想使用主题的默认样式，可以在这里进行修改，使用里面的文件覆盖主题的默认文件。具体可以参考material官方文档。
>   name: material # 主题名称，Material已经是最优秀的选择了，相信我。
>   logo: static/images/logo.png # logo 图片
>   language: en # 默认语言
>   features: # 功能  
>     - announce.dismiss # 可以叉掉公告的功能
>     - content.action.edit # 编辑按钮，似乎没啥用
>     - content.action.view # 查看按钮，似乎没啥用
>     - content.code.annotate # 代码注释，具体不清楚
>     - content.code.copy # 复制代码按钮
>     # - content.code.select # 选择代码按钮
>     # - content.tabs.link # 链接标签
>     - content.tooltips # 不太清楚呢这个
>     # - header.autohide # 自动隐藏header
>     - navigation.expand # 默认展开导航栏
>     - navigation.footer # 底部导航栏
>     - navigation.indexes # 索引按钮可以直接触发文件，而不是只能点击其下属选项浏览，这个功能可以给对应的section提供很好的预览和导航功能
>     # - navigation.instant # 瞬间加载 最好注释掉，多语言切换这个会导致跳回首页
>     - navigation.instant.prefetch # 预加载
>     - navigation.instant.progress # 进度条
>     - navigation.path # 导航路径， 目前好像没啥用
>     # - navigation.prune # 只构建可见的页面
>     - navigation.sections # 导航栏的section
>     - navigation.tabs # 顶级索引被作为tab
>     - navigation.tabs.sticky # tab始终可见
>     - navigation.top # 开启顶部导航栏
>     - navigation.tracking # 导航栏跟踪
>     - search.highlight # 搜索高亮
>     - search.share # 搜索分享
>     - search.suggest # 搜索建议
>     - toc.follow # 目录跟踪-页面右侧的小目录
>     # - toc.integrate # 目录跟踪集成到左侧大目录中
>   palette:
>     - media: "(prefers-color-scheme)" # 主题颜色
>       scheme: slate
>       primary: black
>       accent: indigo
>       toggle:
>         icon: material/link
>         name: Switch to light mode
>     - media: "(prefers-color-scheme: light)" # 浅色
>       scheme: default
>       primary: indigo
>       accent: indigo
>       toggle:
>         icon: material/toggle-switch
>         name: Switch to dark mode
>     - media: "(prefers-color-scheme: dark)" # 深色
>       scheme: slate
>       primary: black
>       accent: indigo
>       toggle:
>         icon: material/toggle-switch-off
>         name: Switch to system preference
>   font: # 字体，大概率不需要换
>     text: Roboto
>     code: Roboto Mono
>   favicon: assets/favicon.png # 网站图标 似乎不需要管
>   icon: # 一些用到的icon
>     logo: logo
>     previous: fontawesome/solid/angle-left
>     next: fontawesome/solid/angle-right
>     tag:
>       default-tag: fontawesome/solid/tag
>       hardware-tag: fontawesome/solid/microchip
>       software-tag: fontawesome/solid/laptop-code
> 
> # Plugins
> plugins:
>   - tags # 标签功能插件
>   - blog # 博客功能插件
>   - rss: # rss订阅插件 - 不太懂是干嘛的目前
>       match_path: blog/posts/.* 
>       date_from_meta:
>         as_creation: date
>       categories:
>         - categories
>         - tags 
>   # - social # 目前我开启会报错，还没研究透 
>   - search: # 搜索插件
>       separator: '[\s\u200b\-_,:!=\[\]()"`/]+|\.(?!\d)|&[lg]t;|(?!\b)(?=[A-Z][a-z])' # 分隔符
>   - minify: # 压缩插件
>       minify_html: true
>   # - privacy # 隐私插件
>   - i18n: # 多语言插件
>       docs_structure: suffix # 抄来的，不太懂
>       fallback_to_default: true # 抄来的，不太懂
>       reconfigure_material: true # 抄来的，不太懂
>       reconfigure_search: true # 抄来的，不太懂
>       languages: # 多语言配置 - 需要小心一点
>         - locale: en
>           default: true # 默认语言
>           name: English
>           build: true # 是否构建
>           # site_name: Infinity
>         - locale: zh
>           name: 简体中文
>           build: true
>           nav_translations: # 导航栏翻译，不可以有缩进
>             HOME: 首页
>             ABOUT: 关于
>             SPONSORSHIP: 赞助
>             CS: 计算机
>             CODING: 编程
>             EMBEDDED-SYS: 嵌入式系统
>             DSP: 数字信号处理
>             PERCEPTION: 感知
>             ACTUATION: 执行
>             IOT: 物联网
>             CLOUD: 云
>             CLOUD-TECH: 云技术
>             HANDS-ON: 上手实践
>             Have A Server: 拥有一台服务器
>             Server Configuration: 服务器配置
>             Get A Domain Name: 获得一个域名
>             AI: 人工智能
>             RESEARCH: 研究
>             PROJECT: 项目
> # Hooks - 讲真，这个东西我还没搞懂
> # hooks:
> #   - material/overrides/hooks/shortcodes.py
> #   - material/overrides/hooks/translations.py
> 
> # 额外配置项
> extra:
>   generator: false # 是否显示生成器
>   status: # 不是很懂有什么用
>     new: Recently added
>     deprecated: Deprecated
>   analytics: # 分析工具， 我反正没用到
>     provider: google
>     property: !ENV GOOGLE_ANALYTICS_KEY
>     feedback: # feedback form
>       title: Was this page helpful?
>       ratings:
>         - icon: material/thumb-up-outline
>           name: This page was helpful
>           data: 1
>           note: >-
>             Thanks for your feedback!
>         - icon: material/thumb-down-outline
>           name: This page could be improved
>           data: 0
>           note: >- 
>             Thanks for your feedback! Help us improve this page by
>             using our <a href="..." target="_blank" rel="noopener">feedback form</a>.
>   # alternate: # 由上面那个i18n插件提供的多语言功能，这个似乎就不需要了。 这个是官方文档的例子，但是官方没有提供很详细的例子，所以我也不知道怎么用。
>   #   - name: English
>   #     link: /en/ 
>   #     lang: en
>   #   - name: Chinese
>   #     link: /zh/
>   #     lang: zh
>   social: # 社交媒体
>     - icon: fontawesome/solid/house
>       link: http://www.cuishuaiwen.com/
>     - icon: fontawesome/brands/github
>       link: https://github.com/Shuaiwen-Cui
>     - icon: fontawesome/brands/linkedin
>       link: https://www.linkedin.com/in/shaun-shuaiwen-cui/
>     - icon: fontawesome/brands/researchgate
>       link: https://www.researchgate.net/profile/Shuaiwen-Cui
>     - icon: fontawesome/brands/orcid
>       link: https://orcid.org/0000-0003-4447-6687
>     - icon: fontawesome/brands/twitter
>       link: https://twitter.com/ShuaiwenC
>   tags: # 自定义标签
>     Default: default-tag
>     Hardware: hardware-tag
>     Software: software-tag
>   # consent: # 征求同意 Cookie
>   #   title: Cookie consent
>   #   description: >- 
>   #     We use cookies to recognize your repeated visits and preferences, as well
>   #     as to measure the effectiveness of our documentation and whether users
>   #     find what they're searching for. With your consent, you're helping us to
>   #     make our documentation better.
> 
> # 扩展
> markdown_extensions: # markdown extensions
>   - abbr
>   - admonition
>   - attr_list
>   - def_list
>   - footnotes
>   - md_in_html
>   - toc:
>       permalink: true
>   - pymdownx.arithmatex:
>       generic: true
>   - pymdownx.betterem:
>       smart_enable: all
>   - pymdownx.caret
>   - pymdownx.details
>   - pymdownx.emoji:
>       emoji_generator: !!python/name:material.extensions.emoji.to_svg
>       emoji_index: !!python/name:material.extensions.emoji.twemoji
>   - pymdownx.highlight:
>       anchor_linenums: true
>       line_spans: __span
>       pygments_lang_class: true
>   - pymdownx.inlinehilite
>   - pymdownx.keys
>   - pymdownx.magiclink:
>       normalize_issue_symbols: true
>       repo_url_shorthand: true
>       user: squidfunk
>       repo: mkdocs-material
>   - pymdownx.mark
>   - pymdownx.smartsymbols
>   - pymdownx.snippets:
>       auto_append:
>         - includes/mkdocs.md
>   - pymdownx.superfences:
>       custom_fences:
>         - name: mermaid
>           class: mermaid
>           format: !!python/name:pymdownx.superfences.fence_code_format
>   - pymdownx.tabbed:
>       alternate_style: true
>       combine_header_slug: true
>       slugify: !!python/object/apply:pymdownx.slugs.slugify
>         kwds:
>           case: lower
>   - pymdownx.tasklist:
>       custom_checkbox: true
>   - pymdownx.tilde
> 
> # 导航树 - 请按照我的做法来做，否则可能无法正常工作。引号可以省略。开头的点和斜杠也可以省略 ("./HOME/about.md" 或 Home/about.md) 。注意，导航树这里的文件名是 filename.md 这样的，但在文件夹中，它实际上被命名为 filename.en.md 和 filename.zh.md。我猜测默认是英文，所以, index.en.md 和 index.md 是一样的。i18n插件会自动识别文件名，然后根据文件名的后缀来切换语言。所以，如果你想添加一个新页面，你需要添加两个文件，一个是 filename.en.md，另一个是 filename.zh.md。其中，filename.en.md 也可以被命名为 filename.md，但是 filename.zh.md 不能被命名为 filename.md，否则会导致无法识别。
> nav: 
>   - HOME: 
>       - "index.md"
>       - ABOUT: "./HOME/about.md"
>       - SPONSORSHIP: "./HOME/sponsorship.md"
>   - CS: 
>       - "./CS/CS.md"
>   - CODING: 
>       - "./CODING/coding.md"
>   - EMBEDDED-SYS: 
>       - "./EMBEDDED-SYS/embedded-sys.md"
>   - DSP: 
>       - "./DSP/dsp.md"
>   - PERCEPTION: 
>       - "./PERCEPTION/perception.md"
>   - ACTUATION: 
>       - "./ACTUATION/actuation.md"
>       - ROS: "./ACTUATION/ROS/ros.md"
>   - IOT: 
>       - "./IOT/iot.md"
>   - CLOUD: 
>       - "./CLOUD/cloud.md"
>       - CLOUD-TECH: "./CLOUD/CLOUD-TECH/cloud-tech.md"
>       - HANDS-ON:
>           - Have A Server: "./CLOUD/HANDS-ON/001-HAVE-A-SERVER/have-a-server.md"
>           - Server Configuration: "./CLOUD/HANDS-ON/002-SERVER-CONFIG/server-config.md"
>           - Get A Domain Name: "./CLOUD/HANDS-ON/003-DOMAIN-NAME/domain-name.md"
>   - AI: 
>       - "./AI/ai.md"
>   - RESEARCH: 
>       - "./RESEARCH/research.md"
>   - PROJECT: 
>       - "./PROJECT/project.md"
>       - TECH-BLOG: "./PROJECT/TECH-BLOG/mkdocs_and_material.md"
> ```

> [!info]- 简洁的配置教程
> [so easy！从头教你用mkdocs构建个人博客系统~_mkdocs教程-CSDN博客](https://blog.csdn.net/qq_41261251/article/details/116021097?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522172153281616800188567960%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=172153281616800188567960&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-116021097-null-null.142^v100^pc_search_result_base7&utm_term=mkdocs&spm=1018.2226.3001.4187)
