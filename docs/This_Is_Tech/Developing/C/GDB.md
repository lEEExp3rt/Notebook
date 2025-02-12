---
tags:
- C
- cpp
---

# GDB

> GDB的吉祥物是一种射手鱼，用于将虫子(*Bug*)击落

!!! tip "技巧"
    [100个gdb小技巧](https://wizardforcel.gitbooks.io/100-gdb-tips/content/)

[GDB](https://sourceware.org/gdb/)是GNU提供的用于查看程序执行时发生在内部信息的调试工具

本文介绍GDB的简单用法

## 生成调试信息

使用gcc/g++编译，使用参数`-g`生成可调试的信息

```bash
gcc hello.c -o hello -g
```

---

## 显示代码

`l` 或 `list`

```bash
# 默认只显示10行，回车继续显示
(gdb) list

# 显示行号为rownum的行
(gdb) list <rownum>

# 显示函数的上下文
(gdb) list <funcname>
```

### 切换文件

```bash
# 显示文件filename的第rownum行
(gdb) l <filename>:<rownum>

# 显示文件filename的子函数funcname
(gdb) l <filename>:<funcname>
```

### 设置显示的行数

```bash
# 设置显示行数为num
(gdb) set listsize <num>
(gdb) set list <num>

# 查看已设置的行数设置
(gdb) show listsize
(gdb) show list
```

---

## 设置参数

```bash
# 设置参数
(gdb) set args <arg1> <arg2> ...

# 使用其它文件中的参数，文件中参数应按每行一个分布
(gdb) set args `cat args.txt`

# 清除参数
(gdb) set args

# 查看已设置的参数
(gdb) show args
```

---

## 启动与执行

启动参数只能使用一次

```bash
# 只执行main函数的第一行后就停止
(gdb) start

# 执行直到断点或结束
(gdb) run
(gdb) r

# 继续执行
(gdb) continue
(gdb) c
```

---

## 断点

### 设置断点

```bash
# b == break
# 设置当前文件的普通断点
(gdb) b <rownum>
(gdb) b <funcname>

# 设置普通断点到其它文件
(gdb) b <filename>:<rownum>
(gdb) b <filename>:<funcname>

# 设置条件断点，满足条件变量名varname等于某个值value才执行
(gdb) b <rownum> if varname == value
```

### 查看断点

```bash
(gdb) i b
(gdb) info break
```

!!! example "查看断点信息"
    ```bash
    (gdb) i b
    Num     Type           Disp Enb Address            What
    1       breakpoint     keep y   0x00005555555551d4 in main at hello.c:11
            breakpoint already hit 1 time
    2       breakpoint     keep y   0x0000555555555208 in main at hello.c:13
    3       breakpoint     keep y   0x0000555555555211 in main at hello.c:15
            stop only if j == 10
    ```

具体参数信息：

- `Num` ：断点的编号，删除断点或设置断点状态需要用
- `Enb` ：断点状态， `y` 表示可用， `n` 表示不可用
- `What` ：描述断点被设置在哪个文件的哪一行或哪个函数上

### 删除断点

断点删除后编号仍然继续而不会归零开始，创建的新断点也是

```bash
# delete == d
(gdb) d <Num>
(gdb) d <Num1> <Num2> ... 

# 删除一个范围
(gdb) d <Num1> - <Num2>
```

!!! example "删除断点"
    ```bash
    (gdb) i b
    Num     Type           Disp Enb Address            What
    1       breakpoint     keep y   0x00005555555551d4 in main at hello.c:11
            breakpoint already hit 1 time
    2       breakpoint     keep y   0x0000555555555208 in main at hello.c:13
            breakpoint already hit 1 time
    3       breakpoint     keep y   0x0000555555555211 in main at hello.c:15
            stop only if j == 10
            breakpoint already hit 1 time
    (gdb) d 1
    (gdb) i b
    Num     Type           Disp Enb Address            What
    2       breakpoint     keep y   0x0000555555555208 in main at hello.c:13
            breakpoint already hit 1 time
    3       breakpoint     keep y   0x0000555555555211 in main at hello.c:15
            stop only if j == 10
            breakpoint already hit 1 time
    (gdb) d 2-3
    (gdb) i b
    No breakpoints or watchpoints.
    ```

### 设置断点状态

```bash
# disable == dis, enable == ena
(gdb) dis <Num>
(gdb) dis <Num1> <Num2> ...
(gdb) ena <Num1> ...
(gdb) ena <Num1> - <Num2>
```
