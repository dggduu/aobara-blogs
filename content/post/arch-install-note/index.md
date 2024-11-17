+++
author = "aobara"
title = "我的 Arch linux 手动安装笔记"
description = ""
date = "2024-10-10"
categories = [
    "电脑",
    "笔记",
]
image = "1.png"
+++
## 起因
平常我都是直接用 ArchInstall 安装 arch ，但每每重装 arch 总有一些指令记不住（比如说怎么联网），于是干脆写篇笔记做个记录。所以说为什么不用 WSL Arch呢(苦笑)
## 正题
本文基本参考于：https://wiki.archlinux.org/title/Installation_guide  
### 下载镜像
 [arch ISO 清华源](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/)  
 找个虚拟机
 ### 确定以UEFI启动
 ```shell
  cat /sys/firmware/efi/fw_platform_size    
  ```
 >如果命令返回 64，则系统以 UEFI 模式启动，并具有 64 位 x64 UEFI。如果命令返回 32，则系统以 UEFI 模式启动，并具有 32 位 IA32 UEFI;虽然支持此功能，但它会将引导加载程序的选择限制为 systemd-boot 和 GRUB。如果该文件不存在，系统可能会以 BIOS（或 CSM）模式启动。如果系统没有以您想要的模式（UEFI 与 BIOS）启动，请参阅您的主板手册。  
 >——Arch Wiki
 >
 若不是UEFI模式：  
 vbox-设置-系统-启用EFI
 ### 联网
 ```shell
 pacman -S iwd      //如果下面的iwctl没找到程序则运行这个

 iwctl
 device list
 station wlan0 get-networks
 station wlan0 connect xxx
 exit

 ping  baidu.com
 ```
 ps.等等如果我是在实体机装这个，那么怎么进行 captive potral 来连上我们学校校园网呢（逃

 #### 解决wlan0 powered:off问题
 ```sh
rfkill unblock wlan
 ```

 ### ssh（可选）
 我用的是VBox,先在设置里进行端口映射
 ```shell
 passwd root    \\别忘记了
 sudo nano /etc/ssh/sshd_config
 把PORT 22那行的注释去掉
宿主机如果发生密钥冲突的话：
 ssh-keygen -R "[127.0.0.1]:PORT"
 ```
宿主机：  
```shell
ssh -l root -p PORT 127.0.0.1
```
 ### 禁用 reflector 服务!!!
 ```shell
 systemctl stop reflector.service

 systemctl status reflector.service
 ```
 ### 换源（清华源）
 ```shell
 sudo nano /etc/pacman.d/mirrorlist
 ```
 ```shell-script
 Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
 ```
 ### 更新系统时钟
 ```shell
timedatectl set-ntp true 

timedatectl status 
```
 ### 分区和格式化（使用 exf4 文件系统）
 #### 分盘
 使用`cfdisk`  
 |分区大小         |备注           |
|----------|--------|
| 100MB          | EFI 分区       |
| 600MB          | Linux fileSystem |
| 所有剩余空间   | Linux fileSystem |  
注：我这是虚拟机且电脑没什么空间了，所以没有配置SWAP分区，实体机的话建议配置一下  
##### 在固态硬盘使用cfdisk
![Alt text](/2.png "Optional title")
```sh
cfdisk /dev/nvme0n1
```
#### 格式化并挂载
```shell
mkfs.fat -F32 /dev/sda1  
mkfs.ext4 /dev/sda2  
mkfs.ext4 /dev/sda3  

mount --mkdir /dev/sda3 /mnt      //按顺序来
mount --mkdir /dev/sda2 /mnt/boot
mount --mkdir /dev/sda1 /mnt/efi
```
### 配置系统
#### 启用pacman多线程下载（可选但推荐）
```shell-script
nano /etc/pacman.conf      //改线程为16，记得取消注释
```
#### 安装基本软件包
```shell-script
pacman -Sy archlinux-keyring
pacstrap -K /mnt base linux-zen linux-firmware vim nano alsa-utils networkmanager base-devel        //这里使用了linux-zen内核，你可以使用linux内核
```
这是Arch Wiki上使用的：
>pacstrap -K /mnt base linux linux-firmware

可选项：
PipeWire：pipewire-audio，pipewire-alsa，pipewire-pulse，pipewire-jack，wireplumber  
网络管理：networkmanager  
蓝牙：bluez，bluez-utils  
声卡管理：alsa-utils  
有关帮助文档: man-db，man-pages，texinfo  
SSH ：openssh  
微码修正文件 (虚拟机不需要安装)：intel-ucode，amd-ucode  
#### 进入系统
```shell
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

nano /etc/locale.gen
locale-gen

pacman -S sudo     //如果下面无法正常执行
echo "%wheel ALL=(ALL:ALL) ALL" > /etc/sudoers
```
#### 添加用户名(别忘了)
```shell
useradd -m -G wheel userName
passwd userName
```
#### GRUB 安装
```shell-script
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/efi
grub-mkconfig -o /boot/grub/grub.cfg
```
### 安装KDE
```shell-script
pacman -S plasma-meta kde-applications xorg noto-fonts noto-fonts-cjk noto-fonts-emoji gtkmm3

systemctl enable sddm
exit
reboot
```

### 附录：Btrfs文件格式[^1]
```shell-script
mkfs.fat -F32 /dev/sda1
mkfs.btrfs -L arch-btrfs /dev/sda2
mount -t btrfs -o compress=zstd /dev/sda2 /mnt

btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume list -p /mnt    #复查子卷情况
umount /mnt

mount -t btrfs -o subvol=/@,compress=zstd /dev/sda2 /mnt
mkdir /mnt/home
mount -t btrfs -o subvol=/@home,compress=zstd /dev/sda2 /mnt/home 
mkdir -p /mnt/boot 
mount /dev/sda1 /mnt/boot
df -h

pacman -Sy archlinux-keyring
pacstrap /mnt base base-devel linux linux-firmware btrfs-progs vim nano sudo networkmanager

genfstab -U /mnt > /mnt/etc/fstab
arch-chroot /mnt
...(看上面的)
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH
```
```shell-script
The algorithms can be characterized as follows regarding the speed/ratio trade-offs:
关于速度/比率权衡，算法可以描述如下：

ZLIB 兹利布
slower, higher compression ratio
速度较慢，压缩率较高

levels: 1 to 9, mapped directly, default level is 3
级别：1 到 9，直接映射，默认级别为 3

good backward compatibility
良好的向后兼容性

LZO LZO 公司
faster compression and decompression than ZLIB, worse compression ratio, designed to be fast
比 ZLIB 更快的压缩和解压缩，压缩率更差，设计为快速

no levels 无级别

good backward compatibility
良好的向后兼容性

ZSTD
compression comparable to ZLIB with higher compression/decompression speeds and different ratio
压缩率可与 ZLIB 相媲美，具有更高的压缩/解压缩速度和不同的比率

levels: 1 to 15, mapped directly (higher levels are not available)
级别：1 到 15，直接映射（更高级别不可用）

since 4.14, levels since 5.1
自 4.14 起，级别自 5.1 起
```
### 使用NetworkManager
```sh
systemctl enable NetworkManager
systemctl start NetworkManager

nmcli connection add type wifi ifname wlan0 con-name XiaomiSu7 ssid "XiaomiSu7" 802-11-wireless-security.key-mgmt wpa-psk 802-11-wireless-security.psk "your_wifi_password"
```

### 安装yay
```sh
sudo pacman -Syu
sudo pacman -S --needed base-devel git

git clone https://aur.archlinux.org/yay.git
cd yaycd yay
```

### 修改DNS
```sh
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved

sudo nano /etc/systemd/resolved.conf

sudo systemctl restart systemd-resolved
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

### 配置临时代理
```sh
export http_proxy=http://192.168.141.106:7897
export https_proxy=http://192.168.141.106:7897
```
### 安装hyprland
```sh
pacman -Sy hyprland
```
## 可能有用的东西：
sudo systemctl start sshd  
sudo systemctl enable sshd  
sudo nano /etc/resolv.conf  
sudo systemctl enable NetworkManager.service  

ctrl+alt+F5  
[^1]:参阅：
https://arch.icekylin.online/guide/rookie/basic-install#_7-%E5%88%86%E5%8C%BA%E5%92%8C%E6%A0%BC%E5%BC%8F%E5%8C%96-%E4%BD%BF%E7%94%A8-btrfs-%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F