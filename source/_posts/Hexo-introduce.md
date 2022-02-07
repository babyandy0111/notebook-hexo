---
title: Hexo introduce
date: 2022-02-07 17:25:49
tags: Hexo
---
# what is Hexo
Hexo 是利用 Node.js 所撰寫而成的部落格程式，可以利用Hexo指令產生出靜態網頁。

# why Hexo
主要就是在選擇撰寫平台時，覺得市面上的部落格平台都不太符合自己想要，且想要單一維護在github上，因此找到Hexo，會選擇的原因主要為：

###### 支援 Markdown 撰寫
用工程的方式學習寫文章，好像不錯～

###### 一鍵部署
部落格文章也可以部署，sounds good

###### 很炫XD
沒其他原因了...

# How to Use
1. create github repository for website
2. npm install -g hexo-cli
3. npm install hexo-deployer-git --save
4. hexo init 
5. edit _config.yml 
``` 
deploy:
  type: "git" #使用 Git 部署
  repo:  https://github.com/babyandy0111/notebook.git #儲存庫
  branch: "master" #儲存庫分支
```
5. hexo clean && hexo g && hexo d

# 參考連結
[Hexo](https://hexo.io/)

[30 天利用 Hexo 打造技術部落格](https://ithelp.ithome.com.tw/users/20139218/ironman/3910)

[markdown 教學](https://markdown.tw/)

