

# 注意

maven课程最后有个小demo，里面有公司编程习惯模式。

# maven篇

#### 手动maven工程

 1   mvn archetype:gernerate  创建一个mvn工程，arrchetype是插件名字，调用插件，gernerate是目标

执行后，后面有个选择默认是 7，快速的archetype



2 用maven工程操作，必须在pom所在目录下，不然执行报错，可以多个指令一起写比入mvn clean  install 



mvn compile java文件编译，放到target中

mvn clean 清除target目录

mvn test  执行test下的所有@test注解方法

mvn package  打成jar包，并不会把test代码打包进去，好像只有test全通过了，才能打包

mvn install  将本项目的打包的jar包，存入本地仓库中





#### junit结构

 1   maven结构，一般在main中创建了一些代码，在test中有个对应的测试代码。这样后续查看测试代码的时候条理清晰，不然我们在main里自测，方法多了之后就很冗长



#### 依赖scope的作用范围

scope标签有几种 compile  ,test ,provided,system,runtime,import 



|          | main目录 | test目录 | 开发时 | 部署时 |
| -------- | -------- | -------- | ------ | ------ |
| test     | 不能     | 能用     | 能用   | 不能   |
| compile  | 能用     | 能用     | 能用   | 能用   |
| provided | 能用     | 能用     | 能用   | 不能   |

对有的有服务器有的jar包，用不打包进去，不然有时候可能出现版本对不上，jar包冲突。这种的用provided标签修饰。

一般从maven仓库下载的时候，scope都是帮你写好的，不要乱改就行



#### jar包冲突原因

版本问题
同一个jar引入了两个版本A和B, 构建时取了其中一个版本A, 这时候可能会冲突:

```
如果A是低版本, 代码里用了高版本B的代码, 调用时就会报上述的错
如果A是高版本, 但没有兼容B版本, 代码里调用了B版本独有的代码, 也会报错，
比如一个项目的多个mudule，引入了2次jar，a module 用的a包写的代码没报错， b module用的b包写的代码也没报错，
但是构建的时候，只会选用其中一个版本。
```



- 包命名问题
  两个不同的jar, 因为命名不规范定义了相同的全路径Class, 构建时没问题两个jar都被构建, 但调用时有可能会报错, 之所以说有可能还与具体的机器相关

```
如果机器先加载A包, 调用A包同路径Class的代码就不会报错
如果机器先加载A包, 调用B包同路径Class的代码就会报错
如果机器先加载A包, 调用B包同路径Class的代码, 但A包也有相同方法签名的方法, 此时不会报错, 但是调用逻辑可能是非预期但很难发现
```

#### 依赖传递性

a 依赖 b  ，b 依赖 c ，那么a 能不能直接调用c的方法。 如果b对c的依赖是copile范围，那么能传递，如果是test和provided那么不能传递给a

#### 依赖排除

a 依赖b ,a 依赖c，  b,c各自依赖 用不同版本的d 包，那么就在bc中选一个，排除掉一个d包

```
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <!-- 当前依赖的范围，也就是说junit这个包就是test用的  -->
      <scope>test</scope>
      
      <exclusions>
            <groupId>aaaa</groupId>
      			<artifactId>bbbb</artifactId>
      			      <version>4.12</version>
      <!--排除的不需要写版本-->
      </exclusion>
    </dependency>
```

#### 依赖继承

父工程的依赖，在子工程可以继承版本号。  子工程有些依赖可以不写版本号，如果这样，在父工程或者爷爷工程中一定要一个写版本号

这样方便版本管理，但是有个问题就是，不同工程单独拆下来编码测试的时候，需要自己写，然后再删除吗？



#### 创建父工程

 加入<packaging>pom</packaging>

```
  <groupId>com.cbk.timor</groupId>
  <artifactId>learning_maven_origin</artifactId>
  <version>1.0</version>
  
  <!--只有写了打包方式是pom的工程才能作为父工程-->
  <packaging>pom</packaging>
  
     <!--创建子工程之后，mvn会自动帮你加入moules-->
      <modules>
        <module>son1</module>
        <module>son2</module>
    </modules>
    
    
       <properties>
   		<!-- 构架过程中读取源码使用的字符集 -->
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    	</properties>
     <!--  父工程中，要加入dependencyManagement的标签 -->
    <dependencyManagement>
    <dependencies>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

    </dependencyManagement>
```





#### 创建子工程

在父工程的根目录执行mvn archetype:gernerate  ，就可以创建子工程 ，子工程会出现下面的标签。

并且一般子工程和父工程的本项目的groupid和version都是一致的，所以子工程自己的version和grouplid不用谢，

这里是指com.cbk.timor。子工程还是需要写要哪些依赖，不过不用写版本。如果非要写版本，则覆盖父工程版本

```
   <parent>
        <artifactId>learning_maven_origin</artifactId>
        <groupId>com.cbk.timor</groupId>
        <version>1.0</version>
    </parent>
```



#### 版本号用变量代替

这样只改一个多处都能改,在propertites标签里，我们可以自定义属性

```
    
   <properties>
    	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    	<!--  自定义变量 -->
    	<timor.verson>4.1.2</timor.version>
  </properties>
    
    <dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <!-- 使用自定义变量 -->
            <version>${timor.version}</version>
        </dependency>
    </dependencies>

    </dependencyManagement>
```



#### maven版本积累

就是一套架构，需要哪些依赖，有的依赖需要另外一个依赖多少版本以上，这种能试用的一套maven版本，都是不容易的，可以记录保存下来，以后直接用。



## 标签解读

#### properties

自定义声明一些变量

```xml
    <!-- 在properties里声明变量  -->
	<properties>
        <!--  写好flink版本，后续变更不需要每个都改，只改properties的属性    -->
        <flink.verson>1.13.0</flink.verson>
   </properties>

	
	<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-java</artifactId>
    <version>${flink.verson}</version>
    <scope>provided</scope>
	</dependency>


```

#### scope

用在依赖最下方，表明依赖作用范围，是否参与打包发布等等

```xml
<!--  -->
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-java</artifactId>
    <version>${flink.verson}</version>
    <scope>provided</scope>
</dependency>

<!-- 
compile
默认的scope，表示 dependency 都可以在生命周期中使用。而且，这些dependencies 会传递到依赖的项目中。适用于所有阶段，会随着项目一起发布。
provided
跟compile相似，但是表明了dependency 由JDK或者容器提供，例如Servlet AP和一些Java EE APIs。这个scope 只能作用在编译和测试时，同时没有传递性。
runtime
表示dependency不作用在编译时，但会作用在运行和测试时，如JDBC驱动，适用运行和测试阶段。
test
表示dependency作用在测试时，不作用在运行时。 只在测试时使用，用于编译和运行测试代码。不会随项目发布。
-->

```



# idea对接maven

#### 替换maven源

idea读取maven仓库和配置，默认在/Users/timor/.m2 目录下的，repository是仓库，settings.xml是配置文件。

我已经把之前的名字改过一次了，如果找不到会默认新建2个新库和配置文件

```
<localRepository>/Users/timor/Desktop/coding_software/apache-maven-3.9.0/timor_maven_repository</localRepository>

    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
```

#### 源码下载失败

pom导入jar依赖没问题，不过点进去之后，用idea 下载soure源码一直失败，在目录下的terminal执行 mvn dependency:sources



# 常用依赖pom

#### flink

```xml
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-java</artifactId>
            <version>1.13.0</version>
        </dependency>
        <!-- -->
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-streaming-java_2.12</artifactId>
            <version>1.13.0</version>
            <scope>provided</scope>
        </dependency>

       <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-clients_2.12</artifactId>
            <version>${flink.verson}</version>
        </dependency>
```

#### log4j相关



# 已解决的问题

1  maven打包时显示，1.5语法不支持静态接口的调用但是idea里面settings和project structure都改成8了，还是不行

最后是pom文件 添加制定maven编译器解决的

```
<properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

2 maven打包不包含相关依赖

pom文件加入这个，这个会把所有依赖都打包进去

```

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <!--给jar包起的别名-->
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <classpathPrefix>lib/</classpathPrefix>
                            <!--添加项目中主类-->
                            <mainClass>com.sdt.intf.client.tools.DealConnectFile</mainClass>
                        </manifest>
                    </archive>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
 
        </plugins>
        </build>

```





# 问题

1.对有服务器bin有的jar包，并且自己打包的时候不小心打包一个新的进去，执行时会加载哪个jar包，还是随机。

2.依赖排除有个问题，就是b,c同时依赖不同版本的d，选一个排除了，是不是b,c用留下的d，会不会不兼容

3父工程的依赖，在子工程中可以用。子工程有些依赖可以不写版本号，如果这样，在父工程或者爷爷工程中一定要一个写版本号

这样方便版本管理，但是有个问题就是，不同工程单独拆下来编码测试的时候，需要自己写，然后再删除吗？还是加上scope标签

