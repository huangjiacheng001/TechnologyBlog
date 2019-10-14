# Docker 安装 Nginx

# 

### docker pull nginx 命令安装

查找 [Docker Hub](https://hub.docker.com/r/library/nginx/) 上的 nginx 镜像

```
runoob@runoob:~/nginx$ docker search nginx
NAME                      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                     Official build of Nginx.                        3260      [OK]       
jwilder/nginx-proxy       Automated Nginx reverse proxy for docker c...   674                  [OK]
richarvey/nginx-php-fpm   Container running Nginx + PHP-FPM capable ...   207                  [OK]
million12/nginx-php       Nginx + PHP-FPM 5.5, 5.6, 7.0 (NG), CentOS...   67                   [OK]
maxexcloo/nginx-php       Docker framework container with Nginx and ...   57                   [OK]
...
```

这里我们拉取官方的镜像

```
$ docker pull nginx
```

等待下载完成后，我们就可以在本地镜像列表里查到 REPOSITORY 为 nginx 的镜像。

```
runoob@runoob:~/nginx$ docker images nginx
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              555bbd91e13c        3 days ago          182.8 MB
```

以下命令使用 NGINX 默认的配置来启动一个 Nginx 容器实例：

```
$ docker run --name runoob-nginx-test -p 8081:80 -d nginx
```

- `runoob-nginx-test` 容器名称。
- the `-d`设置容器在在后台一直运行。
- the `-p` 端口进行映射，将本地 8081 端口映射到容器内部的 80 端口。

执行以上命令会生成一串字符串，类似 **6dd4380ba70820bd2acc55ed2b326dd8c0ac7c93f68f0067daecad82aef5f938**，这个表示容器的 ID，一般可作为日志的文件名。

我们可以使用 docker ps 命令查看容器是否有在运行：

```
$ docker ps
CONTAINER ID        IMAGE        ...               PORTS                  NAMES
6dd4380ba708        nginx        ...      0.0.0.0:8081->80/tcp   runoob-nginx-test
```

PORTS 部分表示端口映射，本地的 8081 端口映射到容器内部的 80 端口。

在浏览器中打开 **http://127.0.0.1:8081/**，效果如下：

![img](https://www.runoob.com/wp-content/uploads/2016/06/23F2B078-C579-4C09-B789-25C64C6934A2.jpg)

------

## nginx 部署

首先，创建目录 nginx, 用于存放后面的相关东西。

```
$ mkdir -p /opt/nginx/html /opt/nginx/logs /opt/nginx/conf
```

拷贝容器内 Nginx 默认配置文件到本地当前目录下的 conf 目录，容器 ID 可以查看 **docker ps** 命令输入中的第一列：

```
docker cp 6dd4380ba708:/etc/nginx/nginx.conf /opt/nginx/conf
```

- **www**: 目录将映射为 nginx 容器配置的虚拟目录。
- **logs**: 目录将映射为 nginx 容器的日志目录。
- **conf**: 目录里的配置文件将映射为 nginx 容器的配置文件。

### 部署命令

```
$ docker run -d -p 80:80 --name nginx001 -v /opt/nginx/html:/usr/share/nginx/html -v /opt/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /opt/nginx/logs:/var/log/nginx nginx
```

命令说明：

- **-p 8082:80：** 将容器的 80 端口映射到主机的 8082 端口。
- **--name runoob-nginx-test-web：**将容器命名为 runoob-nginx-test-web。
- **-v /opt/nginx/html:/usr/share/nginx/html：**将我们自己创建的 html目录挂载到容器的 /usr/share/nginx/html。
- **-v /opt/nginx/conf/nginx.conf:/etc/nginx/nginx.conf：**将我们自己创建的 nginx.conf 挂载到容器的 /etc/nginx/nginx.conf。
- **-v /opt/nginx/logs:/var/log/nginx：**将我们自己创建的 logs 挂载到容器的 /var/log/nginx。

启动以上命令后进入 ~/nginx/html目录：

```
$ cd ~/nginx/html
```

创建 index.html 文件，内容如下：

`touch index.html`

`vim index.html`

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
</body>
</html>
```

输出结果为：

![img](https://www.runoob.com/wp-content/uploads/2016/06/B0DFB2A6-E1B5-4502-8EC4-0687A7C880FA.jpg)

### 相关命令

如果要重新载入 NGINX 可以使用以下命令发送 HUP 信号到容器：

```
$ docker kill -s HUP container-name
```

重启 NGINX 容器命令：

```
$ docker restart container-name
```