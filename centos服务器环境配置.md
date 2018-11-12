# 1：上传文件到服务器
1.在linux服务器查看ssh服务是否正在运行：ps -e | grep ssh

2.如果没有运行：service sshd start

3.向linux服务器传输文件： scp -r 本地目录 linux用户名@ip地址:目标路径
# 2：yum安装mysql5.6

连接：https://my.oschina.net/wangmengjun/blog/897758

1：了避免冲突，先移除CenttOS上默认的mysql-libs，使用yum remove mysql-libs完成

2：然后，使用yum clean dbcache清空db缓存

3：下载MySQL rpm安装包   
wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm

4：安装下载好的rpm文件 

rpm -ivh mysql-community-release-el6-5.noarch.rpm

yum install mysql-community-server

5：启动服务

service mysqld start

6：修改密码

默认密码是空的~使用mysql -uroot进入~

use mysql;

update user set password=PASSWORD("YOUR_PASSWORD") where user='root';

flush privileges;

7:密码验证

quit   

mysql -uroot -p

8:设置mysql为开机自动启动

首先查看mysql是否是开机自动启动:chkconfig --list | grep mysql 

chkconfig mysqld on

9:关闭mysql数据库

service mysql stop

# 3：yum安装java8

连接：https://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/

 ### 1.  Download Latest Java Archive

`cd /opt/`
```
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jdk-8u144-linux-x64.tar.gz"
```
`tar xzf jdk-8u144-linux-x64.tar.gz`

### 2.  Install Java   8 with Alternatives

 `cd /opt/jdk1.8.0_144/`
```
alternatives --install /usr/bin/java java /opt/jdk1.8.0_144/bin/java 2

alternatives --config java
```
 At this point JAVA 8 has been successfully installed on your system. We also recommend to setup javac and jar commands path using alternatives
```
alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_144/bin/jar 2

alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_144/bin/javac 2

alternatives --set jar /opt/jdk1.8.0_144/bin/jar

alternatives --set javac /opt/jdk1.8.0_144/bin/javac
```
###  3. Check Installed Java Version

`java -version`

### 4. Setup Java Environment Variables
```
export JAVA_HOME=/opt/jdk1.8.0_144

export JRE_HOME=/opt/jdk1.8.0_144/jre

export PATH=$PATH:/opt/jdk1.8.0_144/bin:/opt/jdk1.8.0_144/jre/bin
```
# 4：yum安装nginx

### 1：安装必要的库

`yum install gc gcc gcc-c++ pcre-devel zlib-devel openssl-deve`

###  2：创建Nginx用户和组

`groupadd www`

//创建一个用户，不允许登陆和不创主目录 

`useradd -s /sbin/nologin -g www -M www`

###  3：下载并解压Nginx

`wget http://nginx.org/download/nginx-1.12.0.tar.gz`

`tar -xzvf nginx-1.12.0`

`cd nginx-1.12.0`

### 4：配置并编译安装

`./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module`

`make && make install`

参数说明：

nginx大部分常用模块，编译时./configure --help以--without开头的都默认安装。

--prefix=PATH ： 指定nginx的安装目录。默认 /usr/local/nginx

--conf-path=PATH ： 设置nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf

--user=name： 设置nginx工作进程的用户。安装完成后，可以随时在nginx.conf配置文件更改user指令。默认的用户名是nobody。--group=name类似

--with-pcre ： 设置PCRE库的源码路径，如果已通过yum方式安装，使用--with-pcre自动找到库文件。使用--with-pcre=PATH时，需要从PCRE网站下载pcre库的源码（版本4.4 - 8.30）并解压，剩下的就交给Nginx的./configure和make来完成。perl正则表达式使用在location指令和 ngx_http_rewrite_module模块中。

--with-zlib=PATH ： 指定 zlib（版本1.1.3 - 1.2.5）的源码解压目录。在默认就启用的网络传输压缩模块ngx_http_gzip_module时需要使用zlib 。

--with-http_ssl_module ： 使用https协议模块。默认情况下，该模块没有被构建。前提是openssl与openssl-devel已安装

--with-http_stub_status_module ： 用来监控 Nginx 的当前状态

--with-http_realip_module ： 通过这个模块允许我们改变客户端请求头中客户端IP地址值(例如X-Real-IP 或 X-Forwarded-For)，意义在于能够使得后台服务器记录原始客户端的IP地址

--add-module=PATH ： 添加第三方外部模块，如nginx-sticky-module-ng或缓存模块。每次添加新的模块都要重新编译（Tengine可以在新加入module时无需重新编译）

###  5：配置Nginx命令和服务并开机启动

`vim /etc/init.d/nginx`
```
#!/bin/bash  
# nginx Startup script for the Nginx HTTP Server  
#  
# chkconfig: - 85 15  
# description: Nginx is a high-performance web and proxy server.  
# It has a lot of features, but it's not for everyone.  
# processname: nginx  
# pidfile: /var/run/nginx.pid  
# config: /usr/local/nginx/conf/nginx.conf  
nginxd=/usr/local/nginx/sbin/nginx  
nginx_config=/usr/local/nginx/conf/nginx.conf  
nginx_pid=/usr/local/nginx/nginx.pid  
 
RETVAL=0  
prog="nginx" 
 
# Source function library.  
. /etc/rc.d/init.d/functions  
 
# Source networking configuration.  
. /etc/sysconfig/network  
 
# Check that networking is up.  
[ ${NETWORKING} = "no" ] && exit 0  
 
[ -x $nginxd ] || exit 0  
 
 
# Start nginx daemons functions.  
start() {  
 
if [ -e $nginx_pid ];then 
   echo "nginx already running...." 
   exit 1  
fi  
 
   echo -n $"Starting $prog: " 
   daemon $nginxd -c ${nginx_config}  
   RETVAL=$?  
   echo  
   [ $RETVAL = 0 ] && touch /var/lock/subsys/nginx  
   return $RETVAL  
 
}  
 
 
# Stop nginx daemons functions.  
stop() {  
        echo -n $"Stopping $prog: " 
        killproc $nginxd  
        RETVAL=$?  
        echo  
        [ $RETVAL = 0 ] && rm -f /var/lock/subsys/nginx /var/run/nginx.pid  
}  
 
 
# reload nginx service functions.  
reload() {  
 
    echo -n $"Reloading $prog: " 
 $nginxd -s reload  
    #if your nginx version is below 0.8, please use this command: "kill -HUP `cat ${nginx_pid}`" 
    RETVAL=$?  
    echo  
 
}  
 
# See how we were called.  
case "$1" in 
start)  
        start  
        ;;  
 
stop)  
        stop  
        ;;  
 
reload)  
        reload  
        ;;  
 
restart)  
        stop  
        start  
        ;;  
 
status)  
        status $prog  
        RETVAL=$?  
        ;;  
*)  
        echo $"Usage: $prog {start|stop|restart|reload|status|help}" 
        exit 1  
esac  
 
exit $RETVAL

```

`cd /etc/rc.d/init.d`

#附加执行权限

`chmod 755 /etc/init.d/nginx`

#开机自启

`chkconfig --level 345 nginx on`

`service nginx start #可选  start | stop | restart | reload | status |  help`

###  6.查看系统IP地址,打开nginx的本地网页

`[root@bogon ~]# ifconfig eth0
`

sudo fuser -k 80/tcp

# 5：安装tomcat

`tar -zxvf apache-tomcat-8.0.33.tar.gz -C /opt`
```
cd /opt/apache-tomcat-8.0.33/bin/
./startup.sh

./shutdown.sh
```

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload
```
or
```
vi /etc/sysconfig/iptables
```

`service iptables restart`


修改默认目录

```
conf/server.xml

</Host>前

<Context path="" docBase="D:/eclipse3.3/jb51.net/tomcat/" debug="0"/>
```

# 6：mysql5.6操作

创建用户

```
create user 'xcoin'@'%' IDENTIFIED BY 'x7C4o6i3N7r8d5S2';

create user 'xcoin' identified by 'x7C4o6i3N7r8d5S2';

grant all privileges on *.* to xcoin;
```

ERROR 1045 (28000): Access denied for user

```
mysql -uroot -p

use mysql

select host,user,password from user;

delete from user where user=' ';

flush privileges;
```


# 端口

查看netstat -lnp|grep 88

 kill -9 1777

