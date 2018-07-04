---
title : "一键安装SS+BBR科学上网"
date  : "2018-07-02T03:06:20Z"
draft : false
---

---

**在开始之前，首先需要一个运行 Linux 的服务器，一般作 SS 用的是国外（或者香港的）VPS。**



### 搭建 Shadowsocks 服务器

#### 下载 Shadowsocks

进入服务器的 ssh 页面，输入以下代码

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



其中 PASSWORD 是需要自定义的 SS 服务器端的密码，PORT 是自定义的端口（1-65535）

如果填错了，只需要再运行一遍这一步的代码。



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



### 一键开启 BBR

BBR是 Google 开源的一套内核加速算法，装就对了。使用如下代码

```bash
ss-fly/ss-fly.sh -bbr
```

重启服务器后即可开启加速。可以在安装完成后按 y 立即重启，也可以稍后使用`sudo reboot`重启。

重启后判断 BBR 是否开启，使用以下命令

```bash
sysctl net.ipv4.tcp_available_congestion_control
```

如果返回值里面有`bbr`，例如

```bash
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```

则说明已经开启成功了，enjoy。

