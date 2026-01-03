# Cudy TR3000 硬件扩容Flash方法

> ⚠️Warning  
> 由于没有新的设备，仅在TR3000 128MB SN2544以前的版本测试，新版本硬改方法可用性未知，欢迎测试反馈

前提条件/工具：
- 热风枪、电烙铁
- CH341或CH347编程器
- 256MB(W25N02KVZE) 或 512MB(W25N04KVZE) NAND Flash
- 较好的动手能力

> 🔔Info   
> 若设备刷过第三方uboot，直接进uboot刷入本仓库提供的uboot即可，刷入后跳至Step2；若设备是原厂系统，请从Step1开始。

---

## Step 1 刷入uboot
原教程文件下载及参考：[Bilibili@你逗你玩](https://www.bilibili.com/video/BV1HkqoYwEkb/)、[Bilibili@-SAS-only](https://www.bilibili.com/opus/1090481446156501027)

1. 刷入过渡系统  
首先进入原厂系统，更新固件，刷入过渡固件`cudy_tr3000-v1-sysupgrade.bin`
2. 刷入uboot解锁固件  
进入过渡系统，刷入uboot解锁固件`openwrt-mediatek-filogic-cudy_tr3000-v1-squashfs-sysupgrade.bin`（密码为password）
3. 在openwrt中刷入新的uboot  
以下命令基于本仓库提供的uboot（当然原教程的uboot也能用，自行选择）  
    - 打开系统->文件传输，上传新的uboot：`mt7981_cudy_tr3000-v1-fip-fixed-parts-multi-layout-512mb.bin`（在本仓库的Release中下载）  
    - 打开系统->TTYD终端，执行命令
`mtd write /tmp/upload/mt7981_cudy_tr3000-v1-fip-fixed-parts-multi-layout-512mb.bin FIP` 刷入支持512MB的uboot
    - 然后执行`reboot`重启


## Step 2 确认uboot功能正常
1. 进入uboot  
按住reset按钮插电，指示灯先白色闪烁，稍后红灯常亮，说明已经进uboot
2. 确认uboot正确刷入  
确保uboot中能看到490M的布局，如下图
![images/uboot.jpg](images/uboot.jpg)


## Step 3 备份原机Flash
1. 拆下原机flash
2. 使用编程器备份原机flash


## Step 4 烧录新的Flash
1. 将备份的文件烧录进新买的flash中
2. 将Flash焊回路由器

## Step 5 测试&刷机
1. 进入uboot
2. 在uboot中刷入系统
选择mtd布局`mod-490`，刷入512MB版本的系统固件。（可从本仓库Release中下载）


---

## 关于硬改256MB Flash的说明
由于 512MB(W25N04KVZE) 比 256MB(W25N02KVZE) 价格相差很大（约￥20 VS 约￥5），如果不需要太多空间，似乎硬改为256MB的性价比更高

经测试，上述教程对于256MB Flash也适用，固件和uboot使用本仓库**512MB版本**即可，刷入后可以正常识别Flash空间，系统中显示可用空间对比如下

|Flash|可用空间|
|---|---|
|512MB|约415MB|
|256MB|约198MB|
