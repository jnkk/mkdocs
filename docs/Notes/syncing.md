---
title: Syncing
---

# Sync to Google Drive

`rclone` is for syncing to the cloud like Google Drive.  
`rsync` is for local backups. Can be used for local directories.

## rclone how to

Install. Then:

```bash
rclone config
```

Follow the steps in the terminal.

Install the package `cronie` for archlinux.  
Add a `cron` job:

`@reboot rclone mount GoogleDrive:/markdown /home/jnkk/gd-markdown/ --daemon`

