---
title : "Caddy+Hugo打造自动化个人博客"
date  : "2018-07-03T03:06:20Z"
draft : false
---

### 介绍
> Caddy 是一个 Go 语言实现的面向 HTTP2 和 HTTPS 的服务器，与 Nginx 和 Apache 相比，是一款激进的面向未来的浏览器；Hugo 是一款由 Go 语言实现的静态网站生成器

两个用 Go 语言写的利器，用来生成静态个人博客，再好不过了。这里使用 Ubuntu 16.04 操作系统安装 Hugo 生成个人博客，再利用Caddy 作网页服务器，替代 Ningx 监听 Hugo 生成的静态页面，时间仅需要 30ms 左右。Caddy 的 http-git，可以很好的实现版本控制。再配合 http-hugo 插件和 GitHub 的 Webhook 功能，能生成一个高度自动化的系统，在每次创作并且 git 到 GitHub 之后，自动使用 Hugo 生成静态页面，通过 Caddy 进行展示。