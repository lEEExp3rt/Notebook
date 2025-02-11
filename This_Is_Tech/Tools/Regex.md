---
tags:
  - regex
icon: TiRegex
---

# Regex

> [!abstract] 本节摘要
> 本节收录正则表达式的相关内容

[Runoob](https://www.runoob.com/regexp/regexp-tutorial.html)

[Python3 正则表达式 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-reg-expressions.html)

## Introduction

正则表达式(*Regular Expression*)是一种文本模式，其中包括普通字符如`a-z`和特殊字符（称为"元字符"）

正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串，通常被用来检索、替换那些符合某个模式（规则）的文本

## Syntax

### 普通字符

|    字符    | 描述                    | 实例  |
| :------: | --------------------- | --- |
| `[...]`  | 匹配`[...]`中的所有字符       |     |
| `[^...]` | 匹配**除了**`[...]`中的所有字符 |     |
| `[A-Z]`  | 匹配区间内的字符              |     |
|   `.`    | 匹配除换行符之外的任何单个字符       |     |
