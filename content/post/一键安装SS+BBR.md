---
title : "一键安装SS+BBR科学上网"
date  : "2018-07-03T03:06:20Z"
draft : false
---

---

**在开始之前，首先需要一个运行 Linux 的服务器，一般作 SS 用的是国外（或者香港的）VPS。**



## 搭建 Shadowsocks 服务器

#### 下载 Shadowsocks

进入服务器的 ssh 页面，输入以下代码[^注]

[^注]: 这里选用的是 flyzy 的一键搭建脚本，后同

```bash
 git clone https://github.com/flyzy2005/ss-fly 
```



如果提示 `bash: git: command not found`，则需要先安装 git，

```bash
CentOS          执行: yum -y install git
Ubuntu / Debian 执行: apt-get -y install git
```



#### 安装 Shadowsocks

```bash
ss-fly/ss-fly.sh -i PASSWORD PORT
```



其中 PASSWORD 是 SS 的密码，PORT 是自定义的端口（1-65535）

如果填错了，只需要再运行一遍刚才的代码。



#### 相关 SS 操作

```bash
修改配置文件 ：vim /etc/shadowsocks.json
停止ss服务  ：ssserver -c /etc/shadowsocks.json -d stop
启动ss服务  ：ssserver -c /etc/shadowsocks.json -d start
重启ss服务  ：ssserver -c /etc/shadowsocks.json -d restart
```



#### 卸载 SS

```bash
ss-fly/ss-fly.sh -uninstall
```

