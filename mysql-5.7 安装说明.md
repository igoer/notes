#mysql-5.7 解压版安装步骤
mysql-5.7官方下载地址：http://dev.mysql.com/downloads/mysql/#downloads
本教程的安装环境为 Windows

##配置环境变量
1、解压压缩文件如：D:\Program Files\mysql-5.7.10-winx64

2、配置系统 path 环境变量为 D:\Program Files\mysql-5.7.10-winx64\bin

PS：该步骤不是必须的，只是为了使用命令行的时候更加的方便，如果不配置环境变量则下面的所有命令都需要在D:\Program Files\mysql-5.7.10-winx64\bin目录下运行。

##mysql配置文件
在MySQL的安装目录（例如我的是D:\Program Files\mysql-5.7.10-winx64）下，建立 my.ini 配置文件，并在其中添加内容如下
```
[mysqld]
basedir=D:\Program Files\mysql-5.7.10-winx64
datadir=D:\Program Files\mysql-5.7.10-winx64\data
port = 3306
```

##初始化数据库
以管理员自身份打开CMD执行以下命令
```
mysqld --initialize --user=mysql --console
```
如：
```
gNXSw5FTkw
```
在控制台消息尾部会出现随机生成的初始密码(请记录下来，该密码为临时密码需要修改)

##将MySQL添加到系统服务
以管理员自身份打开CMD执行以下命令
```
mysqld --install mysql
```
启动mysql服务
```
net start mysql
```

PS：我在windows server 2012上添加系统服务时成功，但是启动服务报如下异常
```
发生系统错误 2。
系统找不到指定的文件。
```
解决办法需要在 mysql的bin目录下执行添加系统服务

##修改初始密码
以管理员自身份打开CMD执行以下命令
```
mysqladmin -u root -p password
Enter password:   //这里输入上面的v:gNXSw5FTkw
New password:    //重新输入新密码
Confirm new password: //重新输入新密码
```

OK，现在可以使用新密码登录mysql了。

