docker安装jenkins最新版本

1.pull一个jenkins镜像 docker pull jenkins;
这个是安装最新版的jenkins,如果安装旧版本，很多插件安装不上，docker环境下升级又比较麻烦。


image.png
2.查看已经安装的jenkins镜像 docker images;

image.png

查看是否是最新版 docker inspect ba607c18aeb7


image.png
3.创建一个jenkins目录 mkdir /home/jenkins_home;
docker进程有权限读写此目录 chmod 777 /home/jenkins_home
4.启动一个jenkins容器 docker run -d --name jenkins_01 -p 8000:8080 -v /home/jenkins_home:/var/jenkins_home jenkins/jenkins


image.png
5.查看jenkins服务 docker ps | grep jenkins


image.png
6.启动服务端 。localhost:8000


image.png
7.进入容器内部docker exec -it jenkins_01 bash
8.执行：cat /var/jenkins_home/secrets/initialAdminPassword，得到密码并粘贴过去


image.png

；
9.输入密码之后，重启docker镜像 docker restart {CONTAINER ID}，安装完毕。


### 安装问题

1.Jenkins 启动一直显示 Jenkins正在启动,请稍后...
```java
解决方法

需要你进入jenkins的工作目录，打开

hudson.model.UpdateCenter.xml

把

http://updates.jenkins-ci.org/update-center.json

改成

http://mirror.xmission.com/jenkins/updates/update-center.json
```

账号：
huangjiacheng
密码：
77882211qq
fullname:
huangjiacheng
邮箱：
171817712@qq.com
网站地址：
jenkins.01home.cn


