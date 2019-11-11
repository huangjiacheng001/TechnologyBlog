---

---

### 使用Docker搭建Maven私服

####  1 拉取镜像

```shell
docker pull sonatype/nexus
```

#### 2 创建文件挂在目录

```powershell
mkdir -p /home/nexus/nexus-data
```

`文件夹授权 避免docker无法写入数据到宿主机`

```shell
chmod 777  /home/nexus/nexus-data
```

#### 3 运行容器

```dockerfile
docker run --rm -d --privileged=true -p 8001:8081 --name nexus -v /home/nexus/nexus-data:/var/nexus-data sonatype/nexus3
```

上面命令是指使用nexus3镜像创建并启动一个容器，然后指定暴露8081端口到对应主机的8001端口，将容器内部/var/nexus-data挂载到主机/root/nexus-data目录。
如果没有任何问题的话，Nexus应该是搭建成功了。

#### 4 登录

`http://ip:8001即可看到以下页面：(ip为远程主机的ip地址)`

```
image.png
```

点击右上方的Sign in进行登录，初始账号密码为admin/admin123.请登录后修改密码
如下图：


image.png

可以看到默认情况下Nexus会帮我们创建了几个仓库，仔细观察红色框住的地方，里面有几种仓库的类型，解释如下：

proxy 远程仓库的代理，比如说nexus配置了一个central repository的proxy,当用户向这个proxy请求一个artifact的时候，会现在本地查找，如果找不到，则会从远程仓库下载，然后返回给用户。
hosted 宿主仓库，用户可以把自己的一些仓库deploy到这个仓库中
group 仓库组，是nexus特有的概念，目的是将多个仓库整合，对用户暴露统一的地址，这样就不需要配置多个仓库地址。
下面我们仔细看一下里面的一些仓库。点击maven-central仓库:
image.png

可以看到是一个proxy类型的仓库，他代理的远程仓库地址是https://repo1.maven.org/maven2/。
后退，在进入maven-public查看:


image.png

可以看到这是一个group类型的仓库，里面包含了maven-releases/maven-snapshots/maven-central仓库，意思是我们只需要在本地添加这个仓库，则可以依赖到上述3个仓库中的库了。

准备工作
为了实现本地上传代码库，并且实现依赖的示例，这里创建一个新的仓库(也可以选用已经存在的仓库)和一个用户

创建仓库，点击Create repository,然后选择maven2(hosted)然后输入仓库名称（test-release）。在version policy中选择这个仓库的类型，这里选择release,在Deployment policy中选择Allow redeploy（这个很重要）.
创建成功如下：


image.png
创建用户
点击左侧菜单栏的Users菜单，然后点击Create local user.我这里创建了一个用户，账号密码都是：test

本地操作
修改本地.m2目录下的settings.xml

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
      <server>
        <id>test</id>
        <username>test</username>
        <password>test</password>
      </server>
    </servers>
</settings>
```

上面指定私库的账号密码。
注意，若是用idea工具，要先配置maven指定到对应的目录。

使用IDEA创建一个Maven项目：

pop.xml

```xml
 <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dubbo</artifactId>
        <groupId>com.iti</groupId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
    <packaging>jar</packaging>
    <modelVersion>4.0.0</modelVersion>

​```
<artifactId>dubbo-api</artifactId>

<!--注意限定版本一定为RELEASE,因为上传的对应仓库的存储类型为RELEASE-->
<version>1.0-RELEASE</version>
<!--指定仓库地址-->
<distributionManagement>
    <repository>
        <!--此名称要和.m2/settings.xml中设置的ID一致-->
        <id>test</id>
        <url>http://172.21.13.229:8081/repository/test-release/</url>
    </repository>
</distributionManagement>

<build>
    <plugins>
        <!--发布代码Jar插件-->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-deploy-plugin</artifactId>
            <version>2.7</version>
        </plugin>
        <!--发布源码插件-->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>3.0.0</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
</project>
```




简单的代码：

```java
public class TestUtil {
    public static void main(String[] args) {
        System.out.println("I am from test-lib！");
    }
}
```


一切准备就绪后，打开终端，输入mvn deploy

如无意外，上传成功，回到Nexus的网页中查看结果


image.png
可以看到已经上传成功。

测试依赖
本地再次创建一个项目，对这个项目进行依赖.

在pom.xml添加如下代码

``` xml
<dependencies>
        <dependency>
            <groupId>com.iti</groupId>
            <artifactId>dubbo-api</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.iti</groupId>
            <artifactId>dubbo-config</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>

<repositories>
    <repository>
        <!--此名称要和.m2/settings.xml中设置的ID一致-->
        <id>test</id>
        <url>http://172.21.13.229:8081/repository/test-release/</url>
    </repository>
</repositories>
```



然后发现依赖成功了
至此完成了Docker中使用Nexus部署maven私有仓库的所有步骤。

`问题：`
如果配域名，需要在nginx配置跨域请求头信息:

```shell
proxy_set_header X-Forwarded-Host $host;
proxy_set_header X-Forwarded-Server $host;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
```

 #### 完整配置

``` shell
 server {
        listen 80;
        server_name  nexus.01home.cn;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        location / {
                proxy_pass http://117.48.231.93:8001;
                                proxy_connect_timeout 600;
                                proxy_read_timeout 600;
        }
    }
```



