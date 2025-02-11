---
tags:
  - dev
icon: TiVersions
---

# 语义化版本

## Introduction

[软件版本号如此奇怪有它自己的理由](https://semver.org/lang/zh-CN/#%E7%AE%80%E4%BB%8B)，基于此，**语义版本控制**(*SemVer*)是一种软件版本管理方案，旨在传达版本中基本变更的含义

语义版本管理提供了一种清晰、结构化的软件版本管理方法，让开发人员更容易了解变更的影响并管理依赖关系

通过遵循 SemVer 规则，开发人员可以确保其软件以可预测的方式稳定发展

> [!info] 社区规范
> [semver.org](https://semver.org)

---

## Definitions

### API

使用SemVer的软件必须定义公共API

### 标准版本号

> [!info] 更完整严谨的规范
> [语义化版本控制规范](https://semver.org/lang/zh-CN/#%E8%AF%AD%E4%B9%89%E5%8C%96%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6%E8%A7%84%E8%8C%83semver)

SemVer使用由三部分组成的版本号：

$$\text{major.minor.patch}$$

即**主版本.小版本.PATCH版本**

- 主版本：当出现不兼容的API变动时，主版本号递增
- 小版本：在以向后兼容的方式添加功能时递增
- PATCH版本：在进行向后兼容的错误修复时递增

> [!danger] 注意
> 标记版本号的软件发行后禁止改变改版本软件的内容，任何新修改都必须以新版本发行

### 迭代流程

#### 初始开发

初始开发的版本从`0.1.0`开始，一切都可能随时被改变，这样的公共API不应该被视为稳定版

#### 第一个稳定版本

发布的第一个稳定版本为`1.0.0`，界定了公共API的形成

这一版本之后的所有版本号更新都基于公共API及其修改内容

#### 补丁更新与修复

发布一些补丁更新和错误修复后，迭代递增PATCH版本号

这里的修订是指针对不正确结果而进行的内部修改

> [!example] 小修复
> `1.0.0` -> `1.0.1`

#### 添加新功能

添加一些新功能和增量更改后迭代递增次版本号

注意这里的新功能要**向后兼容**

> [!tip] 提示
> 任何公共API的功能被标记为弃用时也要递增次版本号

每当次版本号递增时，修订号必须归零

> [!example] 新功能
> `1.0.1` -> `1.1.0`

#### 重大版本

添加了一些不向后兼容的重大变更后，迭代递增主版本号

> [!example] 重大更新
> `1.1.0` -> `2.0.0`

主版本号递增时，次版本号和修订号也要归零

### 特殊版本

#### 预发布版本

用连字符和一系列以点分割的标识符表示，其中标识符必须由ASCII字母数字和连接号，即`[0-9A-Za-z-]`组成，且禁止留白，数字型的标识符禁止前向补零

预发布版本的优先级低于相关联的标准版本号

> [!example] 预发布版本
> ```txt
> 1.0.0-alpha
> 1.0.0-alpha.1
> 1.0.0-0.3.7
> 1.0.0-x.7.z.92
> ```

#### 元数据

版本编译信息，即元数据，可以在被标注的修订版或预发布版本后，再用加号和一系列以点分割的标识符表示，其中标识符必须由ASCII字母数字和连接号，即`[0-9A-Za-z-]`组成，且禁止留白，数字型的标识符禁止前向补零

在判断版本优先层级时可以忽略元数据，只有在两个版本的标准版本号和预发布版本号等都一致时才比较元数据

> [!example] 版本优先层级比较
> `1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0`

---

## Grammar

```txt title:"巴克斯范式语法"
<valid semver> ::= <version core>
                 | <version core> "-" <pre-release>
                 | <version core> "+" <build>
                 | <version core> "-" <pre-release> "+" <build>

<version core> ::= <major> "." <minor> "." <patch>

<major> ::= <numeric identifier>

<minor> ::= <numeric identifier>

<patch> ::= <numeric identifier>

<pre-release> ::= <dot-separated pre-release identifiers>

<dot-separated pre-release identifiers> ::= <pre-release identifier>
                                          | <pre-release identifier> "." <dot-separated pre-release identifiers>

<build> ::= <dot-separated build identifiers>

<dot-separated build identifiers> ::= <build identifier>
                                    | <build identifier> "." <dot-separated build identifiers>

<pre-release identifier> ::= <alphanumeric identifier>
                           | <numeric identifier>

<build identifier> ::= <alphanumeric identifier>
                     | <digits>

<alphanumeric identifier> ::= <non-digit>
                            | <non-digit> <identifier characters>
                            | <identifier characters> <non-digit>
                            | <identifier characters> <non-digit> <identifier characters>

<numeric identifier> ::= "0"
                       | <positive digit>
                       | <positive digit> <digits>

<identifier characters> ::= <identifier character>
                          | <identifier character> <identifier characters>

<identifier character> ::= <digit>
                         | <non-digit>

<non-digit> ::= <letter>
              | "-"

<digits> ::= <digit>
           | <digit> <digits>

<digit> ::= "0"
          | <positive digit>

<positive digit> ::= "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

<letter> ::= "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J"
           | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T"
           | "U" | "V" | "W" | "X" | "Y" | "Z" | "a" | "b" | "c" | "d"
           | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n"
           | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x"
           | "y" | "z"
```
