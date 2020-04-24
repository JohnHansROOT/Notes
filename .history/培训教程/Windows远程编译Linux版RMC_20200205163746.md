# Windows远程编译Linux版本RMC:

## 1.先用iNode连接至Real内网
inode用户名：mayugao
密码：mayugao

![](2020-02-05-16-27-12.png)

## 2.VS中配置编译服务器
IP: 192.168.9.27
这应该是打开之前发的Linux版本的RMC.sln就已经自动显示配置好了，不用更改

![](2020-02-05-16-26-25.png)

## 3.编译
## 4.登录服务器查看输出
* 通过Xshell命令行查看，或者用XFTP进行文件传输
用户名：mayg
密码: mayugao
端口：22
//注：用XFTP中协议用SFTP

* 编译输出out文件目录：
`/home/mayg/projects/RMC/bin/x64/Release`

* 注：多人编译时，输出到同一个文件夹下同名文件会覆盖，除非你编译时在VS中更改一下输出目录

![](2020-02-05-16-37-41.png)