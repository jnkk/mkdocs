---
title: Making a Website (or learning in general. Mainly using Django)
tags:
---

# Fullstack Using Django

Before anything, designing the frontend. And it is not easy. Adding tailwindcss is kinda confusing, but atleast shown where you put it.  
Managing where you put where is the hard stuff.
Kinda understand why companies employ web developers. And this just the basic of html. Like "click here to page 2" is something.

Using [penpot](https://penpot.app/) in a docker is a start. A free and self-hosted Figma alternative. But that's later down the line.  
At least functioning not so looking web is a start. Since I am the only user.

Using Django is not really for beginners, but starting to get the hang of it. Tried FastHTML which is far more confusing.
Since I dont know what I dont know.  
Maybe it is a way to learn the python programming language. "All in one" programming language.

Added nix for my dev enviorentment, docker for database stuff so not installed locally which can break stuff.  
I hope not to break the OS.

Since I am dreaming a lot of things that can be written in python. Just a matter of assembling and make it work.

First step, creating a really basic web page. Using tailwindcss to make it at least pretty to look at.  
Change the background to a darker color is easy, the hard part is connect and configuring it.

## Mapping on Mermaid Markdown

Visualizing on how software "looks" is better for me to understand.
Django's "visual" MVC

```mermaid
graph TD


```

##### Docker Compose File

Getting a head for my self, since I know I will need this:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        "NAME": "postgres",
        "USER": "postgres",
        "PASSWORD": "postgres",
        "HOST": "127.0.0.1",
        "PORT": "5432",
    }
}
```

```
version: '3.8'

services:
  db:
    image: postgres:17
    container_name: postgres_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: always

  redis:
    image: redis:latest
    container_name: redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    restart: always

volumes:
  postgres_data:
    driver: local
  redis_data:
    driver: local

```
