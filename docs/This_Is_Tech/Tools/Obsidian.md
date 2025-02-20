---
tags:
- Markdown
---

# Obsidian 使用手册

!!! abstract "本节摘要"
    *Obsidian是一款注重隐私、灵活性高的笔记应用，能让你根据自己的思维方式记录笔记和写作*

!!! info "官方文档"
    - [Obsidian中文指南](https://publish.obsidian.md/help-zh/%E7%94%B1%E6%AD%A4%E5%BC%80%E5%A7%8B)
    - [Obsidian中文论坛](https://forum-zh.obsidian.md/)

---

## Basic

一些Obsidian基本用法和功能

### 代码块

* [支持语法高亮的语言](https://prismjs.com/#supported-languages)

### 标注

* [标注块](https://publish.obsidian.md/help-zh/%E7%BC%96%E8%BE%91%E4%B8%8E%E6%A0%BC%E5%BC%8F%E5%8C%96/%E6%A0%87%E6%B3%A8)

!!! info "支持的标注块类型"
    * [标注块类型](https://publish.obsidian.md/help-zh/%E7%BC%96%E8%BE%91%E4%B8%8E%E6%A0%BC%E5%BC%8F%E5%8C%96/%E6%A0%87%E6%B3%A8#%E6%94%AF%E6%8C%81%E7%9A%84%E7%B1%BB%E5%9E%8B)

### 注释

```markdown
%%注释内容%%

<!-- 注释内容 -->

%%
跨行注释
%%

<!--
跨行注释
-->
```

---

## Advanced

介绍一些实用的第三方插件

### Code Styler

!!! info "相关信息"
    - [在Obsidian插件市场查看](obsidian://show-plugin?id=code-styler)
    - [Github](https://github.com/mayurankv/Obsidian-Code-Styler)

这个插件能够用来把Obsidian中的行内代码和代码块更加美化

#### 注释链接

可以在注释中添加链接

```python title="Codeblock Comment Links"
# [Obsidian](https://obsidian.md/)
print("Hello")
```

#### 代码块

代码块第一行可以添加代码块参数

!!! note "注意"
    代码块参数的顺序可以是任意的

    ``````Markdown title="examples"
    ```cpp fold title:example_title
    ```
    
    ```cpp title=example_title fold
    ```
    
    <!-- No language set -->
    ``` fold title:example_title 
    ```
    
    ```{python title, hl=5}
    ```
    ``````

| 参数  |          符号          | 用法                                                                        | 说明                                 |
| :-: | :------------------: | ------------------------------------------------------------------------- | ---------------------------------- |
| 行号  |         `ln`         | 1. `ln:NUMBER`<br>2. `ln:false`                                           | 起始行号                               |
| 标题  |       `title`        | 1. `title:title`<br>2. `title:"long title"`<br>3. `title:'long title'`    | 标题含有空格的需要使用引号                      |
| 引用  | `reference`<br>`ref` | 1. `ref:[Obsidian](https://obsidian.md/)`                                 | 如果没有`title`属性，那么引用链接的名字作为代码块标题     |
| 折叠  |        `fold`        | 1. `fold`<br>2. `fold:"This is collapsed"`                                | 如果没有`title`属性，`fold`的标题作为代码块的标题    |
| 高亮  |         `hl`         | 1. `hl:1,3-4,foo,'bar baz',"bar and baz",/#\w{6}/`<br>2. `{1,3-4,6-9,11}` | 高亮单行、连续多行、包含的文本、正则表达式              |
| 包装  |       `unwrap`       | 1. `unwrap`<br>2. `unwrap:true`<br>3. `unwrap:inactive`                   | 如果启用，则单击代码块会把长的行多行显示               |
| 忽略  |       `ignore`       | 1. `ignore`                                                               | 如果启用，则不使用本插件的代码块渲染而使用Obsidian的默认渲染 |

``````Markdown title="All examples above"
<!-- ln -->
```cpp ln:28
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

<!-- title -->
```cpp title:"title"
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

<!-- reference -->
```cpp ref:[Obsidian](https://obsidian.md/)
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

<!-- fold -->
```cpp fold
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

```cpp fold:"Collapsed code"
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

<!-- hl -->
```cpp hl:1,3-4,foo,'bar baz',"bar and baz",/#\w{6}/
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

```cpp {1,3-4}
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

<!-- unwrap -->
```cpp unwrap:inactive
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

<!-- ignore -->
```cpp ignore
int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```
``````

#### 行内代码

!!! example "行内代码样例"
    ```Markdown title="Inline Code"
    `{python icon title:"title"}'result if true'.method() if 1 else result_if_false.property`
    ```

    `{python icon title:"title"}'result if true'.method() if 1 else result_if_false.property`
