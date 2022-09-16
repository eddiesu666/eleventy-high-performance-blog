---
title: 寫網頁就是要炫耀啊，不然要幹麻？寄生 Github Pages
description: 寄生 Github Pages
date: 2022-09-16
tags:
    - Eleventy
    - 11ty
    - eleventy-high-performance-blog
    - GitHub Pages
    - GitHub Actions
    - CI/CD
    - 自己的 Blog 自己架
layout: layouts/post.njk
image: /img/posts/Github Pages.jpeg
---

### 前言
本地開方環境弄得差不多後，再來就是找個地方來放了。
既然 Github 提供了 [Actoins](https://docs.github.com/en/actions) 及 [Pages](https://docs.github.com/en/pages) 的服務，
當然不能辜負了官方的好意，透過調整 CI/CD workflow 來實現一條龍服務吧。

### 前置作業
3支 branche：
1. develop：主要為開發與修改使用。push 後會觸發 *Run CodeQL Analysis* 和 *Build and Test*。
2. main：不做任何修改，透過 *Pull requests* 異動，有異動便會觸發 *Build and Deploy* 發布至 gh-pages。
3. gh-pages：不做任何修改，由 Actions 自動更新，分支名稱 gh-pages 時，GitHub 便會預設將此分支設為 Pages 的來源，當然也可以依個人喜好到 Settings > Pages 修改。

### Workflows
目前使用的有三個，分別是 codeql-analysis、build-and-test 和 build-and-deploy，
codeql-analysis 和 build-and-test 是 template 中就有的，沒有修改。

build-and-deploy 
```yml
# Runs build and deploy
name: Build and Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]

    steps:
      - uses: actions/checkout@v2

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies and build
        run: npm install && npm run build-ci

      - name: Deploy
        uses: JamesIves/github-pages-deploy-action@v4.3.4
        with:
          ssh-key: ${{ secrets.DEPLOY_KEY }}
          branch: gh-pages
          folder: _site

```

Deploy 的部分，使用了 [JamesIves/github-pages-deploy-action](https://github.com/JamesIves/github-pages-deploy-action)，*Settings > Deploy Keys* 及 *Settings > Secrets* 記得要設定，可以參考[說明文件](https://github.com/JamesIves/github-pages-deploy-action#using-an-ssh-deploy-key-)。

### 總結
賀！網站已上線。基本 Blog 站台已成形，再來就各自發揮了，DNS、Google Analytics、Google Search Console、留言板之類的，就看個人有沒有要加了。  
>系列文請參考 [自己的 Blog 自己架](/tags/blog)。