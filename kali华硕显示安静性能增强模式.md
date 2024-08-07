# kali华硕显示安静性能增强模式

首先, kali无法捕获到`fn`+`f5`功能键, 所以打开`设置` -> `键盘` -> `应用程序快捷键`是没用的

## 在哪里看到华硕电脑当前运行模式?
使用`cat /sys/devices/platform/asus-nb-wmi/throttle_thermal_policy`  
`0,1,2`分别对应`安静,性能,增强`

## 通过`一般监视器(xfce4-genmon-plugin)`轮询获取当前模式
1. 下载`xfce4-genmon-plugin`
```shell
sudo apt install xfce4-genmon-plugin
```
2. 在任意位置创建脚本
```shell
mkdir -p ~/myscript && touch ~/myscript/sysmode.sh && chmod +x  ~/myscript/sysmode.sh
```
3. 编辑脚本  
打开文件
```shell
mousepad ~/myscript/sysmode
```  
4. 以下有`bash`和`zsh`两种写法,任选一种  
```shell
#!/usr/bin/bash
a=("安静" "性能" "增强") && echo ${a[$(cat /sys/devices/platform/asus-nb-wmi/throttle_thermal_policy)]}
```
```shell
#!/usr/bin/zsh
a=(安静 性能 增强) && echo ${a[$(expr $(cat /sys/devices/platform/asus-nb-wmi/throttle_thermal_policy) + 1)]}
```
5. 右击任务栏 -> `面板` -> `添加新项目` -> `一般监视器` -> `添加`
6. 右击刚添加的一般监视器 -> `属性` -> `命令`选择刚保存的脚本文件 -> `保存`