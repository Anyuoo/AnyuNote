---
title: Linux权限命令（1）--chmod
date: 2020-03-02 20:07:11
comment: true
categories: Linux
tags: Centos
---
chmod `change the permissions mode od a file`改变文件或目录权限 命令路径：*/bin/chmod*
语法：

    * chmod [{ugoa}{+-=}{rwx}] [文件或目录]
    * chmod [mode=421 ] [文件或目录]
    
参数：

    * u `user` 所有者
    * g `group` 所属组
    * o `other` 其他人
    * a `all` 所有人
    * r `read`读权限 数字表示-->**4**
    * w `write` 写权限 数字表示-->**2**
    * x `execute` 执行权限 数字表示-->**1**
    * -R 可递归赋给子文件夹相同权限
    
![chmod](Linux--chmod/chmod.chmod.jpg)

由图可知，文件夹 *Videos* 权限是 **rwxr-xr-x** ， 每三个一组，从左到右分别是

    * 所有者权限：rwx `4+2+1=7`
    * 所有组权限：r-x `4+1=5`
    * 其他人权限：r-x `4+1=5`
    
用数字赋值权限：chmod 755 [文件或目录]
    对于目录：rx代表可以查看和进目录，权限一般同时出现
    对于目录中文件的删除执行不是看文件权限而是看目录权限