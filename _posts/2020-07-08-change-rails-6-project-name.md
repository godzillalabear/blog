---
layout: post
title: "我想要換名字！！— Rails 6 專案改名"
sub_title: "The common elements"
categories:
  - Rails
elements:
  - content
  - css
  - formatting
  - html
last_modified_at: 2020-07-08T21:38:53-40:12
---

要想出一個好的專案名稱真的好難，前陣子 Demo Day 專案的名稱也為了取名問題卡很久，為了不浪費時間，先 new 了一個專案，後來才想到好名字，想要改名的時候，卻怕漏改東西，該怎麼辦才好呢？

在網路上搜尋 **rails 專案改名**的相關關鍵字，通常第一個就會叫你裝套件 [rename](https://github.com/morshedalam/rename)，但這些文章通常都年代久遠，而且這個套件也已經三年沒更新了。
在舊的版本的 Rails 專案中，專案中很多 class 都有專案名稱作為 namespace 在前面，像是這樣：
```ruby
#config/routes.rb
MyProject::Application.routes.draw do
end
```
當我們要修改專案名稱時，就需要一個一個改很麻煩，所以才會需要 rename 這個套件，但到了 Rails 6，這些東西都被改成下面這樣了：
```ruby
#config/routes.rb
Rails.application.routes.draw do
end
```
所以現在專案裡面也不會有太多跟 my_project 有關的東西了！

那這樣有什麼東西需要改呢？
第一個就是專案最上層的資料夾名稱了！我們除了可以透過本來作業系統的圖形化介面改資料夾名稱外，我們也可以透過終端機執行
```shell
❯ mv my_project better_name
```

除了專案的最上層資料夾名稱外，我們可以在終端機下指令去尋找專案資料夾底下所有檔案有沒有跟專案名稱一樣的內容
```shell
❯ grep -ri --exclude-dir={node_modules,public,log,tmp} 'my.*project' .
```
`grep 關鍵字 路徑` 這個指令可以尋找檔案或資料夾裡面有沒有符合關鍵字的部分，`-r` 可以往下層資料夾每一層都找，`-i` 是不在乎關鍵字的大小寫，而關鍵字也可以用正規表達式來寫。以專案名稱 my_project 來說，在 Rails 專案裡面，可能會變成 MyProject 這樣的形式，所以我們的正規表達式要寫成 `'my.*project’`，`.*` 代表零到多個任意字元。在 Rails 6 專案中，`node_modules` 與 `public` 分別存放我們需要用的套件跟 webpack 打包的檔案，而 `log` 紀錄我們程序執行的過程，`tmp` 資料夾中放的是暫存檔案，上述的 4 個資料夾內容都很多而且跟專案名稱不會有太大的關係，所以我們利用 `--exclude-dir={node_modules,public,log,tmp}` 來排除他們，讓搜尋這件事不會浪費資源做太久。

下完指令就可以看到終端機告訴我們哪些檔案中有內容需要改名囉！
```shell
./app/views/layouts/application.html.erb:    <title>MyProject</title>
./config/cable.yml:  channel_prefix: my_project_production
./config/application.rb:module MyProject
./config/database.yml:  database: my_project_development
./config/database.yml:  database: my_project_test
./config/database.yml:  database: my_project_production
./package.json:  "name": "my_project",
```

一一去修改這些檔案中有專案名字的部分，就不會漏掉東西少改了！
最最重要一定要記得改的是 `./app/views/layouts/application.html.erb` 中的標題，因為這會顯示在瀏覽器上方的標題，另外一個要記得改的是 `./config/application.rb` 中的 module 名稱喔～

到了 Rails 6 ，我們可以發現其實改名要修改的東西真的不多，很簡單。這樣以後就算 `rails new` 下錯名字也不用擔心了！




參考資料：
1. [Renaming a Ruby on Rails Application - Adam Scott](http://www.adamscott.io/blog/2014/01/21/renaming-a-ruby-on-rails-application/)
2. [refactoring - Effectively Renaming a Rails Project - Stack Overflow](https://stackoverflow.com/questions/15591791/effectively-renaming-a-rails-project)
3. [GitHub - morshedalam/rename: To rename rails application](https://github.com/morshedalam/rename)
