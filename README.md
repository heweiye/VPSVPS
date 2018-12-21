#### 系统软件源日常更新
``` bash
apt-get update
apt-get upgrade
apt-get install curl unzip sudo -y
```

#### aria2安装并整合rclone自动上传
``` bash
wget -N https://raw.githubusercontent.com/heweiye/aria2.sh/master/aria2.sh && chmod +x aria2.sh && bash aria2.sh
```
使用说明
``` bash
启动：/etc/init.d/aria2 start
停止：/etc/init.d/aria2 stop
重启：/etc/init.d/aria2 restart
查看状态：/etc/init.d/aria2 status
配置文件：/root/.aria2/aria2.conf
默认下载目录：/root/Download
```
附加功能
`delete.aria2.sh` 下载完成后删除 .aria2 文件。
`delete.sh` 下载错误和停止后删除文件（包括 .aria2 文件），避免占用空间。
`autoupload.sh` 下载完成后自动调用 rclone 上传(move)到网盘，删除 .aria2 文件，过滤无用文件等。
#### 安装rclone并配置自动挂载磁盘
安装
``` bash
curl https://rclone.org/install.sh | sudo bash
```
配置rclone开机自动挂载
``` bash
wget https://github.com/heweiye/VPSVPS/raw/master/rcloned && nano rcloned
```
修改一下内容：
``` bash
NAME=""  #rclone name名，及配置时输入的Name
REMOTE=''  #远程文件夹，Google Drive网盘里的挂载的一个文件夹
LOCAL=''  #挂载地址，VPS本地挂载目录
```
使用命令：
#Debian系统
``` bash
mv rcloned /etc/init.d/rcloned
chmod +x /etc/init.d/rcloned
update-rc.d -f rcloned defaults
bash /etc/init.d/rcloned start
```
检测信息显示rclone启动成功即可。
