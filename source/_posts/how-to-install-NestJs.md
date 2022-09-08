---
title: how to install NestJs
date: 2022-05-25 21:08:06
categories: nodejs nestjs
tags: nodejs nestjs express
---

# what is nestjs
Nest (NestJS) 是一個用於構建高效、可擴展的Node.js服務器端應用程序的框架。它使用漸進式 JavaScript，使用 TypeScript 構建並完全支持TypeScript（但仍允許開發人員使用純 JavaScript 進行編碼），並結合了 OOP（面向對象編程）、FP（函數式編程）和 FRP（函數式反應式編程）的元素。

<br>

在底層，Nest 使用了強大的 HTTP 服務器框架，比如Express（默認），並且可以選擇配置為使用Fastify！

<br>

Nest 在這些常見的 Node.js 框架（Express/Fastify）之上提供了一個抽象級別，但也將它們的 API 直接暴露給開發人員。這使開發人員可以自由使用可用於底層平台的無數第三方模塊。

<br>

# why nestjs
近年來，由於 Node.js，JavaScript 已成為前端和後端應用程序的網絡“通用語”。這催生了Angular、React和Vue等很棒的項目，它們提高了開發人員的工作效率，並支持創建快速、可測試和可擴展的前端應用程序。然而，雖然 Node（和服務器端 JavaScript）存在大量出色的庫、幫助程序和工具，但它們都沒有有效地解決架構的主要問題。

Nest 提供了一個開箱即用的應用程序架構，允許開發人員和團隊創建高度可測試、可擴展、鬆散耦合且易於維護的應用程序。該架構深受 Angular 的啟發。

# how to install

```sh
$ npm i -g @nestjs/cli
$ nest new [project-name]
$ cd [project-name] && npm run start
```

<br>

![label](how-to-install-NestJs/1.png)

<br>

打開瀏覽器並導航到 http://localhost:3000/ <br>


<br>


出現HelloWorld，就可以開始寫API拉～
