---
layout: post
title:  "android selinux 权限问题"
date:   2018-11-28
desc: "Quick test on writing code snippets in a blog post"
keywords: "android,selinux,ssh"
categories: [Android]
tags: [android,selinux,ssh]
icon: icon-mobile-device
---


安装好termux app后，第一次启动会在系统files目录下 生成/home 和/usr 目录，
在终端需要安装好sshd ，才会在/home/.ssh 生成目录 ，该目录下会有 authorized_keys 文件，是授权文件
如果需要登录该机器，需要将公钥拷贝到该authorized_keys，拷贝命令如下
echo "这里是公共钥匙的内容">> authorized_keys

确保里面没有空格符号，可以通过echo 追加进去文件，如果追加错误，可以通过:>authorized_keys 清空该文件内容，注意是清空文件内容不是删除文件,我做错了一步就是以root用户删除了该文件，重新创建了新的文件，该文件权限变成了归属用户root了，可以通过 su u0_a263 切换回u0_a263 用户，然后创建了新的authorized_keys ，发现归属用户对了，通过命令chmod 600 修改用户为 所要的权限 4表示read ，2 表示write ，1 表示execute ，600就是表示 所属用户可以读写，但是同一用户组，和其他用户组的不能读写执行,按道理事情到这一步应该结束了，可是发现授权文件虽然权限改对了，但还是无法登录，登录出现需要密码提示的情况，但是我们设置的公钥根本不需要密码登录，猜测是sexlinux权限问题，adb shell 进去查看 getenforce ，返回值为 Enforcing，通过setenforce 0 修改成为 Permissive (宽容模式) 但不会关闭，如果重启后会重新变成Enforcing 模式。通过
ls -laZ 查看到 authorized_keys 和其他文件夹内容不一样 ,少了c512，c768权限，但是目前还不知道怎么添加该权限
事情最后处理结果就是通过先关闭了selinux，ssh远程登录该手机，创建了新的authorized_keys，然后下次重新selinux虽然开启了，但是authorized_keys文件已经有了 u:object_r:app_data_file:s0:c512,c768了,这样子就可以登录了，所以sexlinux 的权限设置非常严格.
```
-rw------- 1 u0_a263 u0_a263 u:object_r:app_data_file:s0 16 2018-11-28 12:24 authorized_keys
```

```
bullhead:/data/data/com.termux # ls -Zla
total 32
drwx------   6 u0_a263 u0_a263       u:object_r:app_data_file:s0:c512,c768 4096 2018-11-27 17:26 .
drwxrwx--x 163 system  system        u:object_r:system_data_file:s0        8192 2018-11-27 17:25 ..
drwxrws--x   2 u0_a263 u0_a263_cache u:object_r:app_data_file:s0:c512,c768 4096 2018-11-27 17:25 cache
drwxrws--x   2 u0_a263 u0_a263_cache u:object_r:app_data_file:s0:c512,c768 4096 2018-11-27 17:25 code_cache
drwx------   4 u0_a263 u0_a263       u:object_r:app_data_file:s0:c512,c768 4096 2018-11-27 17:26 files
drwxrwx--x   2 u0_a263 u0_a263       u:object_r:app_data_file:s0:c512,c768 4096 2018-11-28 11:29 shared_prefs
```









