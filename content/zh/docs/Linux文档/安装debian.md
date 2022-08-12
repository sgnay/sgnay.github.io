---
title: 安装debian
description: 在kvm中安装debian虚拟机
toc: true
authors: [sgnay]
date: 2022-05-15T16:17:51+08:00
lastmod: 2022-05-15T16:17:51+08:00
draft: false
weight: 1
featureVideo: https://player.bilibili.com/player.html?aid=511542425&bvid=BV1uu41167Yp&cid=720945908&page=1
---

# 安装debian虚拟机

通过网络在kvm中安装debian11

## 下载镜像

- 进入官网选择网络镜像下载[https://www.debian.org/download](https://www.debian.org/download) ![download_debian_iso](/images/安装debian/download_debian_iso.png)
- 镜像下载完成后放入virt-manager的镜像库目录。

## virt-manager创建虚拟机

- 打开virt-manager界面，点击创建新虚拟机 ![kvm_start_install](/images/安装debian/kvm_start_install.png)
- 选择刚下载的debian镜像。 ![kvm_select_source](/images/安装debian/kvm_select_source.png) ![kvm_select_iso](/images/安装debian/kvm_select_iso.png)
- virt-manager会自动探测系统镜像类型，也可自行选择。 ![kvm_install_iso](/images/安装debian/kvm_install_iso.png)
- 选择配置CPU和内存。 ![kvm_conf_cpu](/images/安装debian/kvm_conf_cpu.png)
- 创建虚拟磁盘。 ![kvm_create_qcow2](/images/安装debian/kvm_create_qcow2.png) ![kvm_create_qcow2_01](/images/安装debian/kvm_create_qcow2_01.png) ![kvm_create_qcow2_02](/images/安装debian/kvm_create_qcow2_02.png) ![kvm_create_qcow2_03](/images/安装debian/kvm_create_qcow2_03.png) ![kvm_create_qcow2_04](/images/安装debian/kvm_create_qcow2_04.png)
- 最终确认配置后完成创建，启动虚拟机进入debian引导安装流程。 ![kvm_final_confirm](/images/安装debian/kvm_final_confirm.png)

## 开始安装debian

- 安装过程视频 <iframe src="//player.bilibili.com/player.html?aid=511542425&bvid=BV1uu41167Yp&cid=720945908&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="672" height="378"> </iframe>
