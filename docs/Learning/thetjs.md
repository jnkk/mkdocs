---
title: aProject TJs
---

# System Design

## Ecom System Design


```mermaid
graph LR
  index-->home
  index-->collection
  index-->blog
  index-->about-us
  index-->history
```

```mermaid
classDiagram
  class user
    user: user_id
    user: username
    user: email
    user: fullname
  class blog
    blog: user_id
    blog
  user-->blog
  user-->shop
```
