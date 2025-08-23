# CMCC RAX3000M 折腾记录

此文为我折腾硬路由CMCC RAX3000M的相关流程和注意相关事项

## 简要介绍
CMCC RAX3000M分为普通版和算力版区别是普通版为128M闪存 (NAND)，算力版则具有64G eMMC空间
本篇的流程仅适用于CMCC RAX3000M NAND型号。
这里仅提供作者觉得可以帮到的资料，主要细节流程也有很多帖子了，这里的流程并不全。如果你完全是小白，并且未看过其余帖子，可能这个帖子对你并无帮助，还有可能对你造成混乱。请你**酌情阅读本文章**

## 相关资源

### 固件
ImmortalWrt 官方固件 [下载](https://firmware-selector.immortalwrt.org/?version=24.10.2&target=mediatek%2Ffilogic&id=cmcc_rax3000m)  
hanwckf 的二改 ImmortalWrt，基于版本 ImmortalWrt 21.02 [hanwckf/immortalwrt-mt798x](https://github.com/hanwckf/immortalwrt-mt798x)  
> 两个固件最主要的区别是 hanwckf 对 798x 系列的进行了大量优化，如果只是当硬路由来使用**个人**更推荐 hanwckf 的二改 ImmortalWrt

### uboot
ImmortalWrt 官方固件页[下载](https://firmware-selector.immortalwrt.org/?version=24.10.2&target=mediatek%2Ffilogic&id=cmcc_rax3000m)  
hanwckf 出品的 uboot [hanwckf/bl-mt798x](https://github.com/hanwckf/bl-mt798x/)  
> 如果想体验最新的 ImmortalWrt 请使用 ImmortalWrt 的官方 uboot  
> hanwckf 出品的 uboot 又称为不死 uboot，在社区里面响应很高，但是不支持官方的 itb 格式的固件，推荐使用二改 hanwckf 的 ImmortalWrt 和官方 ImmortalWrt 23.05.4 及以下的使用

### 刷写工具
需要使用到的工具
- ssh 客户端
- 支持 TFTP 的服务端 (如 tftpd 等)

## 刷机流程

### 刷入 ImmortalWrt 官方固件
1. 解锁 ssh  
2. 通过 ssh 连接到路由器  
3. 刷入 uboot  
4. 刷入 `cmcc_rax3000m-initramfs-recovery.itb` (集成最小文件系统的 Linux 内核，适用于首次安装或故障恢复)  
5. 浏览器打开 `http://192.168.1.1`  
6. 系统 - 备份与升级 - 刷写新的固件，选择官方 `cmcc_rax3000m-squashfs-sysupgrade.itb`  
7. 完成 (?)  

关于 `BL2` 分区：在我的这台 CMCC RAX3000M 流程便是刷入 uboot 后刷入 `BL2` 分区，之后再次刷入 recovery 固件，但是在中国移动的官方固件底下 `BL2` 分区是被锁定的。所以没有进行刷写，直接使用 TFTP 工具进行刷写 recovery 固件，相当于直接跳过了 `BL2` 分区的刷写，使用并未发现任何错误。  

> 之后成功刷写 ImmortalWrt 之后可以使用内核模块 `kmod-mtd-rw` 进行解锁之后刷写 `BL2` 分区 (~~困扰了老半天~~)(挠头)  
> **官方固件都使用支持 TFTP 协议的刷写工具进行刷入** (例如 tftpd 等)  
> 跳过 `BL2` 写入有潜在风险，务必确认自己设备与版本适配后再决定。

### hanwckf 的二改 ImmortalWrt
1. 解锁 ssh  
2. 刷入 uboot  
3. 本机网卡固定 IP 为 `192.168.1.254`，网关设为 `192.168.1.1`  
4. 浏览器进入 `http://192.168.1.1`  
5. 选择后缀名为 `bin` 类型或者其余兼容的固件进行刷写  
6. 等待上传完成和校验完成，Web 上会有等待重启的界面  
7. 完成