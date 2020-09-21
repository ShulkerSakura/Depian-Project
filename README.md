# #Depian-Project
### Q&A: 什么是Depian-Project?<br>
Depian-Project立志于将Deepin环境以容器的形式安装进Debian系统，将Debian打造成更完美好用的Linux发行版，与Arch,Manjaro等发行版在生态上平起平坐。<br>
## 一,部署并构建Deepin-Rootfs容器
### 1,安装所需工具软件包
`$ sudo apt install debootstrap systemd-container -y`
### 2,创建容器所在目录
`$ mkdir "文件夹名称(建议英文)"`
### 3,寻找构建Rootfs所需镜像
无特定，这里建议使用一下URL镜像站:<br>
DeepinV15:<br>
http://mirrors.ustc.edu.cn/deepin/<br>
DeepinV20:<br>
https://community-packages.Deepin.com/Deepin/<br>
*Tag:镜像站内目录中的"stable""unstable""bbuster"等，为发行版代号(codename)，在/usr/share/debootstrap/scripts中有与之对应的脚本。*<br>
*PS:DeepinV20(codename:apricot)在debootstrap中没有相对应的脚本，建议将"stable"或"unstable"复制为"apricot"。*<br>
`$ sudo cp /usr/share/debootstrap/scripts/stable /usr/share/debootstrap/scripts/apricot`<br>
或<br>
`$ sudo cp /usr/share/debootstrap/scripts/unstable /usr/share/debootstrap/scripts/apricot`<br>
### 4,使用debootstrap构建Deepin-Rootfs
`$ sudo debootstrap --include=systemd-container --no-check-gpg "镜像中的codename" "容器文件夹名称" "镜像URL"`<br>
*Tag:"--no-check-gpg"禁用gpg文件校验。*<br>
### 5,配置容器内Deepin系统的root账户密码并编写启动脚本
临时进入Deepin<br>
`$ sudo systemd-nspawn -D "文件夹名称"`<br>
设置root账户密码<br>
`root@"Host":# passwd`<br>
退出Deepin临时登陆<br>
`root@"Host":# exit`<br>

