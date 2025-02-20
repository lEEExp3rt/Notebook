---
tags:
  - c
  - cpp
icon: SiMake
---

# Make

本文介绍Makefile的规则与使用

> [!info] 一些参考
> [跟我一起写Makefile](https://seisman.github.io/how-to-write-makefile/)

---

## Make规则

默认情况下，make命令会在当前目录下寻找文件名为`Makefile`、`makefile`、`GNUmakefile`的文件

### Makefile基本规则

```makefile
#规则语法1
targets: prequisites
    command
    ...

#规则语法2
target: prequisites; command
    command
    ...
```

参数表：

|参数|名称|备注|
| :--:| :--: | :---: |
|targets|目标文件|用空格分开，可以使用通配符|
|command|命令|与prequisites同行时用空格分开，不同行时开头用`\t`|
|prequisites|依赖目标|可以用`\`表示换行，但其后不能跟任何字符|

### Makefile中的变量

Makefile中的变量是用一个字符串在Makefile中定义的，这个字符串就是变量的值

```makefile
#定义变量
VARNAME=string

#引用变量
    ${VARNAME}
```

除用户定义的变量外，make还允许使用环境变量、自动变量和预定义变量

常见的预定义变量：

|命令格式|含义|默认值|
| :---: | :---: | :---: |
|`AR`|库文件维护程序的名称|`ar`|
|`AS`|汇编程序名称|`as`|
|`CC`|C编译器名称|`cc`|
|`CPP`|C预编译器名称|`$(CC)-E`|
|`CXX`|C++编译器名称|`g++`|
|`RM`|文件删除程序的名称|`rm-f`|

常见的自动变量：

|命令格式|含义|
| :--: | :--: |
|`$*`|不包含扩展名的目标文件名称|
|`$+`|所有的依赖文件，以空格分开，并以出现的先后为序，可能包含重复的依赖文件|
|`$<`|第一个依赖文件的名称|
|`$?`|所有时间戳比目标文件晚的依赖文件，以空格分开|
|`$@`|目标文件的完整名称|
|`$^`|所有不重复的依赖文件，以空格分开|
|`$%`|如果目标是归档成员，则该变量表示目标的归档成员名称|
