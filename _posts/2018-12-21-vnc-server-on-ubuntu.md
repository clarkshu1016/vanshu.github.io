---
layout: post
title:  "Vnc server on ubuntu"
date:   2018-12-21
keywords: "ubuntu,linux,vncserver,vnc"
categories: [Linux]
tags: [Linux]
icon: icon-linux-mint
---

* 开启start.sh      ```restart.sh```
```console
#!/bin/bash
vncserver -kill :1
sleep 3
vncserver -alwaysshared :1 && netstat -nlpt|grep vnc
```

* 在服务端将客户端的公钥内容写入./ssh/authorized_keys 文件中，默认是没有这个文件的需要新建.ssh文件夹和authorized_keys文件

* 如果需要恢复之前都密码登录方式，可以选择移除公钥即可,客户端输入以下命令表示使用客户端机器都私钥去校验服务端公钥.
```
ssh root@xxx.xxx.xxx.xxx 
```

* 如果在客户端删除了私钥，但客户端依然保留了公钥，依然无法登录
```bash
$ ssh root@xxx.xxx.xxx.xxx             
root@xxx.xxx.xxx.xxx: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```