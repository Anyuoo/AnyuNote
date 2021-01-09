---
title: Linux文件系统操作命令 
date: 2020-02-28 15:31:24
comment: true
categories: 
- Linux

tags: 
- Centos
- Linux文件处理命令
---
#### 1.  **ls**  `list` 显示目录文件  命令所在路径 : */bin/ls*
```bash
语法：ls 选项[-ldi] [文件或目录]
* -l `long` 长格式显示，详细信息显示
* -h `human` 以人可读方式显示数据大小
* -i `iNode` 显示文件的i节点号
* -d `direction` 查看目录属性
* -a `all` 显示所有文件，包括隐藏文件
```
`ps:设立隐藏文件主要为了将系统文件隐藏，避免用户误操作`

#### 2. **mkdir** `make directeries` 创建目录 命令所在路径：*/bin/mkdir*
    语法：mkdir 选项[-p] [目录名称]
    * -p 递归创建多级文件夹
#### 3. **rmdir**` remove directeries` 删除空目录 命令所在路径：*/bin/rmdir*
    语法：rmdir [目录名]
#### 4. **rm** `remove` 删除目录或文件 命令所在路径：*/bin/rm*
    语法：rm 选项[-rf] [目录名或文件名]
    * -r 递归删除，一次删除多级文件
    * -f `force` 强制执行
#### 5.**cp** `copy` 复制文件或目录 命令所在路径：*/bin/cp*
    语法：cp -rp [原文件或目录] [目标目录]
    * -r 复制目录
    * -p 保留文件属性
#### 6.**touch** 创建空文件夹 命令所在路径：*/bin/touch*
    语法：touch [文件名]
#### 7. **cat** 显示文件内容 命令所在路径：*/bin/cat*
    语法：cat 选项[-n b] [文件名]
    * -n `number` 由 1 开始对所有输出的行数编号
    * -b `number nonblan` 和 -n 相似，只不过对于空白行不编号
#### 8. **more** 分页显示文件内容 命令所在路径：*/bin/more*
    语法：more 选项[-num +num ] [文件名]
    * -num `number` 一次显示的行数
    * +num `number` 从第 num 行开始显示
    常用操作命令：
    * Enter 向下n行，需要定义。默认为1行
    * Ctrl+F 向下滚动一屏
    * 空格键 向下滚动一屏
    * Ctrl+B 返回上一屏
    * = 输出当前行的行号
    * ：f 输出文件名和当前行的行号
    * V 调用vi编辑器
    * !命令 调用Shell，并执行命令
    * q 退出more
#### 9. **less** less 与 more 类似，但使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动，而且 less 在查看之前不会加载整个文件
    语法：less [文件名]
#### 10. **head** 显示文件前几行 命令所在路径：*usr/bin/head*
    语法：head [文件名]
    * -n 指定行数
#### 11. **tail** 显示文件后面几行 命令所在路径：*/usr/bin/tail*
    语法：tail [文件名]
    * -n 指定行数
    * -f 动态显示



