# 我想给kali引导菜单添加关机选项
编辑`/etc/grub.d/40_custom`文件,在结尾添加如下内容:
```shell
sudo mousepad /etc/grub.d/40_custom
```
```
menuentry "关机" --class halt {
    echo "正在关机..."
    halt
}

menuentry "重启" --class reboot {
    echo "正在重启..."
    reboot
}
```
更新grub配置
```
sudo update-grub
```