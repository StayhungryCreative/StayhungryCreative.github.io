---
title: imu_tk配置教程
date: 2024-11-07 21:00:00
excerpt: imu标定工作的环境配置步骤
tags:
  - imu标定
  - Ubuntu
categories:
  - 工具
---



说明：**请在ubuntu18.04上安装，ubuntu22.04不可以！！！** 因为imu_tk 的 cmake 必须要qt4.x，但 ubuntu22.04 和qt4.x不适配。



## 1、安装 ceres-solver

下载路径：http://ceres-solver.org/installation.html   （需要梯子，核心内容记录如下。需下载 ceres-solver 安装包）

Start by installing all the dependencies.

```bash
# CMake
sudo apt-get install cmake
# google-glog + gflags
sudo apt-get install libgoogle-glog-dev libgflags-dev
# Use ATLAS for BLAS & LAPACK
sudo apt-get install libatlas-base-dev
# Eigen3
sudo apt-get install libeigen3-dev
# SuiteSparse (optional)
sudo apt-get install libsuitesparse-dev
```

We are now ready to build, test, and install Ceres.

```bash
tar zxf ceres-solver-2.2.0.tar.gz
mkdir ceres-bin
cd ceres-bin
cmake ../ceres-solver-2.2.0
make -j3    # 有的地方写 -j2，不清楚区别，先按官方的来
make test
# Optionally install Ceres, it can also be exported using CMake which
# allows Ceres to be used without requiring installation, see the documentation
# for the EXPORT_BUILD_DIR option for more information.
sudo make install
```



## 2、安装imu_tk

下载路径：https://github.com/Kyle-ak/imu_tk

```bash
sudo apt-get install build-essential cmake libeigen3-dev libqt4-dev libqt4-opengl-dev freeglut3-dev gnuplot

cd imu_tk
mkdir build
cd build
cmake  ..
make
```

### cmake不通过，debug

1、没有装boost

```bash
sudo apt-get update
sudo apt-get install libboost-all-dev
```

2、找不到eigin
参照：https://blog.csdn.net/qq_43872529/article/details/100937091

```bash
sudo ln -s /usr/include/eigen3/Eigen /usr/include/Eigen
```





## 尾巴

> 一开始就该好好看 imu_tk 的说明，能少走好多弯路。**慢就是快，偷懒不解决的问题后面爆雷还要解决。**



走的弯路：

https://blog.csdn.net/qq_36267302/article/details/127423884

https://blog.csdn.net/CAS_sweet/article/details/143331300 (这个作者对问题做了初步分析，学习ta的思路)——这个做法不可取，治标不治本！后面的方法才对。



在ubuntu22.04上试图安装imu_tk，报错如下：

![image](/images/image.png)

![image1](/images/image1.png)

boost没有安装，cmake报错如下：

![image2](/images/image2.png)

