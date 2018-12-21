#### 系统软件源日常更新
``` bash
apt-get update
apt-get upgrade
apt-get install curl nano unzip git sudo -y
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
默认aria2是正常下载，如需要整合rrclone上传后删除本地文件
``` bash
vi /root/.aria2/autoupload.sh
name='Onedrive' #配置Rclone时的name
folder='/DRIVEX/Download'   #网盘里的文件夹，留空为整个网盘。
vi /root/.aria2/aria2.conf
# 下载完成后执行的命令
# 删除.aria2文件
#on-download-complete=/root/.aria2/delete.aria2.sh
# 调用 rclone 上传(move)到网盘
on-download-complete=/root/.aria2/autoupload.sh
重启 Aria2
service aria2 restart
```
#### 安装rclone并配置自动挂载磁盘
``` bash
apt-get install fuse -y
curl https://rclone.org/install.sh | sudo bash
```
配置rclone开机自动挂载
``` bash
wget https://raw.githubusercontent.com/heweiye/VPSVPS/master/rcloned && nano rcloned
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
#### 安装GDLIST
``` bash
git clone https://github.com/reruin/gdlist.git
cd gdlist
docker-compose up -d
```
访问 http://localhost:33001 WebDAV 目录 http://localhost:33001/webdav
<br>通过Caddy添加域名SSL反代ShareList
以下全部内容是一个整体，修改域名、IP、邮箱后一起复制到SSH运行！
``` bash
echo "a.com {
 tls admin@a.com
 proxy / 111.222.333.444:33001 {
    header_upstream Host {host}
    header_upstream X-Real-IP {remote}
    header_upstream X-Forwarded-For {remote}
    header_upstream X-Forwarded-Proto {scheme}
  }
 gzip
}" >> /usr/local/caddy/Caddyfile
/etc/init.d/caddy restart
```
Google Drive 分享链接一般是https://drive.google.com/drive/folders/xxxx?usp=sharing，则ID为xxxx。 
<br>OneDrive 分享链接一般是https://1drv.ms/f/xxxx，则ID为xxxx。

#### gost.sh
``` bash
wget https://raw.githubusercontent.com/heweiye/VPSVPS/master/gost.sh && chmod +x gost.sh && bash gost.sh
```
