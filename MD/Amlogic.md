- ## CPU型号跟机型的对照表
- [点击查看CPU型号跟机型的对照表](https://github.com/ophub/amlogic-s9xxx-openwrt/blob/main/README.cn.md#openwrt-%E5%9B%BA%E4%BB%B6%E8%AF%B4%E6%98%8E)


#
- 可以指定打包所用型号有: s922x 、 s922x-n2 、 s922x-reva 、 a311d 、 s905x3 、 s905x2 、 s905x2-km3 、 s905l3a 、 s912 、 s912-m8s 、 s905d 、 s905d-ki 、 s905x 、 s905w 、 s905

- 机型说明：`s922x-reva` 是 `s922x-gtking-pro-rev_a`，`s922x-n2` 是 `s922x-odroid-n2` ，`s912-m8s` 是 `s912-mecool-m8s-pro-l` ，`s905d-ki` 是 `s912-mecool-ki-pro`，`s905x2-km3` 是 `s905x2-mecool-km3` 

- [内核版本时时变动的，所以选择内核之前，一定要点击这里查看一下当前可用内核，随便搞的话，没有该内核，打包就失败](https://github.com/ophub/kernel/tree/main/pub/stable)
#
```sh
机型和核心组合设置在：build/openwrt_amlogic/diy-part.sh


amlogic_model为机型设置，多机型需要中间加‘_’间隔，比如 s922x_s912_s922x-n2_s922x-reva_s905x2-km3
amlogic_kernel为内核设置，多内核需要中间加‘_’间隔，比如 5.10.100_5.4.180
设定内核的时候加上 -a true 的话，则默认自动检测使用同类型的最高版本，比如单内核 5.4.180 -a true 那就是使用5.4.xxx 最高版本
比如双内核形式 5.10.100_5.4.180 -a true 的话，那就是使用 5.10.xxx 和 5.4.xxx 最高版本
rootfs_size为rootfs分区大小，不能小于500，不懂就默认不要修改。
组合方法同样适用于本地打包的


比如这样的，就是单机型+单核心组合打包
cat >$GITHUB_WORKSPACE/amlogic_openwrt <<-EOF
amlogic_model=s905d
amlogic_kernel=5.10.70
rootfs_size=960
EOF

比如这样的，就是单机型+单核心组合，自动检测使用同类型内核最高版本打包（您不用管他是5.10.70还是5.10.60）
cat >$GITHUB_WORKSPACE/amlogic_openwrt <<-EOF
amlogic_model=s905d
amlogic_kernel=5.10.70 -a true
rootfs_size=960
EOF

比如这样的，就是双机型+双核心组合打包
cat >$GITHUB_WORKSPACE/amlogic_openwrt <<-EOF
amlogic_model=s905x3_s905x2
amlogic_kernel=5.10.100_5.4.170
rootfs_size=960
EOF

比如这样的，就是双机型+双核心组合，自动检测使用同类型内核最高版本打包（您不用管他是5.10.100_5.4.180还是5.10.90_5.4.170）
cat >$GITHUB_WORKSPACE/amlogic_openwrt <<-EOF
amlogic_model=s905x3_s905x2
amlogic_kernel=5.10.100_5.4.180 -a true
rootfs_size=960
EOF


请注意，不是组合越多就越好的，每个内核就打包一个固件而已，不是一个固件里面可以塞进去几个内核的，
要根据自己个人需求组合，要不然打包的固件太多，服务器空间不够用就打包失败了。
```
#
#

## 安装及升级 OpenWrt 的相关说明

- 选择和你的电视盒子型号对应的 OpenWrt 固件，使用 [Rufus](https://rufus.ie/) 或者 [balenaEtcher](https://www.balena.io/etcher/) 等工具将固件写入USB里，然后把写好固件的USB插入电视盒子。

- 提示：安装/升级等功能由 [luci-app-amlogic](https://user-images.githubusercontent.com/68696949/145738345-31dd85cf-5e43-444e-a624-f21a28be2a7c.gif) 提供可视化操作支持。

---
### 安装 OpenWrt

- 从浏览器访问 OpenWrt 的默认 IP: 192.168.1.1 → `使用默认账户登录进入 OpenWrt` → `系统菜单` → `晶晨宝盒` → `安装 OpenWrt` ，在支持的设备下拉列表中选择你的盒子，点击 `安装 OpenWrt` 按钮进行安装。

---
### 升级 OpenWrt

- 从浏览器访问 OpenWrt 的 IP 如: 192.168.1.1 →  `使用账户登录进入 OpenWrt` → `系统菜单` → `晶晨宝盒` → `手动上传更新 / 在线下载更新`

- 如果选择 `手动上传更新` OpenWrt固件，可以将[编译](https://github.com/281677160/build-actions)好 OpenWrt 固件压缩包，如 openwrt_s9xxx_k5.15.25_xxx.img.gz 进行上传（推荐上传压缩包，系统会自动解压。如果上传解压缩后的 xxx.img 格式的文件，可能会因为文件太大而上传失败），上传完成后界面将显示 `更新固件` 的操作按钮，点击即可更新。

- 如果选择 `手动上传更新` [OpenWrt 内核](https://github.com/ophub/kernel/tree/main/pub/stable)，可以将 `boot-xxx.tar.gz`, `dtb-amlogic-xxx.tar.gz`, `modules-xxx.tar.gz` 这 3 个内核文件上传（其他内核文件不需要，如果同时上传也不影响更新，系统可以准确识别需要的内核文件），上传完成后界面将显示 `更新内核` 的操作按钮，点击即可更新。

- 如果选择 `在线下载更新` OpenWrt 固件或内核，将根据`插件设置`中的`固件下载地址`和`内核下载地址`进行下载，你可以自定义修改下载来源，具体操作方法详见 [luci-app-amlogic](https://github.com/ophub/luci-app-amlogic) 的编译与使用说明。

---
### 在 TF/SD/USB 中使用 OpenWrt

- 从浏览器访问 OpenWrt 的默认 IP: 192.168.1.1 → `使用默认账户登录进入 OpenWrt` → `系统菜单` → `TTYD 终端` → 输入命令

```yaml
openwrt-tf
```

- 激活剩余空间后，支持在 TF/SD/USB 中升级内核和 OpenWrt 系统。

---
### 为 OpenWrt 创建 swap

- 如果你在使用 `docker` 等内存占用较大的应用时，觉得当前盒子的内存不够使用，可以创建 `swap` 虚拟内存分区，将 `/mnt/*4` 磁盘空间的一定容量虚拟成内存来使用。下面命令输入参数的单位是 `GB`，默认为 `1`。

- 从浏览器访问 OpenWrt 的默认 IP: 192.168.1.1 → `使用默认账户登录进入 OpenWrt` → `系统菜单` → `TTYD 终端` → 输入命令

```yaml
openwrt-swap 1
```

---
### 备份/还原 EMMC 原系统

- 支持在 `TF/SD/USB` 中对盒子的 `EMMC` 分区进行备份/恢复。建议您在全新的盒子里安装 OpenWrt 系统前，先对当前盒子自带的安卓 TV 系统进行备份，以便日后在恢复电视系统等情况下使用。

- 请从 `TF/SD/USB` 启动 OpenWrt 系统，浏览器访问 OpenWrt 的默认 IP: 192.168.1.1 → `使用默认账户登录进入 OpenWrt` → `系统菜单` → `TTYD 终端` → 输入命令

```yaml
openwrt-ddbr
```

- 根据提示输入 `b` 进行系统备份，输入 `r` 进行系统恢复。

💡提示：须使用 `/mnt/*4/` 空间进行存放 `BACKUP-arm-64-emmc.img.gz` 备份文件，未创建 `TF/SD/USB` 扩展分区的用户，须先使用 `openwrt-tf` 命令创建扩展分区。

---
### 控制 LED 显示

- 从浏览器访问 OpenWrt 的默认 IP: 192.168.1.1 → `使用默认账户登录进入 OpenWrt` → `系统菜单` → `TTYD 终端` → 输入命令

```yaml
openwrt-led
```

- 根据 [LED 屏显示控制说明](https://github.com/ophub/amlogic-s9xxx-armbian/blob/main/build-armbian/armbian-docs/led_screen_display_control.md) 进行调试。

---
- ## 晶晨宝盒 使用截图

![luci-app-amlogic](https://user-images.githubusercontent.com/68696949/145738345-31dd85cf-5e43-444e-a624-f21a28be2a7c.gif)

---
- # 以上相关内容全部获取与[ophub/amlogic-s9xxx-openwrt](https://github.com/ophub/amlogic-s9xxx-openwrt)，需要更新更详细的内容可移步至此大佬仓库查看，感谢大佬的付出！
