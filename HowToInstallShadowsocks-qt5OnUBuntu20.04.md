# How To Install Shadowsocks-qt5 On Ubuntu 20.04
# 如何在 ubuntu 20.04 上使用shadowsocks图形化客户端shadowsocks-qt5
Update: 10 Jan 2022
转载不用注明出处，但需标明非原创。

网络上有不少相关教程，但不再适用于ubuntu 20.04。以下是针对该版本系统的教程，也适用于18.04 bionic的，仅需把所有提到focal的部分看作bionic。

在旧版本系统上就三行代码
```sh
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
第一行添加源，第二行更新源，第三行安装程序

在20.04系统上会卡在第二步报错，当然第三步就无法找到packages了。以下是提供操作，针对小白

```sh
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo gedit /etc/apt/sources.list
# 打开窗口，把以下源地址修改为
# deb http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu focal Release
deb http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu zesty Release
# 原理：shadowsocks-qt5在PPA上未发布20.04系统的版本，于是下载时会找不到包。改为17.04的版本即可。20.04系统叫focal, 17.04叫zesty。同理，想改成xnial也可以，但尽量用新版本。
# 进入目录编辑
cd /etc/apt/sources.list.d
# 看下目录里是否有叫 hzwhuang-ubuntu-ss-qt5-focal.list 和 hzwhuang-ubuntu-ss-qt5-focal.list.save 的文件，如有，继续操作。若无，自行查找解决办法。
ls
# 将两个文件改名为....-zesty.list(.save)
sudo mv -i hzwhuang-ubuntu-ss-qt5-focal.list hzwhuang-ubuntu-ss-qt5-zesty.list
sudo mv -i hzwhuang-ubuntu-ss-qt5-focal.list.save hzwhuang-ubuntu-ss-qt5-zesty.list.save
# 打开两个文件，将内部所有的focal改为zesty (此处仅举例一个文件)
sudo gedit hzwhuang-ubuntu-ss-qt5-zesty.list
# 改完后内容应该如下
deb http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu zesty main
# deb-src http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu zesty main
```
接下来再update软件列表应当就能成功了

如何使用和如何添加代理参见 https://cjh.zone/2018/11/25/科学上网-Linux-Ubuntu-16-04-配置shadowsocks客户端/#proxy

如何添加gfw.pac参见 https://github.com/zhiyi7/gfw-pac 
```sh
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

如果出现了依赖问题，继续往下看

原理就是依次下载并安装缺失的包

如果是libqrencodes3缺失
```sh
# 打开网页 https://packages.ubuntu.com/bionic/amd64/libqrencode3/download ，选择对应的镜像下载，然后进入下载文件夹安装
cd Downloads
sudo dpkg -i libqrencode3_3.4.4-1build1_amd64.deb
```

其他缺失的思路一样，搜索缺失的包在哪个网站上，下载，安装。搞定依赖的问题后就可以安装ss-qt5了。

## Reference
思路来源：
https://bbs.csdn.net/topics/392381640