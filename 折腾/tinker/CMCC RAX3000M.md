此文为我折腾硬路由CMCC RAX3000M的相关流程和注意相关事项
# 简要介绍
CMCC RAX3000M分为普通版和算力版区别是普通版为128M闪存(NAND)，算力版则具有64G emmc空间
本篇的流程仅适用于CMCC RAX3000M NAND型号，请注意辨别
这里仅提供作者觉得可以帮到的资料，主要细节流程也有很多帖子了，这里的流程并不全。如果你完全是小白，并且未看过其余帖子，可能这个帖子对你并无帮助，还有可能对你造成混乱。请你**酌情阅读本文章**
## 相关资源

### 固件
ImmortalWrt官方固件[下载](https://firmware-selector.immortalwrt.org/?version=24.10.2&target=mediatek%2Ffilogic&id=cmcc_rax3000m)
hanwckf的二改ImmortalWrt，基于版本immortalwrt 21.02 [hanwckf/immortalwrt-mt798x](https://github.com/hanwckf/immortalwrt-mt798x)
> 两个固件最主要的区别是hanwckf对798x系列的进行了大量优化，如果只是当硬路由来使用**个人**更推荐hanwckf的二改ImmortalWrt

### uboot
ImmortalWrt官方固件[下载](https://firmware-selector.immortalwrt.org/?version=24.10.2&target=mediatek%2Ffilogic&id=cmcc_rax3000m)
还是hanwckf出品的uboot[hanwckf/bl-mt798x](https://github.com/hanwckf/bl-mt798x/)
> 如果想体验最新的ImmortalWrt请使用ImmortalWrt的官方uboot
> 
> hanwckf出品的uboot又称为不死uboot，在社区里面响应很高，但是不支持官方的itb格式的固件，推荐使用二改hanwckf的ImmortalWrt和官方ImmortalWrt23.05.4及以下的使用

### 刷写工具
需要使用到的工具
- ssh客户端(感觉不用举例子了)
- TFTP的服务端(tftpd等)
## 刷机流程

### 刷入ImmortalWrt官方固件:

解锁ssh -> 通过ssh连接到路由器 -> 刷入uboot -> 刷入cmcc_rax3000m-initramfs-recovery.itb(集成最小文件系统的 Linux 内核。适用于首次安装或故障恢复) -> 进入系统-备份与升级-刷写新的固件 选择官方的cmcc_rax3000m-squashfs-sysupgrade.itb

> 这里着重讲一下关于BL2分区，在我的这台CMCC RAX3000M 流程便是刷入 uboot 刷入BL2 分区，之后再次刷入recovery固件，但是在中国移动的官方固件底下BL2分区是被锁定的。所以没有进行刷写，直接使用tftpd工具进行刷写recovery固件，相当于直接跳过了BL2分区的刷写，使用并未发现任何错误。
> 
> 之后成功刷写ImmortalWrt之后可以使用内核模块kmod-mtd-rw进行解锁之后刷写BL2分区(~~困扰了老半天~~)(挠头)
>
> **官方固件都使用支持TFTP协议的刷写工具进行刷入**例如tftpd等
