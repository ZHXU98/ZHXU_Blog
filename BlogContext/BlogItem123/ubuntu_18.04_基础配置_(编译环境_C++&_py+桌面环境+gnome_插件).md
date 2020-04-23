## ubuntu_18.04_基础配置_(编译环境_C++&_py+桌面环境+gnome_插件)

# PC 型号

##### 本人电脑是 小米pro 15.6寸 MX150 版本 ubuntu上无指纹支持驱动(目前 2019:7:25 我觉得 20 年前 是不太可能的
22年有可能出)

ps： 如果非要享受 指纹这玩意 建议debian 这个针对小米pro 出过一系列更新 有指纹什么的

评价小米pro 15.6 MX150 屏幕出众 但也就屏幕出众了… 大部分机子 用久了 尤其是风扇高速转 突然停转 发出异响 多次这样
低速时也会有较小的异响 硬件问题 用久了 重度用的 都有

官网下的 ubuntu 镜像 U盘启动器直接烧一个就行

### 建议最小安装 不按照不必要应用

默认安装 双系统 什么的也就是 往空盘直接放 我都默认按的 (本身win10 双系统)

安装完之后  
一下软件  
直接再 软件更新 应用里换 阿里源

有些 更新 和 软件要求重启 我们之后统一处理

<https://www.wps.cn/product/wpslinux/> WPS  
<http://www.ubuntuchrome.com/> chrome google  
<https://music.163.com/#/download> 网易云音乐  
<https://code.visualstudio.com/download> vscode  
sudo apt install git ///git

从设置找到语言->管理安装语言，待更新安装完成，设置为fcitx  
<https://pinyin.sogou.com/linux/?r=pinyin> 安装 SOUGOU 这个有些机子要重启 暂时别重启  
在这个github 里面选择TIM 微信安装  
<https://github.com/wszqkzqk/deepin-wine-ubuntu>

C++ 环境 其实正常你更新了 这些环境都是ubuntu自带的

## c++

    
    
    	sudo apt-get install vim
        sudo apt-get install gcc
        sudo apt-get install g++
        sudo add-apt-repository ppa:pasgui/ppa
        sudo apt-get update
        sudo apt-get install codeblocks
        sudo apt-get install codeblocks-contrib
    

vscode 配置 c++ py  
py 暂时不升级 下载pip  
c++ launch.json

    
    
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "(gdb) Launch",
                "type": "cppdbg",
                "request": "launch",
                "program": "${workspaceFolder}/${fileBasenameNoExtension}.out",
                "args": [],
                "stopAtEntry": false,
                "cwd": "${workspaceFolder}",
                "environment": [],
                "externalConsole": true,
                "MIMode": "gdb",
                "preLaunchTask": "build", 
                "setupCommands": [
                    {
                        "description": "Enable pretty-printing for gdb",
                        "text": "-enable-pretty-printing",
                        "ignoreFailures": true
                    }
                ]
            }
        ]
    }
    

c++ settings.json

    
    
    {
        "files.associations": {
            "iostream": "cpp"
        }
    }
    

c++ taks.json

    
    
    {
        "tasks": [
            {
                "label": "build",
                "type": "shell",
                "command": "g++",
                "args": ["-g", "${file}", "-std=c++11", "-o", "${fileBasenameNoExtension}.out"]
            }
         ]
    }
    

py launch.json

    
    
    {
        // 使用 IntelliSense 了解相关属性。 
        // 悬停以查看现有属性的描述。
        // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            
            {
                "name": "Python",
                "type": "python",
                "request": "launch",
                "program": "${file}",
                "console": "externalTerminal"
                //“integratedTerminal”
                // "externalTerminal"
            }
        ]
    }
    
    
    
    # 1. 更新系统包
    sudo apt-get update
    sudo apt-get upgrade
    
    # 2. 安装Pip
    sudo apt-get install python-pip
    
    # 3. 检查 pip 是否安装成功
    pip -V
    

显卡驱动  
阿里源 一般NV都有 直接再软件和更新 里找 附加驱动就行  
现在版本是390 官方给了 431 版本了 有点小问题 还行 不建议去更新  
小米pro 开集显就好 如果你想玩游戏 还是win吧 切换很方便

# TLP 和 GRUB 修复引导

Tlp 简直NB

## 如果出现 耳机杂音 开启TLP 请关掉

编辑 tlp 配置文件 让 `sound_save_power = 0` 重点划线

安装完tlp 和//// 一般不需要 grub 修复  
tlp 下载安装不需要配置

### grub

引导修复 ps 一般用不上

    
    
    sudo add-apt-repository ppa:yannubuntu/boot-repair
    sudo apt-get update
    sudo apt-get install -y boot-repair
    

### TLP

不需要配置 按完就好 默认auto的 笔记本够用的

    
    
     sudo add-apt-repository ppa:linrunner/tlp
    sudo apt-get update
    sudo apt-get install tlp tlp-rdw
     sudo apt-get insta
    

Gnome 插件 不一定非得要Gnome 桌面 大部分可以直接用的  
建议百度 最小安装 桌面  
//// //sudo apt-get install gnome

gnome 插件

### GNOME Shell 扩展

    
    
    sudo apt install gnome-tweak-tool 管理各种美化的用的
    sudo apt install chrome-gnome-shell  // usr主题 插件
    

<https://extensions.gnome.org/>  
我用的 不多 以下直接搜就好  
其实还有剪切板插件 Clipboard Indicator  
截图插件 Screenshot Tool

    
    
    Alt-Tab Switcher Popup Delay Removal  
    AlternateTab
    Auto Move Windows  
    Battery Percentage  
    Dynamic Panel Transparency
    Impatience
    OpenWeather  
    Pixel Saver
    TopIcons  
    User Themes
    VSCode Search Provider  
    

### 双系统 时间同步

看情况了

    
    
    sudo apt install ntpdate
    sudo ntpdate time.windows.com
    sudo hwclock --localtime --systohc ok
    

