---
layout: post
title:  "Android 编译问题集"
date:   2018-12-06
desc: ""
keywords: "android,rom,jackserver,ubuntu"
categories: [Android]
tags: [android,rom,ubuntu]
icon: icon-mobile-device
---

###  [错误一 jack server问题]{https://forum.xda-developers.com/android/software/aosp-cm-los-how-to-fix-jack-server-t3575179}

```
[  0% 3/59149] Ensuring Jack server is installed and started
Jack server already installed in "/home/vanshu/.jack-server"
Launching Jack server java -XX:MaxJavaStackTraceDepth=-1 -Djava.io.tmpdir=/tmp -Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx2G -cp /home/vanshu/.jack-server/launcher.jar com.android.jack.launcher.ServerLauncher
[  0% 4/59149] //art/tools/cpp-define-generator:cpp-define-generator-data clang++ main.cc [linux]
ninja: build stopped: subcommand failed.
18:06:07 ninja failed with: exit status 1
```