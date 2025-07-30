---
title: Phoenix Framework
---

# How to install

Use [`mise`](https://mise.jdx.dev/)

Further reading: [Source on Github](https://github.com/asdf-vm/asdf-erlang)


## Learning Elixir and Phoenix Framework

The best way to learn is by doing.
Lets start the hard way. Web development.

What is a website?

## Sources:

1. [Elixir School](https://elixirschool.com/en)
2. [Elixir HexDocs](https://hexdocs.pm/elixir/introduction.html)
3. [Phoenix HexDocs](https://hexdocs.pm/phoenix/overview.html)

### Mapping

```mermaid
graph TD
    root --> forum
    root --> blog
    root --> clth
    forum --> category

```

### Database Mapping

```mermaid
classDiagram
    class user {
    +id: uuid
    +full_name: string
    +user_name: string
    +is_staff: boolean
    +post_forum: uuid
    +post_clth: uuid
    +post_blog: uuid
    +bookmark: list
    }

    class forum {
   	+id_forum:
   	+category:
   	+forum_created_by:
   	+title:
   	+body:
   	+comments:
   	+reply:
   	+datetime:
    }

    class blog{
    +id_blog
    +blog_created_by
    +
    }


    class staff {
    +is_staff: boolean
    }

    user <|-- staff
    user <|-- forum
    user <|-- blog
    user <|-- clth
```
