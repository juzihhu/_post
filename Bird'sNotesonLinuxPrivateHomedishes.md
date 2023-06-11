---
title: 鸟哥的Linux私房菜笔记
date: 2022-05-28 17:04:21
tags: Linux
categories: Linux
cover: /img/photo-1676973439783-c95e2250c232.png
---
记录《鸟哥的Linux私房菜》简要笔记
<!-- more -->   

# 0. 计算器概论
# 1. Linux是什么与如何学习
# 2. 主机规划与磁盘分区
# 3. 安装CentOS7.x
# 4. 首次登录与在线求助

## 4.2 文本模式下指令的下达

1.指令格式：command options parameters
2.基础指令操作：
    date  (`date +%Y/%m/%d`)
    cal 
    bc 计算机  quit退出

3. 几个简单热键
   

**Tab**
[Tab]接在一串指令的第一个字的后面，则为“命令补
[Tab]接在一串指令的第二个字以后时，则为“文件补
[tab]在某些指令后面使用按键时，可以进行“选项/参数的补齐”功能

例如： ca[Tab][Tab]

```sh
panan@ecs-kc1-large-2-linux-20220314145535:~$ ca 
.bash_history              .config/                   .lesshst                   .npm/                      .ssh/                      .vscode-remote-containers/ .Xauthority
.bash_logout               .ghost/                    Linux/                     .profile                   update                     .vscode-server/            .yarn/
.bashrc                    .gitconfig                 .local/                    quit                       .viminfo                   webserver/                 
.cache/                    .gnupg/                    .mysql_history             .rpmdb/                    .vscode/                   .wget-hsts             
```
**[Ctrl]+C**
中断目前程序的按键
**[Ctrl]+D**
键盘输入结束

**[shift]+{[PageUP]|[Page Down]按键**
使用[Shift]+[Page Down]来往后翻页！

**--help**
**man**
在终端机模式中，如果你知道某个指令，但却忘记了相关选项与参数，请先善用--help的功能来查询相关信息；

当有任何你不知道的指令或文件格式这种玩意儿，但是你想要了解他，请赶快使用man或者info来查
询！

而如果你想要架设一些其他的服务，或想要利用一整组软件来达成某项功能时，请赶快到/usr/share/doc下面查一查有没有该服务的说明文档

# 5. 文件权限与目录配置
