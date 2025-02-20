---
tags:
- tools
---

# WSL

!!! info "官方文档"
    [适用于 Linux 的 Windows 子系统文档 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/)

## Introduction

### WSL 1

WSL1本质是一个翻译层，它将Linux指令翻译成Windows NT内核可以理解的系统指令，而不运行真正的Linux内核，因此在某些情况下会出现兼容性问题，如无法运行Docker

### WSL 2

WSL2基于Hypervisor的虚拟化平台，开启HyperV后，Windows内核就变成了运行在HyperV上的大号虚拟机，WSL2则是在HyperV上的另一个虚拟机，可以运行真正的Linux内核

---

## Configuration

开启WSL 2的基本流程如下：

1. 开启CPU虚拟化，大部分电脑默认开启，可以在**任务管理器-性能-CPU**中查看是否开启
2. 开启Windows功能，在**搜索栏搜索`功能`-进入启用或关闭Windows功能-开启`适用于Linux的Windows子系统`和`虚拟机平台`**
3. 根据系统提示重启电脑
4. 以**管理员身份**运行CMD`#!Powershell wsl --install`开始下载

!!! info "注意"
    默认下载的发行版为**Ubuntu-22.04LTS**

!!! tip "建议"
    如果遇到下载网络问题，可以使用`wsl --install --web-download`

### 选择发行版

也可以安装其它发行版

```powershell
wsl --list --online # 搜索可用发行版
wsl --install <distroName> [--web-download] # 安装对应发行版
```

### 查看已安装列表

输入以下命令查看已安装的发行版列表及其运行状态

```powershell
wsl --list -v
```

### 设置默认发行版

可以设置启动的默认发行版

```powershell
wsl --set-default <distroName>
```

### 卸载

```powershell
wsl --unregister <distroName>
```

### 备份与恢复

使用以下命令将一个已安装的发行版导出为一个压缩包

```poershell
wsl --export <distroName> <zippedPackName>
```

使用以下命令将一个包含发行版数据的压缩包导入系统

```powershell
wsl --import <OSName> <PathToData> <YourZippedPackPath>
```

!!! example "导入导出样例"
    ```powershell
    wsl --export Ubuntu22.04 C:/Users/User/Desktop/Ubuntu.tar
    wsl --import Ubuntu22.04 D:/wsl C:/Users/User/Desktop/Ubuntu.tar
    ```

### 远程桌面

WSL 可以直接与Windows图形界面连接

#### Ubuntu

!!! bug "Ubuntu远程桌面直接连接有兼容性问题而容易踩坑"

使用HyperV虚拟机进行连接远程桌面

#### Kali

在WSL 2中使用Kali-Linux发行版时，可以使用`Win-Kex`进行远程桌面连接

```powershell
sudo apt install kali-win-kex
```

### 高级配置

WSL的配置文件有`wsl.conf`和`.wslconfig`，其中

* `.wslconfig`用于在WSL2上运行的所有已安装发行版中配置全局设置，是一个Windows文件
* `wsl.conf`用于其中每个Linux发行版按各个发行版配置的本地设置，存储路径`etc/wsl.conf`

**如果修改了上述的配置文件，需要遵循*8秒规则*，即关闭发行版8秒后重启**

```powershell
wsl --shutdown # 关闭所有发行版
wsl --terminate <distroName> # 关闭某个发行版
```

#### 默认启动用户

在`wsl.conf`中配置默认启动时的用户

```txt title="wsl.conf"
[user]
default=YourUserName
```

#### 网络配置

默认情况下，WSL的网络网段和宿主机不处于同一个网段上，是典型的NAT网络

要修改网络配置，进入`C:/Users/UserName/`下，创建`.wslconfig`，然后编辑内容为

```txt
[wsl2]
networkingMode=mirrored
```

??? tip "可供参考的配置"
    ```txt
    [wsl2]
    memory=8G
    networkingMode=mirrored
    dnsTunneling=true
    firewall=true
    autoProxy=true
    
    [experimental]
    # requires dnsTunneling but are also OPTIONAL
    bestEffortDnsParsing=true
    useWindowsDnsCache=true
    ```

---

## Usage

在Powershell中选择发行版启动：

```powershell
wsl -d <distroName>
```
