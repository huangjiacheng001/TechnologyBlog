然后进入终端解压：tar -xvf nginx-1.10.0.tar.gz

然后

然后安装依赖包（在nginx目录下进行）

由于nginx中的功能都是模块化的，而模块又依赖于一些软件包(如pcre库、zlib库和openssl库)才能使用。故，安装nginx之前，需要先完成nginx模块依赖的软件包的安装。

pcre-devel包：为nginx模块提供正则表达式库
zlib-devel包：为nginx模块提供数据压缩用的函数库
openssl-devel包：为nginx模块提供密码算法、证书以及SSL协议等功能

通过yum命令方式来安装 yum -y install pcre-devel openssl-devel

这里，没有通过yum命令来安装zlib-devel包，且看安装过程，安装中，yum命令会自动帮忙解决依赖关系。

待安装完毕时，我们可以看到已安装的程序包有openssl-devel 和 pcre-devel，并可以看到作为依赖被安装的程序包中包含了我们原本需要安装的zlib-devel包。

然后配置安装目录：（在nginx目录下进行）
./configure --prefix=/opt/nginx --sbin-path=/usr/bin/nginx

然后安装：make && make install

然后通过 nginx 命令 启动nginx

如果正确启动，则这里可以看到两个进程。
（这里如果错误则可能是防火墙没有关闭，可以关闭防火墙再次尝试，关闭防火墙后记得重启）


