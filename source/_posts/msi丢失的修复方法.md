---

layout: 
title: msi丢失的修复方法
date: 2026-07-13 23:00:01
tags:

---

msi文件丢失的修复方法

<!-- more -->

![](https://cdn.jsdelivr.net/gh/hachiroku-lose/hachiroku-lose.github.io@image/blog-image/Microsoft%20Visual%20C%2B%2B%202022%20msi%20error.jpg)

下载一个 [Windows Installer CleanUp Utility](https://www.download3k.com/Install-Windows-Installer-CleanUp-Utility.html) [备用链接](https://cdn.jsdelivr.net/gh/hachiroku-lose/hachiroku-lose.github.io@main/msicuu2.zip)

安装好了后在开始菜单找到Windows Install Clean Up,打开

remove掉与c++运行库有关的项

重新安装 [微软常用运行库合集](https://www.ghxi.com/yxkhj.html)

此时应该就不会出现报错了,全程不需要重启电脑
