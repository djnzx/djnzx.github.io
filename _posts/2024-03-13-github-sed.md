---
layout: post
title:  "GitHub username change"
date:   2024-03-13 11:00:01 +0200
tags:   [git, github, sed, linux]
---

### GitHub name change

Ten years ago it was a silly decision to take a GitHub name `alexr007`

For a long time it was not possible to change it.
But today I realized it's possible!

Now, another task - to change all upstreams in GitHub projects already
existing on your machine.
Luckily with the proper prompt to ChatGPT you can achieve that
relatively quickly and easy

```shell
git remote set-url origin $(git remote get-url origin | sed 's/alexr007/djnzx/g')
```
