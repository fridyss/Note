# ubuntu 根文件系统移植

  	将ubuntu根文件系统移植到Arm平板imx6ull开发板。

## 1,安装环境

- ubuntu 虚拟机

- imx6ull 开发板

- QEUM工具

  

## 2,准备ubuntu根文件系统

​	根据自己所需，到ubuntu官网（http://cdimage.ubuntu.com ）下载对应的版本号，进入 /ubuntu-base/releases/18.04.5/release/路径下载ubuntu-base-18.04.5-base-arm64.tar.gz文件。

​	创建目录用于文件 ubuntu-base-18.04.5-base-armhf.tar.gz解压。

```
mkidr ubfs18.04 
cd  ./ubfs18.04
```

​	解压文件ubuntu-base-18.04.5-base-armhf.tar.gz， 需要在解压命令前增加sudo, 使拥有组和所属组都为root用户。

```
sudo tar xzvf ubuntu-base-18.04.5-base-arm64.tar.gz
```

## 3,安装QEUM仿真器

​	qemu-arm-static 工具，能在主机（ubuntu）上虚拟ARM环境，通过在PC端进入宿主机的程序安装， 可以有效避免交叉编译带的繁琐和出错的问题。将安装的qemu-arm-static复制到ubuntu根文件系统的/usr/bin路径下。

```
sudo apt-get install qemu-user-static
sudo cp /usr/bin/qemu-arm-static ./usr/bin/
```

## 4,配置根文件系统网络

- ​	更新apt软件源，修改 ./etc/apt/sources.list, 增加arm软件源。

```bash
sudo vim ./etc/apt/sources.list
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
```

- 复制主机/etc/resolv.conf到宿主机/etc/resolv.conf，这样在qemu环境下，安装联网，进行软件安装。

```
sudo cp /etc/resolv.conf./etc/resolv.conf
```

## 5,配置QEMU

- 创建ms.sh脚本。

```
touch ms.h
```

```shell
#!/bin/bash
mnt () 
{
    echo "MOUNTING"
    sudo mount -t proc /proc ${2}proc
    sudo mount -t sysfs /sys ${2}sys
    sudo mount -o bind /dev ${2}dev
    sudo mount -o bind /dev/pts ${2}dev/pts      
    sudo chroot ${2}    
}
umnt ()
{
    echo "UNMOUNTING"
    sudo umount ${2}proc
    sudo umount ${2}sys
    sudo umount ${2}dev/pts
    sudo umount ${2}dev 
}

if [ "$1" = "-m" ] && [ -n "$2" ];
then
    mnt $1 $2
    #echo "mnt -m pwd"
elif [ "$1" = "-u" ] && [ -n "$2" ];
then
    umnt $1 $2
    #echo "mnt -u pwd"
fi
```

​	chroot 命令全称Change Root， 也就是改变程序执行时所参考的根目录位置，使用apt安装应用以切换后的目录路径。

- 运行ms.h,  ./ubfs18.04 为ubuntu根文件系统所在的相对路径。

```
sudo ./ms.h -m ./ubfs18.04 
```

- 输入ls命令后，显示ubuntu根文件系统成功

```bash
root@ubuntu:/# ls
bin   etc   media  proc  sbin  tmp				   var
boot  home  mnt    root  srv   ubuntu-base-18.04.5-base-armhf.tar
dev   lib   opt    run	 sys   usr
```

- 退出ubuntu根文件系统，可以按下面命令操作

```
root@ubuntu:/# exit
sudo ./ms.sh -u ./ubfs18.04/
```

## 6,安装软件

​	根据自己的需要，利用apt安装vim, sudo, ssh,  iputils-ping (ping命令)， net-tools ，htop ，ethtool 。

```
apt update #更新软件源
apt install vim  sudo ssh iputils-ping net-tools htop ethtool 
```

## 7,用户配置

- 配置root用户密码

```bash
passwd root
```

- 增加用户

```
adduser fridy
```

- 配置本机名称和本机IP

```bash
echo "imx6ulll" > /etc/hostname
echo "127.0.0.1 localhost" >> /etc/hosts
echo "127.0.1.1 imx6ulll" >> /etc/hosts
```

## 8,配置控制台

​	增加配置imx6ull控制台信息为物理串口，针对imx6ull芯片平台设置如下。

```bash
ln -s /lib/systemd/system/getty@.service /etc/systemd/system/getty.target.wants/getty@ttymxc0.service
```

## 9,配置DHCP

```bash
echo auto eth0 > /etc/network/interfaces.d/eth0
echo iface eth0 inet dhcp >> /etc/network/interfaces.d/eth0
reboot
```

## 10,配置DNS

​	Ubuntu 18.04 永久修改DNS的方法。

```
sudo vim /etc/systemd/resolved.conf 

[Resolve]
DNS=114.114.114.114 #增加dns服务
DNS=8.8.8.8
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes
```

## 11,sudoers文件增加用户

​	如果不增加用户组，自定义用户无法使用sudo命令。

```
vim /etc/souders

fridy   ALL=(ALL:ALL) ALL # "fridy" is username
```

完成上面的配置就可以将ubfs18.04的根文件系统通过nfs挂载到imx6ull开发板，进行测试了。
