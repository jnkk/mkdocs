---
title: How to use Quartz from my personal github page
---

# **Steps to take to reproduce the website**  

> [!CAUTION]
> **DO NOT USE THE BUILT-IN CLONE TOOLS GITHUB DESKTOP FOR CLONING!!**  
> Use the ssh mode or the "git" mode. By using github desktop, it downloads the https, you can't push directly with the quartz command.  
> The Github Pages DOES NOT LOAD.  

> [!NOTE]
> TODO: Copy the content folder and try again, since it is saved in the repo.  
>  
> TODO: Maybe learn straight to Jekyll ???

## Preface  

Prepare an IDE, VSCode is my default right now.  
Since its not really taxing my hardware so it is okay.

Layout, keybindings and the abillity to see all of the things I might need is really helpful.
No need for "Focused Mode" in other text editors.  
After gathering, collecting and making notes in 1 place is really nice.

Emacs with org-mode enable.
Also testing neovim with kickstart package is nice to learn.
Learning html/css/ruby how to use and customize Jekyll might be the end goal.  

Right now, taking notes and learning.

`nodejs` and `npm` packages are needed.

## Cloning [personal github](https://github.com/jnkk/note-pages)

Learning anything can be a pain, especially if you don't know the fondamental on how things work. Like how `package.json` and `packages-lock.json` conflict with each other.

Steps to take in the future:

1. Download [VSCode or zed](texteditor.md). Zed is "stable" enough in archlinux. The distro I am using. Before it was Debian 12.  
Why? Because dependencies. Using apt and homebrew to make things work kinda defeats the purpose making and taking notes.
But with arch, and both using the "arch way" is better since it's still the same thing. Pacman and [Yay](https://github.com/Jguer/yay).

2. The things I downloaded in pacman is [here](afterinstallARCH.md). Make sure to download base things and then install using yay.
Using yay include github-desktop, might needed since it was for debugging the `package.json` files. There maybe few and far between updates using [Quartz](https://github.com/jackyzha0/quartz). The main theme of this note.
Pull requests and merging git is something I need to learn.  

3. Opening the [repo](https://github.com/jnkk/note-pages) on github. If there's a pull request, install [github-cli](https://cli.github.com/). Or maybe it works using github-desktop.  
Side note: I also made an account to gitlab. Maybe later, still learning on how things work in Github and git in general.
There was 5 pull requests.
Deleted `node_module`, `package-lock.json`, check for errors. Open chatgpt for a rundown. Before it was missing a module, and then running `npm install` after deleting the above files, fixes things.

Got an error like this. This is after chatting with chatgpt.

```bash
npm install --save-dev webpack webpack-cli
```