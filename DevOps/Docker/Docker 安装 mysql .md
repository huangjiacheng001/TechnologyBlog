#docker 安装 mysql 8 版本

# docker 中下载 mysql
`docker pull mysql`

#启动
`docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql`

#进入容器
``` docker exec -it mysql bash```

#登录mysql
``` mysql 
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```
*  给root账号授权远程登陆
 ``` mysql ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
 
 grant all privileges on *.* to root@"%" identified by "123456" with grant option; 
 
 ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
  
 flush privileges;
 ```
* 添加远程登录用户
```mysql
CREATE USER 'hjc'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'hjc'@'%';
```
