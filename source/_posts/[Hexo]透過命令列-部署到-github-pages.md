---
title: "[Hexo]透過命令列部署到 Github Pages"
date: 2019-08-19 15:29:27
tags:
  - Hexo
  - Github Pages
---

<center> 一行指令就把部落格推到 Github Pages 上 </center>

<!-- more -->

## 緣起

最近發現我超過一年沒好好紀錄筆記，急忙地把部落格重新改版上線，結果在最後一個步驟驚覺早就忘記怎麼使用 `hexo deploy` ＸＤＤ，趁這強勢回歸(自已說)的機會，順便記錄一下如何使用這款 Hexo 的部屬套件，以免自己下次回歸時又忘記(X).

---

## 開始

1. 建立完畢 Hexo Blog 並且以推上 Github repo
2. 安裝 **hexo-deployer-git**

```shell
npm install hexo-deployer-git --save
# or
yarn add hexo-deployer-git
```

3. 在專案根目錄下找到 **\_config.yml** 修改設定:

```yml
deploy:
  type: git
  repo: <repo url>
  branch: <branch name>
  message: # commit 訊息，不寫會有預設值紀錄時間
```

4. 快樂的在 commad line 打上:

```shell
# 清理不必要檔案並且開始部署
hexo clean && hexo d
```

---

**<center>最後，記得在 Repo 上開啟 Pages 的服務唷</center>**

---

## 相關連結

[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)
[都寫到這裡了才發現官網有教學...](https://hexo.io/zh-tw/docs/deployment.html)
