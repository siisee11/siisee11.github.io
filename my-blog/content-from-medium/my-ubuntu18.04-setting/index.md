---
title: "My Ubuntu18.04 setting"
description: "Ubuntu 18.04 setting reference blog (korean)"
date: "2019-12-01T14:10:35.513Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/my-ubuntu18-04-setting-d4a47e03eff
redirect_from:
  - /my-ubuntu18-04-setting-d4a47e03eff
---

Ubuntu 18.04 setting reference blog (korean)

-   gnome setting  
    [https://codevkr.tistory.com/89](https://codevkr.tistory.com/89)
-   terminal setting  
    \[Guake\]([https://vitux.com/install-and-use-guake-a-drop-down-terminal-emulator-for-ubuntu/](https://vitux.com/install-and-use-guake-a-drop-down-terminal-emulator-for-ubuntu/))  
    \[Terminator\]([https://deepweller.tistory.com/25](https://deepweller.tistory.com/25))([https://programmingsummaries.tistory.com/361](https://programmingsummaries.tistory.com/361))  
    \[zsh\]([https://kairos03.github.io/2018/09/10/install-oh-my-zsh.html](https://kairos03.github.io/2018/09/10/install-oh-my-zsh.html))  
    \[zsh2\]([https://gurumee92.tistory.com/115](https://gurumee92.tistory.com/115))
-   X11 forward setting  
    \[x11 forward\]([https://harryp.tistory.com/684](https://harryp.tistory.com/684))
-   hostname setting

```
$ hostnamectl set-hostname <name>
```

-   VScode  
    [https://code.visualstudio.com/docs/?dv=linux64\_deb](https://code.visualstudio.com/docs/?dv=linux64_deb)
-   ibus setting (to use korean)  
    [https://snowdeer.github.io/linux/2018/07/11/ubuntu-18p04-install-korean-keyboard/](https://snowdeer.github.io/linux/2018/07/11/ubuntu-18p04-install-korean-keyboard/)
-   Vim setting (.vimrc)

```
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

$ vim .vimrc
```

This below is my current vim setting including setting for Vundle

[https://gist.github.com/e44e8ca3d1500e32d08dc3f40e6ec410.git](https://gist.github.com/e44e8ca3d1500e32d08dc3f40e6ec410.git)

Launch `vim` and run `:PluginInstall`

-   Vim plugins

\[NerdTree\]([https://kamang-it.tistory.com/entry/vi-vimNerdTreeNerdTree%EC%84%A4%EC%B9%98%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0?category=749862](https://kamang-it.tistory.com/entry/vi-vimNerdTreeNerdTree%EC%84%A4%EC%B9%98%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0?category=749862))

```
Plugin ‘scrooloose/nerdtree’
Plugin ‘bling/vim-airline’ 
Plugin ‘tpope/vim-abolish’
Plugin ‘Valloric/YouCompleteMe’
Plugin ‘Lokaltog/vim-easymotion’
```

-   GoogleDrive

[**astrada/google-drive-ocamlfuse**  
_google-drive-ocamlfuse is a FUSE filesystem for Google Drive, written in OCaml. It lets you mount your Google Drive on…_github.com](https://github.com/astrada/google-drive-ocamlfuse "https://github.com/astrada/google-drive-ocamlfuse")[](https://github.com/astrada/google-drive-ocamlfuse)

\[Automounting\]([https://github.com/astrada/google-drive-ocamlfuse/wiki/Automounting](https://github.com/astrada/google-drive-ocamlfuse/wiki/Automounting))
