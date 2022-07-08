# Lxxx

> note

## 常用软件安装

### node.js

```shell
wget https://npmmirror.com/mirrors/node/v16.15.1/node-v16.15.1-linux-x64.tar.xz
tar xf node-v16.15.1-linux-x64.tar.xz

# 设置软链接
ln -s /usr/software/nodejs/bin/npm   /usr/local/bin/ 
ln -s /usr/software/nodejs/bin/node   /usr/local/bin/
```

### mysql8

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

