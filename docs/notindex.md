---
title: Index for learning in my Personal Notes on Github pages
tags:
  - index
---

# Learning

> [!NOTE] NOTE TO SELF
> DO NOT USE NIXOS! You are NOT ready for it. Familiarize and use inside the VM first. Use nix first.  
> Cleaning files and folders of this might be an order. For better site mapping.  
> BUT before that, github actions failed to "execute" because IDK how to use and messed up the `MERGE` thingy in the `Pull Request` thingy. Can't fixed it. Maybe just moving `content` folder and restart it again.  
> So, right now it is just a note taking provider inside Github. Also in Google Drive. But not as up to date as in Github.

Go to the directory note-pages/content
This is the `index.md` file. In the root directory.

## [[Latest]](Notes/notesgithubpages.md)

## [[After install]](./Notes/afterinstallDEBIAN.md)

## [Selfhost](Selfhost/Selfhost.md) [Link](https://selfh.st/)

### TODOs

Customizing the "front page" to display folder files

#### This is a blank Quartz installation.

See the [documentation](https://quartz.jzhao.xyz) for how to get started

Or see the note-pages directory

### [Cloning notes-personal or "My Digital Garden" with steps:](Notes/notesgithubpages.md)

### Start the server

> [!NOTE]
> This is for running it locally:

```bash
npx quartz build --serve
```

### "Resync" with github pages

> [!NOTE]
> (Generally) For uploading the content page. Use:

```bash
npx quartz sync --no-pull
```

> [!NOTE]
> For syncing the github repo, MAINLY after MERGING FROM the github repo. Use:

```bash
npx quartz sync
```
