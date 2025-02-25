---
tags:
- Python
---

# Conda

---

## Installation

> 此处介绍的是在Linux下的安装操作

!!! info "官方信息"
    [anaconda.com](https://docs.anaconda.com/miniconda/)

### Download

首先在[清华源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/)或[浙大源](https://mirrors.zju.edu.cn/docs/anaconda/)下载包

下载后得到文件`Miniconda3-latest-Linux-x86_64.sh`

### Install

使用Bash进行安装

```sh
bash Miniconda3-latest-Linux-x86_64.sh
```

安装完后进入Bash即可使用

## Configuration

### 加入Fish Shell

!!! note "配置信息"
    如果你也使用[Fish Shell](https://fishshell.com/)，那么可以参考这个配置

进入Bash，使用`conda init fish`将初始化命令加入fish中

### 修改prompt位置

如果要把`(base)`改到prompt左侧，在`~/.config/fish/config.fish`中加入

```sh
set -gx CONDA_LEFT_PROMPT 1
```

退出后重启终端即可

### 换源

!!! warning "注意"
    Windows 用户无法直接创建名为`.condarc`的文件，可先执行`#!shell conda config --set show_channel_urls yes`生成该文件之后再修改

换为[浙大源](https://mirrors.zju.edu.cn/docs/anaconda/)

```sh title=".condarc"
channels:
    - defaults
show_channel_urls: true
default_channels:
    - https://mirrors.zju.edu.cn/anaconda/pkgs/main
    - https://mirrors.zju.edu.cn/anaconda/pkgs/r
    - https://mirrors.zju.edu.cn/anaconda/pkgs/msys2
custom_channels:
    conda-forge: https://mirrors.zju.edu.cn/anaconda/cloud
    msys2: https://mirrors.zju.edu.cn/anaconda/cloud
    bioconda: https://mirrors.zju.edu.cn/anaconda/cloud
    menpo: https://mirrors.zju.edu.cn/anaconda/cloud
    pytorch: https://mirrors.zju.edu.cn/anaconda/cloud
    pytorch-lts: https://mirrors.zju.edu.cn/anaconda/cloud
    simpleitk: https://mirrors.zju.edu.cn/anaconda/cloud
    nvidia: https://mirrors.zju.edu.cn/anaconda-r
```

换源后运行`#!shell conda clean -i`清除索引缓存

---

## Usage

```sh
# 虚拟环境
conda create <--name|-n> <env_name>
conda create -n <env_name> python=3.8       # 创建虚拟环境
conda create -n <env_new> --clone <env_old> # 克隆虚拟环境
conda remove -n <env_name> --all
conda env remove -n <env_name>              # 删除虚拟环境
conda activate <env_name>                   # 激活虚拟环境
conda deactivate                            # 退出虚拟环境
conda create -n <env_new>
conda env list                              # 列出当前所有可用环境

# 软件包
conda install [-n <env_name>] <package_name>[=version] # 安装包[到指定环境中][版本号]
conda search <package_name>                            # 搜索软件包
conda list [-n <env_name>]                             # 列出[指定]环境中的所有包
conda update --all                                     # 更新所有已安装的软件包
conda remove [-n <env_name>] <package_name>            # 从环境中删除一个包

conda update conda             # 更新conda
```
