---
title: "20230206 Yesterdays War"
date: 2023-03-07T20:01:14-06:00
---

I've been around awhile. Maybe you have two. One of the eternal wars
among developers is: Emacs, Vi, or an IDE? It's so famous its in cartoons,
such as this famous one from (XKCD)[https://xkcd.com].

By the way, if you have never been to that site, treat yourself. It's a hoot!

![Image from xkcd.com](https://imgs.xkcd.com/comics/real_programmers.png)

Yesterday I was remembering the experience of flow in a mouseless world from my early days, and why I left that (lack of good debugger support across languages.) I decided to check on the state of Emacs and Vim and see if I should go back.

For my money, they might as well be the same now.

# The State of Things

My point of comparison was the Emacs distribution, [Spacemacs](https://www.spacemacs.org). Spacemacs type distributions provide nice menu driven interfaces for the potentially unending options. You press the spacebar, and it shows a list of the available categories of commands (which it calls major modes). Select one of those -- a one letter key press -- and you see all the options in that category. This means most commands consist of typing "SPACE key-one key-two". Help is built in to the process, and rarely is a mouse involved.

Then I watched a Neovim configuration setup video and found a distribution, [SpaceVim](https://spacevim.org). It's pretty much the same thing.

Emacs has Elisp as its extenstion language. Vim has Lua. I may prefer Lua, but at the end of the day, it's Lisp.

I'm sure there are important differences, but from 1000 feet it looks much the same.

But I want that debugger. I want that language support for even obscure language. I want [Visual Studio Code](https://code.visualstudio.com). But wait ...

# VSpaceCode

[VSpaceCode](https://vspacecode.github.io) replicates the Spacemacs experience in VS Code, and it seems pretty good. So I can have the extensions and debugger from VS Code with most of the mouseless experience of Vim or Emacs?

Yes please!

Maybe the next month of using VSpaceCode will prove me wrong, but I think its over. Everyone who wants a mouseless experience -- and does not want to spend the time maintaining their own customer editor -- has ended up here.

It's time. Give up bikeshedding. Just go with VS Code and VSpaceCode, and get to the coding. Me, I am 50%-ish through my current project, and I'll finish it with VSpaceCode.

Happy hacking!
