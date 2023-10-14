---
layout: post
title:  "GitHub Actions"
date:   2024-03-14 11:00:01 +0200
tags:   [cicd, github]
---

### Dive into github Actions

this is good, that github provided working `YAML` for a particular case
```yaml
name: Deploy Jekyll site to Pages
on:
  push:
    branches: ["master"]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: "pages"
  cancel-in-progress: false
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Ruby
        uses: ruby/setup-ruby@8575951200e472d5f2d95c625da0c7bec8217c42
        with:
          ruby-version: '3.1'
          bundler-cache: true
          cache-version: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4
      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${ { steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
but now, I have even more questions than I was...
```markdown
what are configurable root keys besides:
- `name`
- `on`
- `permissions`
- `concurrency`
- `jobs`
- `...`

what are configurable properties of `on`
- `push`
- `...`

what are configurable properties of `permissions`
- contents
- pages
- id-token
- `...`

what are configurable properties of each job
- `runs-on`
- `steps`
- `...`

what are configurable properties of each step
- `uses`, `with`
- `run`
- `env`
- `...`

where to see the source code of
- `actions/checkout@v4`
- `ruby/setup-ruby@8575951200e472d5f2d95c625da0c7bec8217c42`
- `actions/configure-pages@v4`
- `actions/upload-pages-artifact@v3`
- `actions/deploy-pages@v4`
- `...`

which env vars available:
- `${ { steps.pages.outputs.base_path }}`
- `${ { steps.deployment.outputs.page_url }}`
- `...`

- how to pass information between steps and actions
```

Thanks to [`ChatGPT`](https://chat.openai.com), I have more useful details:

The source  code of `actions/checkout@v4`
- resides on the [github](https://github.com/actions/checkout)
- `v4` means branch, so the code [is](https://github.com/actions/checkout/tree/v4)

as we can see, the source code of action is a huge enough spaghetti
- [main](https://github.com/actions/checkout/blob/v4/src/main.ts)
- [git-source-provider](https://github.com/actions/checkout/blob/v4/src/git-source-provider.ts)

It's clear enough that everything written in `TypeScript` since `GitHib` relates to `Microsoft`,
but to be precise, I don't know, how would I implement them if I was supposed to do that
