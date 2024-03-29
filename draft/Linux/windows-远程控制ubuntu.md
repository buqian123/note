# 使用Xshell连接Ubuntu
Ubuntu主机没有开启SSH服务，需要开启openssh-server：
```sudo apt-get install openssh-server```
使用
```ps -e | grep ssh```
如果只有ssh-agent表示还没启动，需要
``` /etc/init.d/ssh start```
如果显示sshd则说明已启动成功。
#### 传输文件
>```sudo apt-get install -y lrzsz```
```rz```把Windows的文件上传到linux服务器上
```sz```下载

# Screen

解决ssh进行远程连接的时候，有时候如果不小心断了连接之后，正在进行的任务就会被杀死

```sudo apt install screen```

```
screen -S yourname -> 新建一个叫yourname的session
screen -ls         -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session
screen -X -S yourname quit  -> 清除Detached会话
screen -wipe      ->清除Dead会话
```
# ssh
```ssh tomding@192.168.1.104 -L 127.0.0.1:12345:127.0.0.1:1111```
首先输入这个命令，user_name就是你的服务器的你的个人的用户名， server_ip就是你的服务器的独有ip

127.0.0.1:12345为本地的地址 127.0.0.1:1111为服务器端的地址

大白话理解就是咱们用ssh把本地的127.0.0.1:12345端口地址映射对应于服务器端的127.0.0.1:1111，在本地访问127.0.0.1:12345就相当于访问了127.0.0.1:1111
###### scp传输文件
```scp xrdp-installer-1.2.sh zut_csi@10.36.34.100:/home```
出现Permission denied-
```sudo chmod 754 folderName```
# zerotier
[downloads](https://www.zerotier.com/download/)
[管理](https://my.zerotier.com/network)
 在线安装zerotier
```curl -s https://install.zerotier.com | sudo bash```
 查看安装zerotier版本
```sudo zerotier-cli status```
加入一个netWork
```sudo zerotier-cli join （networkid）```
查看加入的网络的信息，比如network
```sudo zerotier-cli listnetworks```
## 退出加入的network网段
```sudo zerotier-cli leave（networkid）```
开机自启动
```sudo systemctl enable zerotier-one.service```
启动zerotier-one.service
``` sudo systemctl start zerotier-one.service```
加入网络
```sudo zerotier-cli join （networkid）```

# 使用team viewer
安装下好的deb包
```  sudo dpkg -i teamviewer_13.1.8286_amd64.deb```
缺少依赖
```sudo apt install -f```
安装下好的deb包
```  sudo dpkg -i teamviewer_13.1.8286_amd64.deb```





```sudo /usr/local/sunlogin/bin/sunloginclient```