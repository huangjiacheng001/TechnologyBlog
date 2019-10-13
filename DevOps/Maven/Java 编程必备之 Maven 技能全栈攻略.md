## Java 编程必备之 Maven 技能全栈攻略

### Maven 简介

Maven 是 Apache 基金会的一个开源项目，它是研发项目管理和构建的综合工具，为开发人员提供了一个完整的构建生命周期框架。由于 Maven 使用标准的目录布局和默认的构建生命周期，开发团队几乎可以在短时间内自动化项目的构建基础设施。

![在这里插入图片描述](https://images.gitbook.cn/ba453910-e9d7-11e9-8092-c12b22bd7cce)

对于多个开发团队环境，Maven 可以在很短的时间内按照标准设置工作方式。由于大多数项目设置都是简单和可重用的，Maven 使得开发人员在编译、构建、检查、测试自动化和创建报告设置时更加轻松。

### Maven 开发环境配置

Maven 依赖于 JDK，Maven 3.3 以上的版本需要 JDK 1.7 以上。配置 Maven 先检查 JDK 环境。

#### JDK 版本信息检查

##### **Windows**

Windows 下打开命令控制台，输入以下命令：

```bash
c:\> java -version
```

Windows 控制台返回信息如下：

![在这里插入图片描述](https://images.gitbook.cn/f72a8470-e9d7-11e9-928f-d9384dde7caf)

##### **Linux**

Linux 下打开命令终端，输入以下命令：

```bash
  # java -version
```

Linux 命令终端返回信息如下：

![在这里插入图片描述](https://images.gitbook.cn/0f263240-e9d8-11e9-8092-c12b22bd7cce)

##### **macOS**

macOS 打开终端，输入以下命令：

```bash
  java -version
```

macOS 终端返回信息如下：

![在这里插入图片描述](https://images.gitbook.cn/21734140-e9d8-11e9-af43-257c05e1d950)

#### Maven 环境变量设置

Maven 最新版本是 3.6.2，Windows 用户下载 [apache-maven-3.6.2-bin.tar.gz](http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz)，Linux 和 MacOS 用户下载 [apache-maven-3.6.2-bin.zip](http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.zip)。下载地址如下：

> http://maven.apache.org/download.cgi

##### **Windows**

右键“计算机”，选择“属性”，之后点击“高级系统设置”，点击“环境变量”，来设置环境变量，新建系统变量 **MAVEN_HOME**，变量值 **C:\Workspace\apache-maven-3.6.2-bin**。

编辑系统变量 **Path**，添加变量值 **%MAVEN_HOME%\bin;**。

##### **Linux 和 MacOS**

下载 Maven 二进制文件并解压：

```bash
# wget http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.zip
# tar -xvf  apache-maven-3.6.2-bin.tar.gz
# sudo mv -f apache-maven-3.6.2 /usr/local/
```

编辑 /etc/profile 文件，在文件末尾添加如下代码：

```bash
sudo vim /etc/profile
export MAVEN_HOME=/usr/local/apache-maven-3.3.9
export PATH=${PATH}:${MAVEN_HOME}/bin
```

保存文件，并运行如下命令使环境变量生效：

```bash
# source /etc/profile
```

在控制台输入如下命令，如果能看到 Maven 相关版本信息，则说明 Maven 已经安装成功：

```bash
# mvn -v
```

![在这里插入图片描述](https://images.gitbook.cn/3d4858b0-e9d8-11e9-928f-d9384dde7caf)

### Maven POM

Project Object Model（项目对象模型）简称 POM，是 Maven 工程的基本工作单元，是一个 XML 文件，包含了项目的基本信息，用于描述项目如何构建、声明项目依赖。执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0   
http://maven.apache.org/xsd/maven-4.0.0.xsd">  

  <modelVersion>4.0.0</modelVersion>  
  <groupId>cn.qatools</groupId>  
  <artifactId>mavenchat</artifactId>  
  <version>1.0.0</version>  
  <packaging>jar</packaging>  


  <name>Maven In Action</name>  
  <url>http://cn.qatools.topic</url>  

  <dependencies>  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>4.12</version>  
      <scope>test</scope>  
    </dependency>  
  </dependencies>  

</project>  
```

**pom.xml 元素说明**

- project：工程根标签
- modelVersion：模型版本，需要设置为 4.0
- groupId：工程组的标识，它在一个项目中通常是唯一的
- artifactId：工程ID，也是工程名称
- version：工程版本号
- packageing：打包格式，例如 jar、war
- name：Maven 项目名称
- url： 项目地址
- dependencies：项目依赖管理
- dependency：引入一个具体的依赖
- scope：指定依赖的作用范围；例如 compile、provided、runtime、test、system

**pom.xml 完整版**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">
    <!--父项目的坐标。如果项目中没有规定某个元素的值，那么父项目中的对应值即为项目的默认值。 坐标包括group ID，artifact ID和 
        version。 -->
    <parent>
        <!--被继承的父项目的构件标识符 -->
        <artifactId />
        <!--被继承的父项目的全球唯一标识符 -->
        <groupId />
        <!--被继承的父项目的版本 -->
        <version />
        <!-- 父项目的pom.xml文件的相对路径。相对路径允许你选择一个不同的路径。默认值是../pom.xml。Maven首先在构建当前项目的地方寻找父项 
            目的pom，其次在文件系统的这个位置（relativePath位置），然后在本地仓库，最后在远程仓库寻找父项目的pom。 -->
        <relativePath />
    </parent>
    <!--声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。 -->
    <modelVersion>4.0.0</modelVersion>
    <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 如cn.qatools.app生成的相对路径为：/cn/qatools/app -->
    <groupId>cn.qatools</groupId>
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有-chat两个不同的项目拥有同样的artifact ID和groupID；在某个 
        特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven为项目产生的构件包括：JARs，源 码，二进制发布和WARs等。 -->
    <artifactId>manven-chat</artifactId>
    <!--项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型 -->
    <packaging>jar</packaging>
    <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 -->
    <version>1.0-SNAPSHOT</version>
    <!--项目的名称, Maven产生的文档用 -->
    <name>maven chat</name>
    <!--项目主页的URL, Maven产生的文档用 -->
    <url>http://qatools.cn/maven-chat</url>
    <!-- 项目的详细描述, Maven 产生的文档用。 当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，就可以包含HTML标 
        签）， 不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的索引页文件，而不是调整这里的文档。 -->
    <description>topic for maven</description>
    <!--描述了这个项目构建环境中的前提条件。 -->
    <prerequisites>
        <!--构建该项目或使用该插件所需要的Maven的最低版本 -->
        <maven />
    </prerequisites>
    <!--项目的问题管理系统(Bugzilla, Jira, Scarab,或任何你喜欢的问题管理系统)的名称和URL-->
    <issueManagement>
        <!--问题管理系统（例如jira）的名字， -->
        <system>agileone</system>
        <!--该项目使用的问题管理系统的URL -->
        <url>http:///agileone.cn/maven-caht</url>
    </issueManagement>
    <!--项目持续集成信息 -->
    <ciManagement>
        <!--持续集成系统的名字，例如continuum -->
        <system />
        <!--该项目使用的持续集成系统的URL（如果持续集成系统有web接口的话）。 -->
        <url />
        <!--构建完成时，需要通知的开发者/用户的配置项。包括被通知者信息和通知条件（错误，失败，成功，警告） -->
        <notifiers>
            <!--配置一种方式，当构建中断时，以该方式通知用户/开发者 -->
            <notifier>
                <!--传送通知的途径 -->
                <type />
                <!--发生错误时是否通知 -->
                <sendOnError />
                <!--构建失败时是否通知 -->
                <sendOnFailure />
                <!--构建成功时是否通知 -->
                <sendOnSuccess />
                <!--发生警告时是否通知 -->
                <sendOnWarning />
                <!--不赞成使用。通知发送到哪里 -->
                <address />
                <!--扩展配置项 -->
                <configuration />
            </notifier>
        </notifiers>
    </ciManagement>
    <!--项目创建年份，4位数字。当产生版权信息时需要使用这个值。 -->
    <inceptionYear />
    <!--项目相关邮件列表信息 -->
    <mailingLists>
        <!--该元素描述了项目相关的所有邮件列表。自动产生的网站引用这些信息。 -->
        <mailingList>
            <!--邮件的名称 -->
            <name>service</name>
            <!--发送邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <post>servcie@qatools.cn</post>
            <!--订阅邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <subscribe>service@qatools.cn</subscribe>
            <!--取消订阅邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <unsubscribe>admin@qatools.cn</unsubscribe>
            <!--你可以浏览邮件信息的URL -->
            <archive>http:/qatools.cn/maven-chat/archive/</archive>
        </mailingList>
    </mailingLists>
    <!--项目开发者列表 -->
    <developers>
        <!--某个项目开发者的信息 -->
        <developer>
            <!--SCM里项目开发者的唯一标识符 -->
            <id>qatools</id>
            <!--项目开发者的全名 -->
            <name>oak</name>
            <!--项目开发者的email -->
            <email>service@qatools.cn</email>
            <!--项目开发者的主页的URL -->
            <url />
            <!--项目开发者在项目中扮演的角色，角色元素描述了各种角色 -->
            <roles>
                <role>Project Manager</role>
                <role>Architect</role>
            </roles>
            <!--项目开发者所属组织 -->
            <organization>qatools</organization>
            <!--项目开发者所属组织的URL -->
            <organizationUrl>http://qatool.cn/aboutme</organizationUrl>
            <!--项目开发者属性，如即时消息如何处理等 -->
            <properties>
                <dept>No</dept>
            </properties>
            <!--项目开发者所在时区， -11到12范围内的整数。 -->
            <timezone>-5</timezone>
        </developer>
    </developers>
    <!--项目的其他贡献者列表 -->
    <contributors>
        <!--项目的其他贡献者。参见developers/developer元素 -->
        <contributor>
            <name />
            <email />
            <url />
            <organization />
            <organizationUrl />
            <roles />
            <timezone />
            <properties />
        </contributor>
    </contributors>
    <!--该元素描述了项目所有License列表。 应该只列出该项目的license列表，不要列出依赖项目的 license列表。如果列出多个license，用户可以选择它们中的一个而不是接受所有license。 -->
    <licenses>
        <!--描述了项目的license，用于生成项目的web站点的license页面，其他一些报表和validation也会用到该元素。 -->
        <license>
            <!--license用于法律上的名称 -->
            <name>Apache 2</name>
            <!--官方的license正文页面的URL -->
            <url>http://qatools.cn/chat/maven-caht/LICENSE-2.0.txt</url>
            <!--项目分发的主要方式： repo，可以从Maven库下载 manual， 用户必须手动下载和安装依赖 -->
            <distribution>repo</distribution>
            <!--关于license的补充信息 -->
            <comments>A business-friendly OSS license</comments>
        </license>
    </licenses>
    <!--SCM(Source Control Management)标签允许你配置你的代码库，供Maven web站点和其它插件使用。 -->
    <scm>
        <!--SCM的URL,该URL描述了版本库和如何连接到版本库。欲知详情，请看SCMs提供的URL格式和列表。该连接只读。 -->
        <connection>
            scm:git:https://github.com/qatools/maveninaction
        </connection>
        <!--给开发者使用的，类似connection元素。即该连接不仅仅只读 -->
        <developerConnection>
            scm:git:https://github.com/qatools/maveninaction
        </developerConnection>
        <!--当前代码的标签，在开发阶段默认为HEAD -->
        <tag />
        <!--指向项目的可浏览SCM库的URL。 -->
        <url>https://github.com/qatools/maveninaction</url>
    </scm>
    <!--描述项目所属组织的各种属性。Maven产生的文档用 -->
    <organization>
        <!--组织的全名 -->
        <name>qatools</name>
        <!--组织主页的URL -->
        <url>http://qatools.cn</url>
    </organization>
    <!--构建项目需要的信息 -->
    <build>
        <!--该元素设置了项目源码目录，当构建项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <sourceDirectory />
        <!--该元素设置了项目脚本源码目录，该目录和源码目录不同：绝大多数情况下，该目录下的内容 会被拷贝到输出目录(因为脚本是被解释的，而不是被编译的)。 -->
        <scriptSourceDirectory />
        <!--该元素设置了项目单元测试使用的源码目录，当测试项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <testSourceDirectory />
        <!--被编译过的应用程序class文件存放的目录。 -->
        <outputDirectory />
        <!--被编译过的测试class文件存放的目录。 -->
        <testOutputDirectory />
        <!--使用来自该项目的一系列构建扩展 -->
        <extensions>
            <!--描述使用到的构建扩展。 -->
            <extension>
                <!--构建扩展的groupId -->
                <groupId />
                <!--构建扩展的artifactId -->
                <artifactId />
                <!--构建扩展的版本 -->
                <version />
            </extension>
        </extensions>
        <!--当项目没有规定目标（Maven2 叫做阶段）时的默认值 -->
        <defaultGoal />
        <!--这个元素描述了项目相关的所有资源路径列表，例如和项目相关的属性文件，这些资源被包含在最终的打包文件里。 -->
        <resources>
            <!--这个元素描述了项目相关或测试相关的所有资源路径 -->
            <resource>
                <!-- 描述了资源的目标路径。该路径相对target/classes目录（例如${project.build.outputDirectory}）。举个例 
                    子，如果你想资源在特定的包里(org.apache.maven.messages)，你就必须该元素设置为org/apache/maven /messages。然而，如果你只是想把资源放到源码目录结构里，就不需要该配置。 -->
                <targetPath />
                <!--是否使用参数值代替参数名。参数值取自properties元素或者文件里配置的属性，文件在filters元素里列出。 -->
                <filtering />
                <!--描述存放资源的目录，该路径相对POM路径 -->
                <directory />
                <!--包含的模式列表，例如**/*.xml. -->
                <includes />
                <!--排除的模式列表，例如**/*.xml -->
                <excludes />
            </resource>
        </resources>
        <!--这个元素描述了单元测试相关的所有资源路径，例如和单元测试相关的属性文件。 -->
        <testResources>
            <!--这个元素描述了测试相关的所有资源路径，参见build/resources/resource元素的说明 -->
            <testResource>
                <targetPath />
                <filtering />
                <directory />
                <includes />
                <excludes />
            </testResource>
        </testResources>
        <!--构建产生的所有文件存放的目录 -->
        <directory />
        <!--产生的构件的文件名，默认值是${artifactId}-${version}。 -->
        <finalName />
        <!--当filtering开关打开时，使用到的过滤器属性文件列表 -->
        <filters />
        <!--子项目可以引用的默认插件信息。该插件配置项直到被引用时才会被解析或绑定到生命周期。给定插件的任何本地配置都会覆盖这里的配置 -->
        <pluginManagement>
            <!--使用的插件列表 。 -->
            <plugins>
                <!--plugin元素包含描述插件所需要的信息。 -->
                <plugin>
                    <!--插件在仓库里的group ID -->
                    <groupId />
                    <!--插件在仓库里的artifact ID -->
                    <artifactId />
                    <!--被使用的插件的版本（或版本范围） -->
                    <version />
                    <!--是否从该插件下载Maven扩展（例如打包和类型处理器），由于性能原因，只有在真需要下载时，该元素才被设置成enabled。 -->
                    <extensions />
                    <!--在构建生命周期中执行一组目标的配置。每个目标可能有不同的配置。 -->
                    <executions>
                        <!--execution元素包含了插件执行需要的信息 -->
                        <execution>
                            <!--执行目标的标识符，用于标识构建过程中的目标，或者匹配继承过程中需要合并的执行目标 -->
                            <id />
                            <!--绑定了目标的构建生命周期阶段，如果省略，目标会被绑定到源数据里配置的默认阶段 -->
                            <phase />
                            <!--配置的执行目标 -->
                            <goals />
                            <!--配置是否被传播到子POM -->
                            <inherited />
                            <!--作为DOM对象的配置 -->
                            <configuration />
                        </execution>
                    </executions>
                    <!--项目引入插件所需要的额外依赖 -->
                    <dependencies>
                        <!--参见dependencies/dependency元素 -->
                        <dependency>
                            ......
                        </dependency>
                    </dependencies>
                    <!--任何配置是否被传播到子项目 -->
                    <inherited />
                    <!--作为DOM对象的配置 -->
                    <configuration />
                </plugin>
            </plugins>
        </pluginManagement>
        <!--使用的插件列表 -->
        <plugins>
            <!--参见build/pluginManagement/plugins/plugin元素 -->
            <plugin>
                <groupId />
                <artifactId />
                <version />
                <extensions />
                <executions>
                    <execution>
                        <id />
                        <phase />
                        <goals />
                        <inherited />
                        <configuration />
                    </execution>
                </executions>
                <dependencies>
                    <!--参见dependencies/dependency元素 -->
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
                <goals />
                <inherited />
                <configuration />
            </plugin>
        </plugins>
    </build>
    <!--在列的项目构建profile，如果被激活，会修改构建处理 -->
    <profiles>
        <!--根据环境参数或命令行参数激活某个构建处理 -->
        <profile>
            <!--构建配置的唯一标识符。即用于命令行激活，也用于在继承时合并具有相同标识符的profile。 -->
            <id />
            <!--自动触发profile的条件逻辑。Activation是profile的开启钥匙。profile的力量来自于它 能够在某些特定的环境中自动使用某些特定的值；这些环境通过activation元素指定。activation元素并不是激活profile的唯一方式。 -->
            <activation>
                <!--profile默认是否激活的标志 -->
                <activeByDefault />
                <!--当匹配的jdk被检测到，profile被激活。例如，1.8激活JDK1.8，1.8.0_2，而!1.8激活所有版本不是以1.8开头的JDK。 -->
                <jdk />
                <!--当匹配的操作系统属性被检测到，profile被激活。os元素可以定义一些操作系统相关的属性。 -->
                <os>
                    <!--激活profile的操作系统的名字 -->
                    <name>Windows 10</name>
                    <!--激活profile的操作系统所属家族(如 'windows') -->
                    <family>Windows</family>
                    <!--激活profile的操作系统体系结构 -->
                    <arch>x86</arch>
                    <!--激活profile的操作系统版本 -->
                    <version>10.0.18362.388</version>
                </os>
                <!--如果Maven检测到某一个属性（其值可以在POM中通过${名称}引用），其拥有对应的名称和值，Profile就会被激活。如果值 字段是空的，那么存在属性名称字段就会激活profile，否则按区分大小写方式匹配属性值字段 -->
                <property>
                    <!--激活profile的属性的名称 -->
                    <name>mavenVersion</name>
                    <!--激活profile的属性的值 -->
                    <value>3.5.0</value>
                </property>
                <!--提供一个文件名，通过检测该文件的存在或不存在来激活profile。missing检查文件是否存在，如果不存在则激活 profile。另一方面，exists则会检查文件是否存在，如果存在则激活profile。 -->
                <file>
                    <!--如果指定的文件存在，则激活profile。 -->
                    <exists>
                    /usr/local/jenkins/jenkins-home/jobs/maven-chat/workspace/
                    </exists>
                    <!--如果指定的文件不存在，则激活profile。 -->
                    <missing>
                    /usr/local/jenkins/jenkins-home/jobs/maven-chat/workspace/
                    </missing>
                </file>
            </activation>
            <!--构建项目所需要的信息。参见build元素 -->
            <build>
                <defaultGoal />
                <resources>
                    <resource>
                        <targetPath />
                        <filtering />
                        <directory />
                        <includes />
                        <excludes />
                    </resource>
                </resources>
                <testResources>
                    <testResource>
                        <targetPath />
                        <filtering />
                        <directory />
                        <includes />
                        <excludes />
                    </testResource>
                </testResources>
                <directory />
                <finalName />
                <filters />
                <pluginManagement>
                    <plugins>
                        <!--参见build/pluginManagement/plugins/plugin元素 -->
                        <plugin>
                            <groupId />
                            <artifactId />
                            <version />
                            <extensions />
                            <executions>
                                <execution>
                                    <id />
                                    <phase />
                                    <goals />
                                    <inherited />
                                    <configuration />
                                </execution>
                            </executions>
                            <dependencies>
                                <!--参见dependencies/dependency元素 -->
                                <dependency>
                                    ......
                                </dependency>
                            </dependencies>
                            <goals />
                            <inherited />
                            <configuration />
                        </plugin>
                    </plugins>
                </pluginManagement>
                <plugins>
                    <!--参见build/pluginManagement/plugins/plugin元素 -->
                    <plugin>
                        <groupId />
                        <artifactId />
                        <version />
                        <extensions />
                        <executions>
                            <execution>
                                <id />
                                <phase />
                                <goals />
                                <inherited />
                                <configuration />
                            </execution>
                        </executions>
                        <dependencies>
                            <!--参见dependencies/dependency元素 -->
                            <dependency>
                                ......
                            </dependency>
                        </dependencies>
                        <goals />
                        <inherited />
                        <configuration />
                    </plugin>
                </plugins>
            </build>
            <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
            <modules />
            <!--发现依赖和扩展的远程仓库列表。 -->
            <repositories>
                <!--参见repositories/repository元素 -->
                <repository>
                    <releases>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </releases>
                    <snapshots>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </snapshots>
                    <id />
                    <name />
                    <url />
                    <layout />
                </repository>
            </repositories>
            <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
            <pluginRepositories>
                <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
                <pluginRepository>
                    <releases>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </releases>
                    <snapshots>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </snapshots>
                    <id />
                    <name />
                    <url />
                    <layout />
                </pluginRepository>
            </pluginRepositories>
            <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
            <dependencies>
                <!--参见dependencies/dependency元素 -->
                <dependency>
                    ......
                </dependency>
            </dependencies>
            <!--不赞成使用. 现在Maven忽略该元素. -->
            <reports />
            <!--该元素包括使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。参见reporting元素 -->
            <reporting>
                ......
            </reporting>
            <!--参见dependencyManagement元素 -->
            <dependencyManagement>
                <dependencies>
                    <!--参见dependencies/dependency元素 -->
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
            </dependencyManagement>
            <!--参见distributionManagement元素 -->
            <distributionManagement>
                ......
            </distributionManagement>
            <!--参见properties元素 -->
            <properties />
        </profile>
    </profiles>
    <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
    <modules />
    <!--发现依赖和扩展的远程仓库列表。 -->
    <repositories>
        <!--包含需要连接到远程仓库的信息 -->
        <repository>
            <!--如何处理远程仓库里发布版本的下载 -->
            <releases>
                <!--true或者false表示该仓库是否为下载某种类型构件（发布版，快照版）开启。 -->
                <enabled />
                <!--该元素指定更新发生的频率。Maven会比较本地POM和远程POM的时间戳。这里的选项是：always（一直），daily（默认，每日），interval：X（这里X是以分钟为单位的时间间隔），或者never（从不）。 -->
                <updatePolicy />
                <!--当Maven验证构件校验文件失败时该怎么做：ignore（忽略），fail（失败），或者warn（警告）。 -->
                <checksumPolicy />
            </releases>
            <!-- 如何处理远程仓库里快照版本的下载。有了releases和snapshots这两组配置，POM就可以在每个单独的仓库中，为每种类型的构件采取不同的 
                策略。例如，可能有人会决定只为开发目的开启对快照版本下载的支持。参见repositories/repository/releases元素 -->
            <snapshots>
                <enabled />
                <updatePolicy />
                <checksumPolicy />
            </snapshots>
            <!--远程仓库唯一标识符。可以用来匹配在settings.xml文件里配置的远程仓库 -->
            <id>qatools-group</id>
            <!--远程仓库名称 -->
            <name>qatools-group</name>
            <!--远程仓库URL，按protocol://hostname/path形式 -->
            <url>  http://192.168.128.129:8081/repository/qatools-group/</url>
            <!-- 用于定位和排序构件的仓库布局类型-可以是default（默认）或者legacy（遗留）。Maven 2为其仓库提供了一个默认的布局；然 
                而，Maven 1.x有一种不同的布局。我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。 -->
            <layout>default</layout>
        </repository>
    </repositories>
    <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
    <pluginRepositories>
        <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
        <pluginRepository>
            ......
        </pluginRepository>
    </pluginRepositories>


    <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
    <dependencies>
        <dependency>
            <!--依赖的group ID -->
            <groupId>org.apache.maven</groupId>
            <!--依赖的artifact ID -->
            <artifactId>maven-artifact</artifactId>
            <!--依赖的版本号。 在Maven 2里, 也可以配置成版本号的范围。 -->
            <version>3.8.1</version>
            <!-- 依赖类型，默认类型是jar。它通常表示依赖的文件的扩展名，但也有例外。一个类型可以被映射成另外一个扩展名或分类器。类型经常和使用的打包方式对应， 
                尽管这也有例外。一些类型的例子：jar，war，ejb-client和test-jar。如果设置extensions为 true，就可以在 plugin里定义新的类型。所以前面的类型的例子不完整。 -->
            <type>jar</type>
            <!-- 依赖的分类器。分类器可以区分属于同一个POM，但不同构建方式的构件。分类器名被附加到文件名的版本号后面。例如，如果你想要构建两个单独的构件成 
                JAR，一个使用Java 1.4编译器，另一个使用Java 6编译器，你就可以使用分类器来生成两个单独的JAR构件。 -->
            <classifier></classifier>
            <!--依赖范围。在项目发布过程中，帮助决定哪些构件被包括进来。欲知详情请参考依赖机制。 - compile ：默认范围，用于编译 - provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath 
                - runtime: 在执行时需要使用 - test: 用于test任务时使用 - system: 需要外在提供相应的元素。通过systemPath来取得 
                - systemPath: 仅用于范围为system。提供相应的路径 - optional: 当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用 -->
            <scope>test</scope>
            <!--仅供system范围使用。注意，不鼓励使用这个元素，并且在新的版本中该元素可能被覆盖掉。该元素为依赖规定了文件系统上的路径。需要绝对路径而不是相对路径。推荐使用属性匹配绝对路径，例如${java.home}。 -->
            <systemPath></systemPath>
            <!--当计算传递依赖时， 从依赖构件列表里，列出被排除的依赖构件集。即告诉maven你只依赖指定的项目，不依赖项目的依赖。此元素主要用于解决版本冲突问题 -->
            <exclusions>
                <exclusion>
                    <artifactId>spring-core</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
            </exclusions>
            <!--可选依赖，如果你在项目B中把C依赖声明为可选，你就需要在依赖于B的项目（例如项目A）中显式的引用对C的依赖。可选依赖阻断依赖的传递性。 -->
            <optional>true</optional>
        </dependency>
    </dependencies>
    <!--不赞成使用. 现在Maven忽略该元素. -->
    <reports></reports>
    <!--该元素描述使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。 -->
    <reporting>
        <!--true，则，网站不包括默认的报表。这包括"项目信息"菜单中的报表。 -->
        <excludeDefaults />
        <!--所有产生的报表存放到哪里。默认值是${project.build.directory}/site。 -->
        <outputDirectory />
        <!--使用的报表插件和他们的配置。 -->
        <plugins>
            <!--plugin元素包含描述报表插件需要的信息 -->
            <plugin>
                <!--报表插件在仓库里的group ID -->
                <groupId />
                <!--报表插件在仓库里的artifact ID -->
                <artifactId />
                <!--被使用的报表插件的版本（或版本范围） -->
                <version />
                <!--任何配置是否被传播到子项目 -->
                <inherited />
                <!--报表插件的配置 -->
                <configuration />
                <!--一组报表的多重规范，每个规范可能有不同的配置。一个规范（报表集）对应一个执行目标 。例如，有1，2，3，4，5，6，7，8，9个报表。1，2，5构成A报表集，对应一个执行目标。2，5，8构成B报表集，对应另一个执行目标 -->
                <reportSets>
                    <!--表示报表的一个集合，以及产生该集合的配置 -->
                    <reportSet>
                        <!--报表集合的唯一标识符，POM继承时用到 -->
                        <id />
                        <!--产生报表集合时，被使用的报表的配置 -->
                        <configuration />
                        <!--配置是否被继承到子POMs -->
                        <inherited />
                        <!--这个集合里使用到哪些报表 -->
                        <reports />
                    </reportSet>
                </reportSets>
            </plugin>
        </plugins>
    </reporting>
    <!-- 继承自该项目的所有子项目的默认依赖信息。这部分的依赖信息不会被立即解析,而是当子项目声明一个依赖（必须描述group ID和 artifact 
        ID信息），如果group ID和artifact ID以外的一些信息没有描述，则通过group ID和artifact ID 匹配到这里的依赖，并使用这里的依赖信息。 -->
    <dependencyManagement>
        <dependencies>
            <!--参见dependencies/dependency元素 -->
            <dependency>
                ......
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!--项目分发信息，在执行mvn deploy后表示要发布的位置。有了这些信息就可以把网站部署到远程服务器或者把构件部署到远程仓库。 -->
    <distributionManagement>
        <!--部署项目产生的构件到远程仓库需要的信息 -->
        <repository>
            <!--是分配给快照一个唯一的版本号（由时间戳和构建流水号）？还是每次都使用相同的版本号？参见repositories/repository元素 -->
            <uniqueVersion />
            <id>maven-chat</id>
            <name>maven-chat</name>
            <url>file://${basedir}/target/deploy</url>
            <layout />
        </repository>
        <!--构件的快照部署到哪里？如果没有配置该元素，默认部署到repository元素配置的仓库，参见distributionManagement/repository元素 -->
        <snapshotRepository>
            <uniqueVersion />
            <id>maven-chat</id>
            <name>maven-chat Snapshot Repository</name>
            <url>scp://192.168.128.129:/usr/local/maven-chat-snapshot</url>
            <layout />
        </snapshotRepository>
        <!--部署项目的网站需要的信息 -->
        <site>
            <!--部署位置的唯一标识符，用来匹配站点和settings.xml文件里的配置 -->
            <id>maven-chat-site</id>
            <!--部署位置的名称 -->
            <name>maven-chat website</name>
            <!--部署位置的URL，按protocol://hostname/path形式 -->
            <url>
                scp://192.168.128.130:/usr/local/tomcat/deploy
            </url>
        </site>
        <!--项目下载页面的URL。如果没有该元素，用户应该参考主页。使用该元素的原因是：帮助定位那些不在仓库里的构件（由于license限制）。 -->
        <downloadUrl />
        <!--如果构件有了新的group ID和artifact ID（构件移到了新的位置），这里列出构件的重定位信息。 -->
        <relocation>
            <!--构件新的group ID -->
            <groupId />
            <!--构件新的artifact ID -->
            <artifactId />
            <!--构件新的版本号 -->
            <version />
            <!--显示给用户的，关于移动的额外信息，例如原因。 -->
            <message />
        </relocation>
        <!-- 给出该构件在远程仓库的状态。不得在本地项目中设置该元素，因为这是工具自动更新的。有效的值有：none（默认），converted（仓库管理员从 
            Maven 1 POM转换过来），partner（直接从伙伴Maven 2仓库同步过来），deployed（从Maven 2实例部 署），verified（被核实时正确的和最终的）。 -->
        <status />
    </distributionManagement>
    <!--以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件（见settings.xml配置文件里activation元素的说明）。格式是<name>value</name>。 -->
    <properties />
</project>
```

### Maven 构建生命周期

![在这里插入图片描述](https://images.gitbook.cn/04ea3720-ea5c-11e9-b809-1fa647d53274)

构建生命周期解释如下：

| 构建阶段 | 中文称呼 | 描述                                          |
| :------- | :------- | :-------------------------------------------- |
| validate | 验证项目 | 验证项目是否正确并且所有必须信息是可用的      |
| compile  | 执行编译 | 源代码编译                                    |
| test     | 测试     | 使用适当的单元测试框架（例如JUnit）运行测试。 |
| package  | 打包     | 打包生成 jar 或 war 包                        |
| verify   | 检查     | 对集成测试的结果进行检查                      |
| install  | 安装     | 安装打包的项目到本地仓库                      |
| deploy   | 部署     | 拷贝最终的工程包到远程仓库中                  |

命令执行方式：

```bash
mvn validate

mvn compile

mvn test

mvn package

mvn verify

mvn install

deploy
```

Maven 有以下三个标准的生命周期。

**clean**

项目清理的处理，移除所有上一次构建生成的文件。

```bash
 mvn clean 

 mvn clean compile

 mvn celan package

 mvn clean test

 mvn clean verify
```

**default（或 build）**

这是 Maven 的主要生命周期，被用于构建应用。

**site**

项目站点文档创建的处理，Maven Site 插件一般用来创建新的报告文档、部署站点。

```bash
mvn site
```

### Maven 仓库

#### 本地仓库

Maven 的本地仓库在安装 Maven 后并不会创建，它是在第一次执行 Maven 命令的时候才被创建。

运行 Maven 的时候，Maven 所需要的任何构件都是直接从本地仓库获取的。如果本地仓库没有，它会首先尝试从远程仓库下载构件至本地仓库，然后再使用本地仓库的构件。

默认情况下，不管 Linux 还是 Windows，每个用户在自己的用户目录下都有一个路径名为 .m2/respository/ 的仓库目录。Maven 本地仓库默认被创建在 %USER_HOME% 目录下。要修改默认位置，在 %M2_HOME%\conf 目录中的 Maven 的 settings.xml 文件中定义，也可以指定自定义的 settings.xml 文件。

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
   http://maven.apache.org/xsd/settings-1.0.0.xsd">
      <localRepository>/MavenRepo/repo</localRepository>
</settings>
```

#### 中央仓库

Maven 中央仓库是由 Maven 社区提供的仓库，其中包含了大量常用的库。中央仓库包含了绝大多数流行的开源 Java 构件，以及源码、作者信息、SCM、信息、许可证信息等。一般来说，简单的 Java 项目依赖的构件都可以在这里下载到。从中央仓库下载依赖包不需要额外配置，默认就可以使用，但需要能访问因特网。

浏览社区仓库：

```http
https://mvnrepository.com
```

或

```http
https://search.maven.org/#browse
```

#### 远程仓库

如果 Maven 在本地仓库和中央仓库中也找不到依赖的文件，它会尝试从远处仓库中下载所需的依赖包。

阿里云仓库 settings.xml 配置如下：

```xml
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
</mirrors>
```

### 企业私服仓库

Nexus Repository Manager 是比较流行的私库管理软件。它不但可以管理私库的依赖包，还可以作为远程仓库的代理，当它在私库中查找不到指定的依赖包时，Nexus Repository Manager 会尝试从远程仓库中对应的依赖包到私库中。

#### Nexus 安装

**下载并解压**

Nexus OSS 下载地址：

| 操作系统   | 下载地址                                                     |
| :--------- | :----------------------------------------------------------- |
| Unix/Linux | https://download.sonatype.com/nexus/3/latest-unix.tar.gz（[ASC](https://download.sonatype.com/nexus/3/latest-unix.tar.gz.asc)、[MD5](https://download.sonatype.com/nexus/3/latest-unix.tar.gz.md5)、[SHA1](https://download.sonatype.com/nexus/3/latest-unix.tar.gz.sha1)） |
| Windows    | https://download.sonatype.com/nexus/3/latest-win64.zip（[ASC](https://download.sonatype.com/nexus/3/latest-win64.zip.asc)、[MD5](https://download.sonatype.com/nexus/3/latest-win64.zip.md5)、[SHA1](https://download.sonatype.com/nexus/3/latest-win64.zip.sha1)） |
| OSX        | https://download.sonatype.com/nexus/3/latest-mac.tgz（[ASC](https://download.sonatype.com/nexus/3/latest-mac.tgz.asc)、[MD5](https://download.sonatype.com/nexus/3/latest-mac.tgz.md5)、[SHA1](https://download.sonatype.com/nexus/3/latest-mac.tgz.sha1)） |

下载后解压文件：

```
tar xzvf latest-unix.tar.gz 
```

**启动 Nexus 服务**

当前工作目录切换到 Nexus 的 bin 目录下，或者把 Nexus 的 bin 目录设置到操作系统的环境变量中。

切换工作目录并启动 Nuxus：

```bash
  cd /NEXUS_FOLDER/nexus/bin
  ./nexus start
```

也可以设置操作系统的环境变量：

```bash
sudo vim /etc/profile
export NEXUS_HOME=/usr/local/nexus
export PATH=${PATH}:${NEXUS_HOME}/bin

source /etc/profile

nexus start
```

#### 创建企业私服仓库

企业私服仓库应该创建 3 种类型的仓库：宿主仓库（hosted ）、代理仓库（proxy）、仓库组（group ）。

**hosted 宿主仓库**

用来存储和管理企业自己的 jar 包和版本。创建 hosted 宿主仓库只需要填写宿主仓库名称以及选择宿主仓库存储的制品类型。制品类型可以选 Snapshot 、Release 等。SnapShot 表示处于开发中的不稳定版，Release 表示正式发布的稳定版。

![在这里插入图片描述](https://images.gitbook.cn/72e56030-e9dd-11e9-8092-c12b22bd7cce)

**proxy 代理仓库**

代理访问远程仓库，例如阿里云 Maven 仓库。当宿主仓库中不能找到 Maven 构建过程需要的 jar 包时，将尝试从代理的远程仓库获取，并缓存到代理仓库中。

创建 proxy 代理仓库需要填写仓库名称和代理的远程仓库的 URL。

![在这里插入图片描述](https://images.gitbook.cn/937085a0-e9dd-11e9-af43-257c05e1d950)

**group 仓库组**

把宿主仓库和代理仓库设置到仓库组中，Maven 可以自动从宿主仓库和代理仓库中查找需要的依赖 jar 包并下载到本地构建。

创建 group 仓库组需要填写仓库组名称，以及选择已创建的 hosted 宿主仓库和 proxy 代理仓库。

![在这里插入图片描述](https://images.gitbook.cn/bbe23880-e9dd-11e9-9959-59f960f3590d)

创建的 group 仓库组的地址为：

```http
  http://192.168.128.129:8081/repository/qatools-group/
```

#### Maven 使用企业私服仓库

Maven 使用企业私服仓库需要在 settings.xml 文件中做如下配置：

```xml
<mirrors>
    <mirror>
        <id>central</id>
        <mirrorOf>*</mirrorOf> <!-- * 表示让所有仓库使用该镜像--> 
        <name>central-mirror</name> 
        <url>http://192.168.128.129:8081/repository/qatools-group/</url>
        </mirror> 
</mirrors>
```

### 持续集成与持续部署

我们使用 Jenkinsfile 实现 Maven 项目的持续集成和部署。

#### Jenkins 安装 pipeline 插件

Jenkinsfile 需要 pipeline 插件支持, 可以到 Jenkins 插件管理中查看 Pipeline 插件，如果没有安装可以在“可选插件标签”页查询相关插件进行安装。

![在这里插入图片描述](https://images.gitbook.cn/f47fef20-e9dd-11e9-928f-d9384dde7caf)

#### Jenkinsfile 实现持续集成

Jenkinsfile 中构建是一个场景 Stage ，我们通过定义一个 build 场景来构建从版本控制服务器中下载到 Jenkins 工作目录中的 Maven 项目源代码。

**构建打包 Maven 项目**

切换到 Jenkins 的工作目录：

```bash
   cd $workspace/base
   mvn clean package
```

**部署 war 包到 Tomcat 服务器**

部署过程需要关闭 Tomcat 服务，上传 war 包到 Tomcat 工作目录，然后重新启动 Tomcat 服务。

```bash
   stage 'stop_tomcat'
    sh """sh "sshpass  -p'$passwd' ssh -o StrictHostKeyChecking=no '$user'@'$host' 'bash  /usr/local/tomcat-test/bin/shutdown.sh' 
      """

    stage 'upload'
    sh """
        cd $workspace/'$war_name'/target
        sshpass -p'$passwd' scp -o StrictHostKeyChecking=no '$war_name'.war '$user'@'$host':'$tomcat_home'/deploy/'$war_name'.war
    """

    stage 'restart_tomcat'
    sh "sshpass  -p'$passwd' ssh -o StrictHostKeyChecking=no '$user'@'$host' 'bash  /usr/local/tomcat-test/bin/startup.sh'"
```

完整的 Jenkinsfile：

```bash
node () {
def workspace = pwd()
war_name='qatools.war'
tomcat_home='/usr/local/tomcat-test'
host='192.168.128.130'
user='root'
passwd='123456'

  stage 'checkout'
    dir('base'){
     git branch: 'dev', credentialsId: 'git-qatoolsweb', url: 'http://gitlab-scrum/agile/qatoolsweb.git'

     }


    stage 'Build'


    sh"""

        cd $workspace/base
        mvn clean package

     """

      stage 'backup'

    sh """
　　　　cd $workspace/'$war_name'/target
　　　　tar cfz /root/jenkins/jenkins_work_backup/Web`date +%y%m%d-%s`-test.tar.gz '$war_name'.war

    """

    stage 'delete_old'
    sh """sshpass  -p'$passwd' ssh -o StrictHostKeyChecking=no '$user'@'$host' "rm -rf  '$tomcat_home'/deploy/'$war_name'* "
    """

     stage 'stop_tomcat'
    sh """sh "sshpass  -p'$passwd' ssh -o StrictHostKeyChecking=no '$user'@'$host' 'bash  /usr/local/tomcat-test/bin/shutdown.sh' 
      """

    stage 'upload'
    sh """
        cd $workspace/'$war_name'/target
        sshpass -p'$passwd' scp -o StrictHostKeyChecking=no '$war_name'.war '$user'@'$host':'$tomcat_home'/deploy/'$war_name'.war
    """

    stage 'restart_tomcat'
    sh "sshpass  -p'$passwd' ssh -o StrictHostKeyChecking=no '$user'@'$host' 'bash  /usr/local/tomcat-test/bin/startup.sh'"

}
```

### 结束语

使用 Maven 时，setting.xml 和 pom.xml 常常是在模板的基础上修改为自己需要的配置，特别是依赖管理需要自己添加。构建命令 `mvn compile` 、 `mvn package` 、`mvn install` 、`mvn verify` 、 `mvn test` 是常用的几个。持续集成和部署的 Jenkinsfile 文件，一般情况一个团队或项目组一个就够用，不需要团队的每个人都要掌握，团队中有 1~2 个人掌握就能满足使用需求。