### *Ubuntu*

1、进入终端窗口（黑黑的那个）

2、卸载掉gnome-shell主程序

sudo apt-get remove gnome-shell 

3、卸载掉gnome

sudo apt-get remove gnome 

4、卸载不需要的依赖关系

sudo apt-get autoremove 

5、彻底卸载删除gnome的相关配置文件

sudo apt-get purge gnome

6、清理安装gnome时候留下的缓存程序软件包

sudo apt-get autoclean

sudo apt-get clean

### *Centos:*

yum grouplist 查看安装了什么图形软件(不查也行直接全试一遍)



1.卸载`GNOME`桌面环境
yum groupremove "GNOME Desktop Environment"

2.卸载`KDE`桌面环境
yum groupremove "KDE (K Desktop Environment)"

2.卸载`Xwindows`桌面环境
yum groupremove 'X Window System' -y