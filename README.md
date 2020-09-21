# #Depian-Project
### Q&A: 什么是Depian-Project?  
Depian-Project立志于将Deepin环境以容器的形式安装进Debian系统，将Debian打造成更完美好用的Linux发行版，与Arch,Manjaro等发行版在生态上平起平坐。  
## 一,部署并构建Deepin-Rootfs容器
### 1,安装所需工具软件包
`$ sudo apt install debootstrap systemd-container -y`
### 2,创建容器所在目录
`$ mkdir "文件夹名称(建议英文)"`
### 3,寻找构建Rootfs所需镜像
无特定，这里建议使用一下URL镜像站:  
DeepinV15:  
http://mirrors.ustc.edu.cn/deepin/  
DeepinV20:  
https://community-packages.Deepin.com/Deepin/  
*Tag:镜像站内目录中的"stable""unstable""bbuster"等，为发行版代号(codename)，在/usr/share/debootstrap/scripts中有与之对应的脚本。*  
*PS:DeepinV20(codename:apricot)在debootstrap中没有相对应的脚本，建议将"stable"或"unstable"复制为"apricot"。*  
`$ sudo cp /usr/share/debootstrap/scripts/stable /usr/share/debootstrap/scripts/apricot`  
或  
`$ sudo cp /usr/share/debootstrap/scripts/unstable /usr/share/debootstrap/scripts/apricot`  
### 4,使用debootstrap构建Deepin-Rootfs
`$ sudo debootstrap --include=systemd-container --no-check-gpg "镜像中的codename" "容器文件夹名称" "镜像URL"`  
*Tag:"--no-check-gpg"禁用gpg文件校验。*  
### 5,配置容器内Deepin系统的root账户密码并编写启动脚本
临时进入Deepin  
`$ sudo systemd-nspawn -D "文件夹名称"`  
设置root账户密码  
`root@"Host":# passwd`  
退出Deepin临时登陆  
`root@"Host":# exit`  
编写完整启动脚本(你也可以选择别的编辑器)  
`$ vim "自定义脚本名称"`  
脚本中的内容:  
``` 
#解释器
#!/bin/bash
#启动指令(其中--bind选项可以将容器中的图像声音输出到宿主系统中，语言支持推荐填写ZH_CN:zh,-b选项可让容器系统作为一个完整的系统启动)
sudo systemd-nspawn --bind=/tmp/.X11-unix:/tmp/.X11-unix --bind=/run/user/1000/pulse:/run/user/host/pulse --setenv=LANGUAGE="语言支持" -bD "容器所在文件夹名称"
```
保存脚本内容，设置权限x  
`$ sudo chmod +x "脚本名称"`
之后就可以通过脚本启动容器  
`$ ./"脚本名称"`
