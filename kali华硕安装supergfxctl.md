# kali华硕安装[supergfxctl](https://asus-linux.org/)
[supergfxctl项目链接](https://gitlab.com/asus-linux/supergfxctl)  
根据网站介绍一步步向下来即可, 无坑  
+ 安装依赖
```
sudo apt update && sudo apt install curl git build-essential
```
+ 安装rust
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```
+ 克隆源码
```
git clone https://gitlab.com/asus-linux/supergfxctl.git
```
+ 编译安装
```
cd supergfxctl
make && sudo make install
```

# kde桌面环境安装supergfxctl-plasmoid
[supergfxctl-plasmoid项目链接](https://gitlab.com/Jhyub/supergfxctl-plasmoid)  
官方不支持debian系, 这里有坑
## 安装依赖
### 我根本没安装完这些数以十计依赖, 需要先在虚拟机装一遍, 万万不可真机开索!
+ 部分软件包kali和ubuntu同名, 可直接安装
```
sudo apt install -y cmake extra-cmake-modules g++ qt6-base-dev qt6-declarative-dev
```
+ (坑)部分软件包kali和ubuntu不同名, 需改变名称
```
sudo apt install -y libkf6config-dev libkf6i18n-dev
```  
+ (天坑)部分软件包(libplasma-dev)kali没有对应的包, 需从源码编译安装
    - 安装libplasma依赖
    ```
    sudo apt install build-essential cmake qtbase5-dev qtdeclarative5-dev libqt5svg5-dev
    ```
    - 克隆[libplasma项目源码](https://invent.kde.org/plasma/libplasma), 当前(2024-9-27)最新标签是v2.0.0, 但是如下所示过新的版本无法兼容电脑旧版依赖
    ```
    CMake Error at CMakeLists.txt:46 (find_package):
  Could not find a configuration file for package "Qt6" that is compatible
  with requested version "6.7.0".

  The following configuration files were considered but not accepted:

    /usr/lib/x86_64-linux-gnu/cmake/Qt6/Qt6Config.cmake, version: 6.6.2
    /lib/x86_64-linux-gnu/cmake/Qt6/Qt6Config.cmake, version: 6.6.2
    ```
    使用旧版v6.1.5
    ```
    git clone --branch v6.1.5 --depth 1 https://invent.kde.org/plasma/libplasma.git
    ```
    - 进入目录, 编译安装
    ```
    cd libplasma
    mkdir build
    cd build
    cmake ..
    make
    sudo make install
    ```
    ## 🌿! 别装了! 过量的依赖根本装不完! 
    ### 能去ubuntu官网下载这些依赖
    + https://launchpad.net/ubuntu/oracular/amd64/libplasma-dev/6.1.5-0ubuntu1  
    + https://launchpad.net/ubuntu/oracular/amd64/libkf6package-dev/6.6.0-0ubuntu1  
    + https://launchpad.net/ubuntu/oracular/amd64/libkf6windowsystem-dev/6.6.0-0ubuntu1  
    + https://launchpad.net/ubuntu/oracular/amd64/libplasma6/6.1.5-0ubuntu1  
    + https://launchpad.net/ubuntu/oracular/amd64/libplasmaquick6/6.1.5-0ubuntu1  
    + https://launchpad.net/ubuntu/oracular/amd64/plasma-desktoptheme/6.1.5-0ubuntu1  
    ...  
    ## 还有个更简单的方法, 本地跑个ubuntu24.10虚拟机, 必须是24.10版本
    在ubuntu: `sudo apt install libplasma-dev` 能看到需要下载哪些软件包.  
    在kali创建个共享目录, 在ubuntu挂载该目录.  
    在kali缺什么软件包就去ubuntu下载对应的软件包, 然后kali手动安装该包.  
    ...
    ## 邪教
    把kali源改成ubuntu24.10的, 必须是24.10的, 然后apt install一把索, 万万不可upgrade升级系统, 安装好后再改回原来的软件源
