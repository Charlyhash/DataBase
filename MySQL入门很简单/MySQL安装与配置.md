## MySQL安装与配置

Wiindow下安装与配置方法：

* 下载免安装版
* 解压
* 添加环境变量

```
 D:\Program Files\mysql;D:\Program Files\mysql\bin
```

* 在mysql下，复制my-default.ini,并命名为my.ini.进行一些设置

``` 
basedir = D:\Program Files\mysql
datadir = D:\Program Files\mysql\data
port = 3306
server_id = 10
character-set-server = utf8
```

* 打开服务：net start mysql
* 关闭服务：net stop mysql

* 设置密码：