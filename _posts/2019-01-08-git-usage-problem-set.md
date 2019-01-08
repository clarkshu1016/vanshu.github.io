---
layout: post
title:  "Git问题合集"
date:   2019-01-08
keywords: "ubuntu,linux,android,system,rom"
categories: [Android]
tags: [Linux,Android]
icon: icon-mobile-device
---

* 误删了本地的文件，但是没有git commit ,也没有push到远程仓库

查看被删除的文件
```console
 git ls-files --deleted
```
```ssh
test@test-Vostro-3459:~/android_8.1.0/packages/experimental$  git ls-files --deleted
ToggleFlightMode/.gitignore
ToggleFlightMode/Android.mk
ToggleFlightMode/AndroidManifest.xml
ToggleFlightMode/res/layout/activity_main.xml
ToggleFlightMode/res/mipmap-hdpi/ic_launcher.png
ToggleFlightMode/res/mipmap-hdpi/ic_launcher_round.png
ToggleFlightMode/res/mipmap-mdpi/ic_launcher.png
ToggleFlightMode/res/mipmap-mdpi/ic_launcher_round.png
ToggleFlightMode/res/mipmap-xhdpi/ic_launcher.png
ToggleFlightMode/res/mipmap-xhdpi/ic_launcher_round.png
ToggleFlightMode/res/mipmap-xxhdpi/ic_launcher.png
ToggleFlightMode/res/mipmap-xxhdpi/ic_launcher_round.png
ToggleFlightMode/res/mipmap-xxxhdpi/ic_launcher.png
ToggleFlightMode/res/mipmap-xxxhdpi/ic_launcher_round.pn
```

如果要恢复多个被删除的文件，可以使用批处理命令：
```console
git ls-files -d | xargs git checkout --
```
