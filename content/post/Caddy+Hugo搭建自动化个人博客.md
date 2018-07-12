---
title : "Caddy+Hugo打造自动化个人博客"
date  : "2018-07-03T03:06:20Z"
draft : false
---

---

### 介绍
> Caddy 是一个 Go 语言实现的面向 HTTP2 和 HTTPS 的服务器，与 Nginx 和 Apache 相比，是一款激进的面向未来的浏览器；Hugo 是一款由 Go 语言实现的静态网站生成器

Hugo 相比其他 Hexo 和 Jekyll，主要的优势就是编译速度极快，100个页面渲染时间仅需要 30ms 左右。而 Caddy 也是主打的轻量快速，两个用 Go 语言写的利器，用来生成静态个人博客，再好不过了。

这里使用 Ubuntu 16.04 操作系统安装 Hugo 生成个人博客，再利用Caddy 作网页服务器，替代 Ningx 监听 Hugo 生成的静态页面。

Caddy 的 http-git，可以很好的实现版本控制。再配合 http-hugo 插件和 GitHub 的 Webhook 功能，生成一个高度自动化的系统，在每次创作并且 git 到 GitHub 之后，自动使用 Hugo 生成静态页面，通过 Caddy 进行展示。

### 安装并配置 Hugo
首先进行 Hugo 的安装。推荐使用官方 [Release](https://github.com/gohugoio/hugo/releases) 页面的二进制安装包，能自动将 Hugo 安装到`$PATH`。

```bash
$ dpkg -i hugo.deb
$ hugo version
```

#### 创建网站文件夹

```bash
$ mkdir /var/www/qaq.money
```

#### 生成站点

```bash
$ hugo new site /var/www/qaq.money
```

#### 安装主题

Hugo 没有内置默认主题，需要自行安装主题文件，否则页面无法渲染。这里选用仿 Ghost 的主题 [Casper](https://github.com/vjeantet/hugo-theme-casper)。

```bash
$ cd /var/www/qaq.money
$ mkdir themes
$ cd themes
$ git clone https://github.com/vjeantet/hugo-theme-casper casper
```

#### 配置文件

Hugo 的配置文件是位于站点根目录的`config.toml`，我们这里更改`baseUrl`这个参数：

```toml
BaseUrl = "http://qaq.money/"
```

由于 Hugo 没有自带的默认主题，我们需要在最后加上一行：

```toml
theme = "Casper"
```

来指定我们需要使用的主题。

Casper 主题的一些设置也需要编辑该文件进行更改，具体参阅 [Casper 的文档](https://github.com/vjeantet/hugo-theme-casper)。

#### 创建页面

```bash
$ cd /var/www/qaq.money
$ hugo new post/first_post.md
```

这个命令会在站点根目录的`content`文件夹内创建一个`post`文件夹，并放入`first_post.md`。Hugo 支持以 YAML 格式作为 header 的 Markdown 语法文章。

#### 生成静态站点

有了页面之后，在网站的根目录可以通过这个简单的命令：

```bash
$ hugo
```

来生成静态网站，此时根目录下会生成一个`public`文件夹，静态网站的内容都位于其中了。此时使用 Nginx 对`public`目录下的`index.html`进行渲染就可以得到一个网站。然而每次创作之后都需要使用`hugo`命令重新生成一次网页。下面我们通过 Caddy 将这个过程自动化。



### 安装并配置 Caddy

[点此](https://caddyserver.com/download)进入 Caddy 的下载页面，选择好操作系统、插件和授权之后，页面下方会生成一键安装命令，这里使用集成了 http-git 和 http-hugo 插件的 Linux 64位版本。

```bash
$ curl https://getcaddy.com | bash -s personal http.git,http.hugo
```

同样会自动将 Caddy 安装到`$PATH`路径。

#### 设置开机自启动

这里使用 sysvinit 实现自启动。

```bash
$ mkdir /etc/init.d/caddy
$ cd /etc/init.d/caddy
$ wget 
```











### 遇到的坑

#### 网站首页能显示但是看不见文章

之前参考 Hugo 的 quickstart 教程，md 格式的文章都放在 `content/posts`内，之后发现是使用的主题 Casper 默认的渲染路径是`content/post`，将所有文章都迁移到`content/post`文件夹内问题解决。