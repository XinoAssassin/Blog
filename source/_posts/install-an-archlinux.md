---
title: Arch Linux 小折腾记
tags:
  - 杂
  - Programming
  - DIY
date: 2018-02-13 15:46:21
---


久闻 Arch Linux (下文简称 Arch )大名，在虚拟机上多次完成过了整个系统的安装与图形界面的配置，也试过了 KDE 和 GNOME 这两个大名鼎鼎的图形环境，深切体会到了 Arch Linux 的四大原则：

- Simplicity
- Modernity
- Pragmatism
- User Centrality

<del>Pacman 是最好的包管理器</del>

---

我分别在自己的台机上和一台 ThinkPad S1 Yoga 2014 上安装 Arch. 具体过程跟着官方 [Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide) 走就行了，Wiki 中的注意事项也是比较全面了，本文写几个小注意点。

## Btrfs

不知道 Btrfs 的我建议你马上查 Wikipedia.

Arch 的安装 ISO 环境中是自带了 Btrfs 的工具包和支持的，但是在 base 软件包中不包含这些，所以你需要手动安装 `btrfs-progs` 这个包，然后在 /etc/mkinitcpio.conf (注意是在 `arch-chroot` 这一步之后)中的 HOOK 一行中添加 btrfs, 之后跑一遍 `mkinitcpio -p linux`, 看到输出中有 btrfs 即可。

关于 btrfs 的配置问题，参见官方 Wiki, 也可以参考 [egara/arch-btrfs-installation](https://github.com/egara/arch-btrfs-installation).

## Boot Loader

在 Boot Loader 的选择中，我个人还是比较倾向于直接使用 systemd 集成的 systemd-boot, 懒得再装个 GRUB.

虽然 systemd-boot 局限性非常大，比如它只能加载 EFI 可执行文件比如 bootmgfw.efi, vmlinuz-linux.

如果选择了 systemd-boot 作为 Boot Loader, 那么就需要在 `arch-chroot` 后手动运行一次 `bootctl --path=$ESP install` 来把引导程序安装进 EFI 分区中并向 UEFI 界面注册这个引导器，之后你在启动时按 F12 就可以看到一个 Linux Boot Loader.

但是如果只是将 Boot Loader 安装到了 EFI 分区还不足以让它引导我们进 Arch, 仍然需要在 /boot/loader/entries/ 这个目录下建立新的 conf 文件，里面的内容如下

``` 
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=(PARTUUID|PARTLABEL)=xxxxxxxxxxxxxxxxxxxxxxxx rw
```

如果使用的是 Intel 的 CPU 并且有安装 `intel-ucode` 包，那么在第3行之前再加一行 `initrd /intel-ucode.img` 启用它。

建立完了引导所需的系统 config, 需要在 /boot/loader/loader.conf 中的 default 一行中指定刚才建立的 config 文件名，然后使用 Linux Boot Loader 引导器就会默认进入 Arch Linux.

## 双系统与时钟

感谢 UEFI 标准，现在我们可以非常方便地组建双系统 (Windows + Linux) 环境。

如果使用的 Boot Loader 是 systemd-boot, 那么它会自动检测 Windows 的存在，并且在选择菜单自动提供一个 Windows 的选项，这个选项的配置名为 `auto-windows`, 选择它会运行 /boot/Microsoft/Boot/bootmgfw.efi 然后就是 Windows 的流程了。

双系统有一个很大的问题，就是时钟。Windows 默认往 BIOS 写入本地时间，而不是 Linux 标准的 UTC, 这就会导致你从 Windows 切换过去后时间会+8, 也就是按你的时区调整然后又写回到 BIOS 中。从 Linux 切换过去，Windows 认为是本地时间，于是显示给你的实际上是 UTC. 所以为了解决这个问题，在安装 Arch 时，使用 `hwclock --hctosys -l` (将硬件时间作为本地时间读入系统), 而不是 Guide 上所用的 `hwclock --systohc`(将系统时间写入 BIOS 并且是 UTC).

之后启动到 Windows, 注册表定位到 `HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation` 添加一个 DWORD, 名为 `RealTimeIsUniversal` 值为 1, 即可实现 Windows 往硬件中写入的时间是以 UTC 为标准而不是本地时间。

## systemctl enable

由于 Arch Linux 的哲学，大部分软件包在安装之后也并不会自动启用，需要你手动 `systemctl enable|start xxxx`. 尤其要留意的是，还在 ISO 环境下安装系统的时候就要把 dhcpcd 这个服务给 enable 了，解决你进系统之后发现连不上网络想半天不知道问题出在哪里的问题。当然这是针对有线网卡，而大部分无线网卡，我推荐直接使用 Network Manager 搞定。当然使用无线网卡的首次联网你还是需要如下步骤：

```shell
ip link set dev interface up
iw interface scan
wpa_supplicant -B -i interface -c <(wpa_passphrase SSID Password)
```

还有一点需要注意的是，Arch 的 Shadowsocks-libev 默认读取的配置目录在 /etc/shadowsocks/, 与 Python 版相同的目录而不是传统的 /etc/shadowsocks-libev/ 下，启用它的命令为 `systemctl enable shadowsocks-libev@配置文件名.service` .

## Intel 有线网卡的坑

当你机器上使用的网卡的是 Intel Ethernet Connection I21x 的时候，你要小心了，在安装 Arch 之前，在 Windows 中安装 Intel 的官方驱动，然后在网卡的硬件配置面板中将所有的网络唤醒选项给关闭，这可以有效防止你进入 Arch 环境之后获取不到任何的网络信息导致连不上网。

`>> endl;`