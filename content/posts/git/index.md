---
title: "Git 无法连接 GitHub解决办法​"
date: 2026-05-02
draft: false
---
Git 无法连接 GitHub，开了代理也不行，是因为 Git 没有走代理的流量，需要手动给 Git 配置代理端口。​
这是最常见的问题：系统代理开了，但 Git 默认不读取系统代理设置，它的网络请求走的是自己的通道，所以需要单独告诉 Git 该用哪个端口。

# 第一步：确认你的代理端口
打开你的梯子软件，找到本地监听端口，通常在软件设置里叫"本地端口"、"HTTP 端口"或"混合端口"。常见默认值：

Clash：7890
V2RayN：10809
Shadowsocks：1080

记住这个端口号，下面要用。

# 第二步：给 Git 配置代理
打开 PowerShell，执行（把 7890 换成你实际的端口）：
```
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```
配置完后再试一次：
```
git push
```
如果还是不行，试试 SSH 方式推送
方法请自行百度。