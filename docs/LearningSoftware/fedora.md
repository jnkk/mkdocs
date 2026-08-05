---
title: 'Fedora'
icon: material/linux
tags:
  - fedora
  - linux
  - dnf
date_added: 2025-08-13
last_updated: 2025-12-29
---

Go to `/etc/dnf/libdnf5.conf.d/` create a new file, let's say `80-local.conf`

Add these to the file to make the dnf download faster
```
[main]
fastestmirrors=true
max_parallel_downloads=10
```
