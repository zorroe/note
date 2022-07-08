# Lxxx

> 记录所有

## 常用软件命令方式安装

1. node.js

```shell
wget https://npmmirror.com/mirrors/node/v16.15.1/node-v16.15.1-linux-x64.tar.xz
tar xf node-v16.15.1-linux-x64.tar.xz

# 设置软链接
ln -s /usr/software/nodejs/bin/npm   /usr/local/bin/ 
ln -s /usr/software/nodejs/bin/node   /usr/local/bin/
```

2. mysql8

```shell
yum list installed | grep mysql
yum remove mysql80-community-release.noarch
yum clean all --verbose
yum update
rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/mysql-community.repo
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
yum --enablerepo=mysql80-community install mysql-community-server
mysql -V
yum update
systemctl start mysqld
systemctl status mysqld

# 查看生成的默认密码
grep 'temporary password' /var/log/mysqld.log
# 登录
mysql -uroot -p

ALTER user 'root'@'localhost' IDENTIFIED BY 'LIxuan549826...';
set global validate_password.policy=0;
set global validate_password.mixed_case_count=0;
set global validate_password.number_count=0; 
set global validate_password.special_char_count=0; 
set global validate_password.length=6;  
ALTER user 'root'@'localhost' IDENTIFIED BY 'root';
flush privileges;

# 允许远程
mysql -u root -p
use mysql
update user set host="%" where user="root";
flush privileges;
```

## 常用软件Docker安装

1. Docker

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
systemctl start docker
```

2. MySQL

```shell
docker run --name mysql -v /opt/mysql_data:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

3. Redis

```shell
docker run --name redis -p 6379:6379 -d redis:7.0.1
```

4. MongoDB

```shell
docker run -d --name mongod \
	-e MONGO_INITDB_ROOT_USERNAME=lixuan \
    -e MONGO_INITDB_ROOT_PASSWORD=123456 \
    -p 27017:27017 \
    mongo:4.2 --auth 
```

## 测试自动拉取git
