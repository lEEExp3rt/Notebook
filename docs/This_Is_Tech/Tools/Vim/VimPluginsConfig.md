---
tags:
- Vim
---

# Vim Plugins Configuration

To configure some useful plugins for vim, here is a tutorial for reference.

---

## Vundle

See detailed information at [Vundle.Vim](https://github.com/VundleVim/)

### Introduction

Vundle is short for *Vim bundle* and is a Vim plugin manager, which allows you to better manage your plugins.

### Installation

Use git to install Vundle at first.

```Bash
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

After this step you can see Vundle.Vim in your path `~/.vim/bundle/Vundle.vim`.

### Configuration

After installation, you need to open your `~/.vimrc` and edit it by adding these following content into it:

```batch
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

And you can remove some unneeded plugins in the `.vimrc` file by deleting their sentences.

### Usage

After installing and configuring, you can now use Vundle to search, install and better manage your Vim plugins.

To install plugins, you can:

* Launch vim and run `:PluginInstall`.
* Install from command line by `vim + PluginInstall +qall`.

---

## YouCompleteMe(YCM)

See detailed information at [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe).

### Introduction

YouCompleteMe is a code-completion plugin in Vim.

### Installation

Use **Vundle** to install YCM.

By adding `Plugin 'Valloric/YouCompleteMe'` to `~/.vimrc` in this position:

```Ruby
call vundle#begin()
. . . 
Plugin 'Valloric/YouCompleteMe'
. . .
call vundle#end()
```

Then save `.vimrc` and open vim and use `:PluginInstall` and wait until it finished.

> You can check whether it is installed by the command `:PluginList` in Vim.

Then enter the YCM working folder:

```Bash
cd ~/.vim/bundle/YouCompleteMe
```

Then install submodules:

```Bash
git submodule update --init --recursive
```

Then run the installation script:

```Bash
./install.sh --clang-completer # Install by shell.

./install.py --clang-completer # Install by python.
```

Then the installation is completed.

### Configuration

#### Other Language Support

> Reference:[CSDN](https://blog.csdn.net/lyshark_lyshark/article/details/125846994)

However, the installation from above only support C completion. If more functions are needed, you can run:

```Bash
./install.py --all # All language installation.

./install.py --clangd-completer # C code-completion with better completion.
```

`./install.py --clangd-completer` will be completed if these following are added to `~/.vimrc`:

```Batch
" Add these to your .vimrc

let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'

nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR>
```

#### Better UI

> Reference:[CSDN](https://blog.csdn.net/OceanWaves1993/article/details/113562748?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170964419316800186546839%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=170964419316800186546839&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-10-113562748-null-null.142^v99^pc_search_result_base7&utm_term=youcompleteme%E5%85%B3%E9%97%ADpreview&spm=1018.2226.3001.4187)

---
