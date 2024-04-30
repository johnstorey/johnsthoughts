---
title: "VSpaceCode and NeoVim Together"
date: 2023-06-20T06:58:53-05:00
draft: false
---

# Note on using VSpaceCode plugin and Neovim Plugin

Having settled on Visual Studio Code with the VSpaceCode and NeoVim plugins, I want them to work together. The first step is to
get the NeoVim plugin to pass the <SPACE> keypresses to VSpaceCode. Here are the steps (on MacOS)

1. Open or create ~/config/nvim/init.vim
2. Add a command to pass the <SPACE> keypress along

```shell
  " Integrate with whichkey for spacemacs-style space key
  nnoremap <space> :call VSCodeNotify('whichkey.show')<CR>
```


  Now pressing the space bar will again go to the WhichKey plugin in Visual Studio Code.

  This appears to be an interesting [Youtube Playlist](https://www.youtube.com/playlist?list=PLQBdjw2QsRzMZjaGyvBxe88iS4uVaW3OI) on how one person gets some
  NeoVim plugins working with VSpaceCode.