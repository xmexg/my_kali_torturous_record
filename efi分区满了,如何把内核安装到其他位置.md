# efi分区满了,如何把内核安装到其他位置

执行`sudo apt dist-upgrade`报错:  
```sh
正在设置 linux-image-6.12.20-amd64 (6.12.20-1kali1) ...
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-6.12.20-amd64
Updating kernel version 6.12.20-amd64 in systemd-boot...
install: 写入 '/boot/efi/05b898c6b2a942d0b70bcac6b2c699d0/6.12.20-amd64/initrd.i
mg-6.12.20-amd64' 时出错: 设备上没有空间
Error: could not copy '/boot/initrd.img-6.12.20-amd64' to '/boot/efi/05b898c6b2a
942d0b70bcac6b2c699d0/6.12.20-amd64/initrd.img-6.12.20-amd64'.
/usr/lib/kernel/install.d/90-loaderentry.install failed with exit status 1.
run-parts: /etc/initramfs/post-update.d//systemd-boot exited with return code 1
run-parts: /etc/kernel/postinst.d/initramfs-tools exited with return code 1
dpkg: 处理软件包 linux-image-6.12.20-amd64 (--configure)时出错：
 已安装 linux-image-6.12.20-amd64 软件包 post-installation 脚本 子进程返回错误状
态 1
dpkg: 依赖关系问题使得 linux-image-amd64 的配置工作不能继续：
 linux-image-amd64 依赖于 linux-image-6.12.20-amd64 (= 6.12.20-1kali1)；然而：
  软件包 linux-image-6.12.20-amd64 尚未配置。

dpkg: 处理软件包 linux-image-amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
dpkg: 依赖关系问题使得 linux-headers-6.12.20-amd64 的配置工作不能继续：
 linux-headers-6.12.20-amd64 依赖于 linux-image-6.12.20-amd64 (= 6.12.20-1kali1)
 | linux-image-6.12.20-amd64-unsigned (= 6.12.20-1kali1)；然而：
  软件包 linux-image-6.12.20-amd64 尚未配置。
  未安装软件包 linux-image-6.12.20-amd64-unsigned。

dpkg: 处理软件包 linux-headers-6.12.20-amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
dpkg: 依赖关系问题使得 linux-headers-amd64 的配置工作不能继续：
 linux-headers-amd64 依赖于 linux-headers-6.12.20-amd64 (= 6.12.20-1kali1)；然而
：
  软件包 linux-headers-6.12.20-amd64 尚未配置。

dpkg: 处理软件包 linux-headers-amd64 (--configure)时出错：
 依赖关系问题 - 仍未被配置
在处理时有错误发生：
 linux-image-6.12.20-amd64
 linux-image-amd64
 linux-headers-6.12.20-amd64
 linux-headers-amd64
错误： Sub-process /usr/bin/dpkg returned an error code (1)
```

## 如何把新内核安装到/boot/newefi文件夹里?
1. 创建`/boot/newefi`文件夹:  
```
sudo mkdir /boot/newefi
```
2. 下面的chatgpt的回复,管用:
```
检查和修改 /etc/kernel/postinst.d/ 脚本
你可能需要修改 /etc/kernel/postinst.d/ 中的脚本，确保在执行内核安装时强制使用 /boot/newefi。尝试修改以下几个步骤：

修改 /etc/kernel/postinst.d/initramfs-tools

首先，打开文件 /etc/kernel/postinst.d/initramfs-tools：

sudo nano /etc/kernel/postinst.d/initramfs-tools
确保在文件顶部添加一行，设置 BOOT_ROOT 环境变量：

export BOOT_ROOT=/boot/newefi
这样，kernel-install 命令会使用你定义的新路径 /boot/newefi。

确保 /etc/kernel/postinst.d 中的其他脚本也被更新

查看所有 postinst.d 目录下的文件，检查是否有其他脚本没有设置 BOOT_ROOT。例如，文件 /etc/kernel/postinst.d/zz-update-systemd-boot 也可能需要类似的修改。

sudo nano /etc/kernel/postinst.d/zz-update-systemd-boot
在脚本的顶部添加：

export BOOT_ROOT=/boot/newefi
然后保存退出。
```