# 更新python版本教程



## 1,安装依赖包

```
sudo apt update
sudo apt install software-properties-common
```

## 2,添加 deadsnakes PPA 源

```
sudo add-apt-repository ppa:deadsnakes/ppa
...
- Ubuntu 18.04 (bionic) Python2.3 - Python 2.6, Python 3.1 - Python 3.5, Python3.7 - Python3.11
- Ubuntu 20.04 (focal) Python3.5 - Python3.7, Python3.9 - Python3.11
- Ubuntu 22.04 (jammy) Python3.7 - Python3.9, Python3.11
- Note: Python2.7 (all), Python 3.6 (bionic), Python 3.8 (focal), Python 3.10 (jammy) are not provided by deadsnakes as upstream ubuntu provides those packages.
```

## 3, 安装python

- ​	查看当前版本号

```
# lsb_release -a

Distributor ID: Ubuntu
Description:    Ubuntu 18.04.5 LTS
Release:        18.04
Codename:       bionic
root@imx6ulll:~#
```

- 安装 python3.7

```
sudo apt-get install python3.7
```

- 重新映射python3软连接

```
ln -s python3.7 python3
ln -s python3.7m pyton3m
```

- 查看python3版本号 

```bash
python3 --version
Python 3.7.16
```



## 4,安装pip3

```bash
sudo apt install python3-pip
pip3 -V #验证安装
pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.7)
```

## 5,更改pip3镜像源

1. ​	清华：https://pypi.tuna.tsinghua.edu.cn/simple
2.   中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
3.   华中理工大学：http://pypi.hustunique.com/
4.   山东理工大学：http://pypi.sdutlinux.org/
5.   豆瓣：http://pypi.douban.com/simple/

```bash
mkdir ~/.pip
vim ~/.pip/pip.conf
输入
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
```

