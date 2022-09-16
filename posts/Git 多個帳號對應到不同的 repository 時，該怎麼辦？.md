---
title: Git 多個帳號對應到不同的 repository 時，該怎麼辦？
description: Git 帳號與 repository 對應設定
date: 2022-08-30
tags:
    - Git
    - Github
    - Gitlab
    - Azure
    - Config
layout: layouts/post.njk
image: /img/posts/git.png
---

### 前言

現今 Git 百家爭鳴，舉凡 Github、Gitlab、Azure DevOps...  
應改不可能只使用一個平台，或是只有一個帳號吧！  
如果是，那以下可省略了。👋

### 全域設定

首先，檢查一下 gloabal 和 system 的 config，  
主要是看有没有 credential.helper 把帳號密碼存起来了。

```bash
git config --global -l
git config --system -l
```

確認設在哪個後，執行 unset credential.helper`,  
這樣就能重新輸入帳號密碼了。

```bash
git config --global --unset credential.helper
git config --system --unset credential.helper
```

### 各自的權限設定

先確認一下本地 repository 的遠端連線資訊，  
https 開頭的就是使用https，git@ 開頭的就是使用ssh。

```bash
git remote -v
```

以 Github 為例，大概會是長這樣的：

>**{帳號}**@github.com/**{帳號}**/**{repository 名稱}**

設定帳號與 repository 對應：

```bash
git remote set-url origin https://UserA@github.com/UserA/repoA
```

這時候 push，就要输入一下 UserA 的密码，然后就能 push 上去了。  
但每次 push 都需要输入密码了也太麻煩了吧！  
為了避免麻烦，針對本地的 repository，設置一下 local 的 credential.helper ：
```bash
git config --local credential.helper store
```

查看當前配置：
```bash
git config --list
```

最後，為了避免 user.name 與 user.email 使用到全域設定，  
在 repository 底下也設定一組吧。

```bash
git config user.name "your name"
git config user.email "your email"
```

### 總結
1. 清空 global 和system 的credential.helper。
2. 對 repository 設定一下 remote 和 local 的 credential.helper