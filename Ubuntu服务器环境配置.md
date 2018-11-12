# nodejs

```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```
To compile and install native addons from npm you may also need to install build tools:

```
sudo apt-get install -y build-essential
```

# jdk

```
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.tar.gz
```
```
tar -zxvf jdk-8u111-linux-x64.tar.gz   解压tar包
```
```
sudo cp -r jdk-9.0.1 /usr/lib    移动到user/lib

 ```

 vi /etc/profile

 ```
export JAVA_HOME=/usr/lib/jdk-9.0.1
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
```

source /etc/profile  立即生效

# mysql

```
sudo apt-get install mysql-server
```

设置远程访问
```
 sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf  将bind-address = 127.0.0.1注释
 ```

 ```
 GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
 FLUSH PRIVILEGES;
```

```
service mysql restart
```
# nginx

安装gcc g++依赖库

```
apt-get install build-essential
apt-get install libtool
```
安装pcre
```
sudo apt-get install libpcre3 libpcre3-dev
```
安装zlib
```
apt-get install zlib1g-dev
```
安装ssl
```
apt-get install openssl
```

安装nginx

```
wget http://nginx.org/download/nginx-1.11.3.tar.gz
```
```
tar -zxvf nginx-1.11.3.tar.gz
```
```
cd nginx-1.11.3
```
```
./configure --prefix=/usr/local/nginx 
```
make&&make install

# gitlab
```
sudo apt-get install curl openssh-server ca-certificates postfix
```

设置源
```
curl https://packages.gitlab.com/gpg.key 2> /dev/null | sudo apt-key add - &>/dev/null
```
```
vim /etc/apt/sources.list.d/gitlab-ce.list
```
填入deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu trusty main
```
sudo apt-get update
sudo apt-get install gitlab-ce
```
配置文件
```
sudo gitlab-ctl reconfigure
```
重启
```
sudo gitlab-ctl restart
```