---
layout: post
title: vim plugin 사용하는 법
image: Vim.jpeg
author: Jaeyoun
date: 2020-03-05T10:03:47.149Z
tags: 
  - tools
---

Vim을 더 잘 활용할 수 있게 해주는 Plugin설치에 관한 내용이다.

---

# Vim Plugin
Vim에는 Plugin을 설치해서 여타 IDE처럼 사용할 수 있게 되어있다.
CUI환경에서 강력한 기능을 사용할 수 있다는 장점을 가진다.

---

# Vundle
Vundle은 vim에 plugin설치를 도와주는 소프트웨어이다.

Vundle을 통해 vim에 플러그인을 설치하는 방법은 간단하다. [깃허브 사이트](https://github.com/VundleVim/Vundle.vim)에서 더 자세히 볼 수 있다.

먼저, Vundle을 다운로드 받는다.

```
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim 
```

다운로드 받은 후, ```.vimrc```파일을 열어서 아래 내용을 추가한다.

```
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

위 내용에서 주석은 읽어본 후에 지워도 되고, ```call vundle#begin()```과 ```call vundle#end()```사이에 사용하고 싶은 플러그인을 적어주면 된다.

현재 나는 아래의 플러그인들을 사용한다. 하나씩 추가해가면서 사용법을 익히는 것이 좋다.

```
Plugin 'VundleVim/Vundle.vim'

Plugin 'tpope/vim-fugitive'
Plugin 'git://git.wincent.com/command-t.git'
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
Plugin 'scrooloose/nerdtree'
Plugin 'bling/vim-airline' 
Plugin 'tpope/vim-abolish'
Plugin 'Valloric/YouCompleteMe'
Plugin 'Lokaltog/vim-easymotion'
Plugin 'fatih/vim-go'
Plugin 'ronakg/quickr-cscope.vim'
Plugin 'taglist.vim'
```

플러그인을 적어주고 난 후에, ```vim```으로 빈 창을 띄운 후 ```:PluginInstall```을 입력하면 자동으로 설치해준다.

---

# YouCompleteMe
위의 플러그인들을 설치했다면 YCM서버가 없다고 오류가 뜰 것인데, YCM은 따로 설치를 해주어야 한다.

```~/.vim/bundle/YouCompleteMe```에 자동으로 코드는 받아져 있을 것이다. (경로는 다를 수 있다)

해당 경로로 이동한 후에,
```
python3 ./install.py --clang-completer
```

clang 이외의 추가적인 언어설정은 [YCM페이지](https://github.com/ycm-core/YouCompleteMe)를 참고하자.