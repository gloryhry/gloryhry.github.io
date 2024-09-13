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
  2. 进入windows的安装流程。
  3. 等待windows安装完成。
  4. 激活windows，可参考[KMS激活](https://github.com/netnr/kms)。

## 安装docker和WSL2

  1. [可选] 下载安装[Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
  2. 安装WSL2，可参考[Windows官方教程](https://learn.microsoft.com/zh-cn/windows/wsl/install)
  3. `wsl --list --online`可查看可用发行版列表，`wsl --install -d <DistroName>`以安装一个发行版.
  4. 终端中输入`wsl`即可进入linux发行版
     
