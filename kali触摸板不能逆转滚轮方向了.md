# kali触摸板不能逆转滚轮方向了
安装了`xserver-xorg-input-synaptics`导致的, 卸载即可
```shell
sudo apt remove --purge xserver-xorg-input-synaptics
```
触摸板用`libinput`
```shell
sudo apt install libinput
```
搞定  



**另外, 即便安装了`xserver-xorg-input-synaptics`,还是有办法通过命令行临时设置自然滚动的**
1. 安装xinput
```shell
sudo apt install xinput
```
2. 列出所有鼠标和键盘设备
```shell
xinput
```
```
❯ xinput
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ SIGMACHIP Usb Mouse                     	id=10	[slave  pointer  (2)]
⎜   ↳ ASUP1205:00 093A:2008 Mouse             	id=11	[slave  pointer  (2)]
⎜   ↳ ASUP1205:00 093A:2008 Touchpad          	id=12	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Video Bus                               	id=6	[slave  keyboard (3)]
    ↳ Video Bus                               	id=7	[slave  keyboard (3)]
    ↳ Power Button                            	id=8	[slave  keyboard (3)]
    ↳ Sleep Button                            	id=9	[slave  keyboard (3)]
    ↳ ITE5570:00 0B05:19B6 Wireless Radio Control	id=13	[slave  keyboard (3)]
    ↳ Asus WMI hotkeys                        	id=14	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=15	[slave  keyboard (3)]

~ 

```
3. 列出触摸板当前参数(带Touchpad的是电脑触摸板,我这里触摸板的id是12)
```shell
xinput list-props 12
```
4. 设置触摸板逆向滚动(这里要根据上一步列出的参数修改了)  
    ```
    ❯ xinput list-props 12 
    Device 'ASUP1205:00 093A:2008 Touchpad':
        Device Enabled (156):	1
        Coordinate Transformation Matrix (158):	1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
        libinput Tapping Enabled (315):	1
        libinput Tapping Enabled Default (316):	0
        libinput Tapping Drag Enabled (317):	1
        libinput Tapping Drag Enabled Default (318):	1
        libinput Tapping Drag Lock Enabled (319):	0
        libinput Tapping Drag Lock Enabled Default (320):	0
        libinput Tapping Button Mapping Enabled (321):	1, 0
        libinput Tapping Button Mapping Default (322):	1, 0
        libinput Natural Scrolling Enabled (286):	1
        libinput Natural Scrolling Enabled Default (287):	0
        libinput Disable While Typing Enabled (323):	1
        libinput Disable While Typing Enabled Default (324):	1
        libinput Scroll Methods Available (288):	1, 1, 0
        libinput Scroll Method Enabled (289):	1, 0, 0
        libinput Scroll Method Enabled Default (290):	1, 0, 0
        libinput Click Methods Available (325):	1, 1
        libinput Click Method Enabled (326):	1, 0
        libinput Click Method Enabled Default (327):	1, 0
        libinput Middle Emulation Enabled (295):	0
        libinput Middle Emulation Enabled Default (296):	0
        libinput Accel Speed (297):	0.680000
        libinput Accel Speed Default (298):	0.000000
        libinput Accel Profiles Available (299):	1, 1, 1
        libinput Accel Profile Enabled (300):	1, 0, 0
        libinput Accel Profile Enabled Default (301):	1, 0, 0
        libinput Accel Custom Fallback Points (302):	<no items>
        libinput Accel Custom Fallback Step (303):	0.000000
        libinput Accel Custom Motion Points (304):	<no items>
        libinput Accel Custom Motion Step (305):	0.000000
        libinput Accel Custom Scroll Points (306):	<no items>
        libinput Accel Custom Scroll Step (307):	0.000000
        libinput Left Handed Enabled (308):	0
        libinput Left Handed Enabled Default (309):	0
        libinput Send Events Modes Available (271):	1, 1
        libinput Send Events Mode Enabled (272):	0, 0
        libinput Send Events Mode Enabled Default (273):	0, 0
        Device Node (274):	"/dev/input/event19"
        Device Product ID (275):	2362, 8200
        libinput Drag Lock Buttons (310):	<no items>
        libinput Horizontal Scroll Enabled (311):	1
        libinput Scrolling Pixel Distance (312):	15
        libinput Scrolling Pixel Distance Default (313):	15
        libinput High Resolution Wheel Scroll Enabled (314):	1

    ```
    `Natural Scrolling Enabled自然滚动`可以试试
    ```
    sudo xinput set-prop 12 "libinput Natural Scrolling Enabled" 1
    sudo xinput set-prop 12 "libinput Natural Scrolling Enabled" 0
    ```
    如果上面不管事,我们可以通过设置滚动距离为正数或者负数,实现自然滚动
    ```
    sudo xinput set-prop 12 "libinput Scrolling Pixel Distance" 15
    sudo xinput set-prop 12 "libinput Scrolling Pixel Distance" -15
    ```
    要注意上面的命令未必适用于所有人,比如我另一台电脑需要使用`Synaptics Scrolling Distance`设置竖直和水平滚动  
    ```
    sudo xinput set-prop 12 "Synaptics Scrolling Distance" -30 -30
    ```

    # 后记
    1. 不能水平滚动?  
    把带`Horizontal Scroll Enabled`的设置成1即可
    ```
    sudo xinput set-prop 12 "libinput Tapping Enabled" 1
    ```
    2. 锁屏界面无法触摸板点击?  
    [kali任意锁屏界面解锁触摸板点击等功能](kali任意锁屏界面解锁触摸板点击等功能.md)