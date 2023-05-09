---
title: NVIDIA_Jetson_Xavier_NX开发板EMMC版本刷固件方法
tags: JetsonNX EMMC
key: NVIDIA_Jetson_Xavier_NX_EMMC_FLASH
---

****************************************

# NVIDIA Jetson Xavier NX开发板EMMC版本刷固件方法

## 准备

- linux系统，最好64位
- USB线
- 一根杜邦线或跳帽
- 鼠标键盘，显示器（板子刷好后，连接板子）

## 官方固件下载

英伟达官网L4T [下载页面](https://developer.nvidia.com/embedded/jetson-linux-archive) 给出了系列的开发下载资源，对于 Jetson Xavier NX 系统烧录，我们需要下载几个：

- Sample Root Filesystem：[JetPack4.4.1&&L4T_R32.4.4](https://developer.nvidia.com/embedded/L4T/r32_Release_v4.4/r32_Release_v4.4-GMC3/T186/Tegra186_Linux_R32.4.4_aarch64.tbz2)
  
  对应文件："Tegra_Linux_Sample-Root-Filesystem_R32.4.4_aarch64.tbz2"

- L4T Driver Package (BSP)：[JetPack4.4.1&&L4T_R32.4.4](https://developer.nvidia.com/embedded/L4T/r32_Release_v4.4/r32_Release_v4.4-GMC3/T186/Tegra_Linux_Sample-Root-Filesystem_R32.4.4_aarch64.tbz2)
  
  对应文件："Tegra186_Linux_R32.4.4_aarch64.tbz2"

- Overlay to support Jetson Xavier NX 16GB： [JetPack4.4.1&&L4T_R32.4.4](https://developer.nvidia.com/xnx-16gb-overlaytbz2) (仅16GEMMC版本需要)
  
  对应文件："XNX-16GB-overlay.tbz2"

## 解压固件、构建文件系统

```bash
# 建立工作目录
mkdir nx
# 安装qemu
sudo apt-get install qemu-user-static
# 拷贝下载的固件到工作目录/nx下
cp Tegra_Linux_Sample-Root-Filesystem_R32.4.4_aarch64.tbz2 ./nx/
cp Tegra186_Linux_R32.4.4_aarch64.tbz2 ./nx/
cp XNX-16GB-overlay.tbz2 ./nx/

cd ./nx/
tar xf Tegra186_Linux_R32.4.2_aarch64.tbz2
tar xf XNX-16GB-overlay.tbz2
tar xf xnx-16gb-overlay.tbz2

cd Linux_for_Tegra/rootfs/
sudo tar xpf ../../Tegra_Linux_Sample-Root-Filesystem_R32.4.4_aarch64.tbz2
cd ..
sudo ./apply_binaries.sh
```

等待执行结果，最后会有个打印：

```bash
   ...
   Installing extlinux.conf into /boot/extlinux in target rootfs
   Success!
```

完成以上所有步骤就可以准备烧录了。

## 烧录系统

- 使用usb连接线连接 虚拟机 和 Jetson Xavier NX

- 在核心板下方有个名为 “FC_REC” 的引脚 ，其可以控制系统上电后进入recovery模式，使用杜邦线或者跳线帽将其和相邻的GND引脚短接。
  
  ![img](https://img-blog.csdnimg.cn/20200919190803838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ1MzE4MDQ=,size_16,color_FFFFFF,t_70#pic_center)

- 使用DC 5V给板子上电
- 在虚拟机环境下输入lsusb命令，可以查看到：
  
```bash
lsusb
```

```bash
   Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
   Bus 003 Device 004: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
   Bus 003 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
   Bus 003 Device 054: ID 0955:7020 NVidia Corp.      <-------------- 这里就是 nx板子
   Bus 003 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
   Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
   Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
   Bus 002 Device 002: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
   Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
```

- 执行烧录命令:

```bash
sudo ./flash.sh p3449-0000+p3668-0001-qspi-emmc mmcblk0p1
```

接下来耐心等待，大约10分钟，系统会自动擦除emmc，并将固件写入。
最后可以观察到如下打印：

```bash
  ...
   *** The target t186ref has been flashed successfully. ***
   Reset the board to boot from internal eMMC.
```

- 将 “FC_REC” 引脚和GND短接的线去掉，拔掉 DC5V，重新上电，接上HDMI和鼠标键盘。
- 系统会自动进入配置页面，要求接受一些协议，初始化用户名什么的，正常配置下即可。
- 系统启动配置页面,之后就可以正常使用系统了。