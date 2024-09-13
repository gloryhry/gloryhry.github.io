---
title: 重新安装windows和配置wsl2环境
tags: Windows WSL2
key: reinstallWindowsAndSetupWSL2
---

****************************************

## 使用Rufus安装windows的安装U盘

 1. 去[Windows11 官方下载地址](https://www.microsoft.com/zh-cn/software-download/windows11)下载对应版本的Windows11系统。
 2. 选择```Windows 11 (multi-edition ISO)```、```简体中文```、```64-bit Download```版本，之后下面给出24小时下载链接。下载ISO文件。
 3. 下载[Rufus](https://rufus.ie/zh/)，可以选择```便携版```,直接打开就能用。
 4. 打开Rufus，插入8G以上的U盘，在Rufus的```设备```中选择U盘，```引导类型选择```后面选择下载的ISO文件路径。```镜像选项```选```标准Windows安装```，其他默认，直接点击开始，弹出的选项框选默认配置。等待安装U盘配置完成。
 5. 直接拔下U盘，关闭电脑。

## 安装Windows

  1. 插入安装U盘后，开机进入BIOS，选从U盘启动。
  2. 
