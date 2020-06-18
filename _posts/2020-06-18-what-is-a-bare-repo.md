---
layout: post
title: "什麼是 bare repo?"
sub_title: "The common elements"
categories:
  - Git
elements:
  - content
  - css
  - formatting
  - html
last_modified_at: 2020-06-18T16:46:17-57:34
---

除了平常 `git init` 之後可以建立一個 git repository 之外，你知道 `git --bare init ` 會發生什麼事嗎？
首先，我們先來聊聊 bare repository 跟平常的 repository 有什麼差別吧！

做過 `git init` 的專案， git 就可以幫你做版本控制，這種帶有一個`.git`資料夾跟其他檔案的工作目錄可以叫做 working directory，但其實還有另外一種沒有工作目錄的儲存庫 — bare repository （裸儲存庫）。一個 bare repository 就好像單純的 `.git` 資料夾一樣，他存了經過 git 計算與壓縮後的比對結果，commit 紀錄等等。

![](https://i.imgur.com/OIEUL7V.png)
一個經過 `git --bare init` 的資料夾會自己生出這些東西。需要注意的是，一般慣例上來說，bare repo 的資料夾名稱會以 `.git` 結尾。

那什麼時候該用裸儲存庫，什麼時候該用一般的儲存庫呢？

如果你的專案只是自己要用，那就用一般的儲存庫就好了，或是你也可以透過 GitHub、Gitlab 等原始碼代管服務平台來幫你的專案做分享。但是如果你不想用別人的平台來做分享，就可以透過 bare repo 來進行分享。在一台 server 上建立 bare repo，擁有 server 存取權限的人就可以透過 git push 把本地端的專案推上去，也可以通過 clone、pull 把專案拉下來。不過，一個 bare repo 中是沒有原本專案的那些程式碼檔案的，他沒有 working tree，所以不可以對他生出新的 commit 。

所以說當你要工作的時候，就用一般的儲存庫，當你要分享的時候，就要使用 bare repo。








參考資料：[What is a bare git repository? Jon Saints](https://www.saintsjd.com/2011/01/what-is-a-bare-git-repository/)
