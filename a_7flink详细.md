# 个人理解

1 flink 的DataStreamSource 模型和MR的区别，当某个算子比如map过后有多个分支时，这个map算子可以重用，和spark相同

​	相比于MR这个优势

2 flink侧输出流概念可以用在spark中，spark 2个流关联的时候关联不到的，输出到侧输出流保存下来。



# 注意事项

1  connect后用process进行两流关联时，必须用keyby之后再关联，不然的话可能会出现同key分到不同流的情况。

​	比如tuple2( id ,  count ),id相同，但是整体tuple2是如何shuffle的不清楚，所以要手动keyby代码才能保证数据没问题。



# 问题待解决

1 

下面2个都是yarn-application模式，不过本地的jar能正常执行，但是hdfs上的jar无法正常执行，报错如下：

 No ClusterClientFactory found. If you were targeting a Yarn cluster, please make sure to export the HADOOP_CLASSPATH  environment variable or have hadoop in your classpath.

​	个人理解就是如果通过hdfs 必须在hdfs上有jar包并且必须参数指定lib才行，或者说是没有连接上project1:8020就是没找到hdfs所在的执行环境

```shell
./bin/flink run-application -t yarn-application -c com.timor.flink.learning.demo.A3_WordCountStreamSocket  hdfs://project1:8020/flink-jars/git_flink_learning_1.17-1.0-SNAPSHOT.jar


./bin/flink run-application -t yarn-application -c com.timor.flink.learning.demo.A3_WordCountStreamSocket   ./git_flink_learning_1.17-1.0-SNAPSHOT.jar
```

2任务槽设置中，是在flink配置文件设定的，但是flink on yarn，其他子节点都没安装flink，设置插槽数量有什么用？？

3 什么时候用batch批，什么时候用stream流模式

4flink的分区策略有什么用，大部分flink是消费消息队列kafka，而kafka的API应该是有自己的数据发送模式，flink设置了应该没用

5分流作用，flink分流的场景是什么，分流之后也有其他操作，不可以通过if判断来在一个代码里写吗，除非涉及到重用

6 为什么process和flatmap里面有collector 而 map等算子里面没有collector，flatmap好理解，process为什么有collector

7 3流以上的join怎么实现的，目前用connect+ process只能实现2流join



# 待做

去csdn查一下，session和standalone，application模式应用场景与区别







# Flink集群

## 客户端指令

启动：在project2中执行 bin/start-cluster.sh  

UI端口 8081

## 运行模式

#### standalone

standalone模式，就是资源调度用flink本身的功能，不调用外部的资源管理系统。standalone下 执行apllication mode。这时候只能自己手动启动执行节点，用几个自己启动几个，不像yarn会根据任务来自动分配启动几个节点执行任务。



#### yarn模式

yarn-application模式

启动指令：bin/flink run-application -t yarn-application -c com.atguigu.WordCount  FlinkTutorial-1.0-SNAPSHOT.jar 

优化：讲flink的lib和plugins目录上传到hdfs上的flink_disk，这样就不需要每次执行任务的时候，yarn讲lib和plugins临时上传	

​			到hdfs再由各节点去下载，因为生产环境不是每台节点都安装flink



```shell
bin/flink run-application -t yarn-application	-Dyarn.provided.lib.dirs="hdfs://project1:8020/flink-dist"	-c com.atguigu.wc.SocketStreamWordCount  hdfs://hadoop102:8020/flink-jars/FlinkTutorial-1.0-SNAPSHOT.jar


参数指定yarn的lib目录
-Dyarn.provided.lib.dirs="hdfs://project1:8020/flink-dist"

参数指定main方法的类
-c com.atguigu.wc.SocketStreamWordCount

参数指定jar路径
hdfs://hadoop102:8020/flink-jars/FlinkTutorial-1.0-SNAPSHOT.jar

```







通过--help 指令查看帮助，常用的有

```
 -at，——applicationType <arg>为YARN上的应用设置自定义应用类型 
-D <property=value>使用给定属性的值 
-d，——detached如果存在，则以分离模式运行作业 
-h，——help Yarn会话命令行帮助。 
-id，——applicationId <arg>附加到正在运行的YARN会话 
-j，——jar <arg> Flink jar文件的路径 
-jm，——jobManagerMemory <arg> JobManager容器内存，可选单位(默认:MB) 
-m，——jobmanager <arg>要连接的jobmanager (master)的地址。使用此标志连接到与配置中指定的JobManager不同的JobManager。 
-nl，——nodeLabel <arg>指定YARN应用的YARN节点标签 
-nm，——name <arg>设置YARN上应用程序的自定义名称 
-q，——query显示可用YARN资源(内存，内核) 
-qu，——queue <arg>指定YARN队列。 
-s，——slots <arg>每个TaskManager的插槽数 
-t，——ship <arg>发送指定目录下的文件(t用于传输) 
-tm，——taskManagerMemory <arg>每个TaskManager容器的内存，可选单位(默认:MB) 
-yd，——yarndetached如果存在，则以分离模式运行作业(已弃用;使用非特定于yarn的选项代替) 
-z，——Zookeeper Namespace <arg> Namespace用于创建高可用模式下的Zookeeper子路径
```



通过yarn控制资源





# Flink常用Maven

```xml
    <properties>
        <!--  写好flink版本，后续变更不需要每个都改，只改properties的属性    -->
        <flink.verson>1.17.0</flink.verson>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

 <dependencies>
   
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-streaming-java</artifactId>
            <version>${flink.verson}</version>
        </dependency>

        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-clients</artifactId>
            <version>${flink.verson}</version>
        </dependency>


    </dependencies>
```



maven打包插件的pom

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.2.4</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <artifactSet>
                            <excludes>
                                <exclude>com.google.code.findbugs:jsr305</exclude>
                                <exclude>org.slf4j:*</exclude>
                                <exclude>log4j:*</exclude>
                            </excludes>
                        </artifactSet>
                        <filters>
                            <filter>
                                <!-- Do not copy the signatures in the META-INF folder.
                                Otherwise, this might cause SecurityExceptions when using the JAR. -->
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                        <transformers combine.children="append">
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

```



# FlinkAPI

### 环境与算子

##### 环境获取

getExecutionEnvironment()会自动识别，当在idea执行时会默认创建本地模拟集群。用flink脚本执行jar时，会识别创建远程集群自动传递yarn等参数。

不建议在代码中指定流式批式，一套代码流批同用，用参数传递bin/flink run -Dexecution.runtime-mode=BATCH

```java
StreamExecutionEnvironment.getExecutionEnvironment(); //会自动识别是本地还是远程，通过传configure来改变
StreamExecutionEnvironment.createLocalEnvironment(); //开启一个本地模拟集群
StreamExecutionEnvironment.createRemoteEnvironment("host",8088,"jarPath"); //连接远程集群
env.setRuntimeMode(RuntimeExecutionMode.BATCH); //设置代码是流式还是批处理

```



##### 并行度

当env环境和算子同时设置并行度时，算子优先级高于env全局指定，其他没有设置并行度的算子，按env全局并行度来。

当用flink脚本执行时，临时指定 -p 3 指定并行度   优先级别是  算子指定> env指定 > 参数指定

idea执行单代码时，若算子不指定并行度默认和本机核数相同

```java
 //  这个是idea执行创建一个有webUI的en,登陆localhost:8081能看到flinkweb界面,开发时使用
StreamExecutionEnvironment env = StreamExecutionEnvironment.createLocalEnvironmentWithWebUI(newConfiguration());

//环境设置并行度
env.setParallelism(3);
```

##### 算子链

默认没有shuffle 重分区的算子，都可以合到一块执行，有时候可能需要算子不合并执行，比如很复杂的2个map任务，这样可能map之后，在第二个map分到别的节点，这样效率快

比如当有个算子链报错，你定位不到具体是哪里出问题，可以禁用算子链定位到哪个算子出错

```java
Ds.map(v -> 10*v).disableChaining() //这个map算子不会和后面的合在一起
.map(v -> 10*v)

 env.disableOperatorChaining()  //全局禁用算子链合并
.startNewChain() //重新开始算子链合并

```

##### 任务槽

在Flink的/opt/module/flink-1.17.0/conf/flink-conf.yaml配置文件中，可以设置TaskManager的slot数量，默认是1个slot。

taskmanager.numberOfTaskSlots: 8

需要注意的是，slot目前仅仅用来隔离内存，不会涉及CPU的隔离。在具体应用时，可以将slot数量配置为机器的CPU核心数，尽量避免不同任务之间对CPU的竞争。这也是开发环境默认并行度设为机器CPU数量的原因。



### DataStream/DataSet API核心

dataset写法是一个批处理方式，已经过时了。1.12以后已经用DataStream的API通过set batch参数就能用一个代码，同时可实现流处理和批处理

##### 自定义可序列化

Flink对POJO类型的要求如下：

l 类是公有（public）的

l 有一个无参的构造方法

l 所有属性都是公有（public）的

l 所有属性的类型都是可以序列化的

并且实现了searilized接口

##### 常用算子

基本算子map,filter,flatMap  聚合算子 keyBy,sum,min,max,minBy,maxBy,reduce

聚合算子sum,reduce等，必须在keyby之后,也就是KeyedStream类型才能调用这些

##### 分区策略

常见的物理分区策略有：随机分配（Random）、轮询分配（Round-Robin）、重缩放（Rescale）和广播（Broadcast）

分区策略是指同一个消息源，按什么方式发送给并行度,比如2个并行度，如果按random，运气不好可能全发给2线程

random策略通过 ds.shuffle()来调用

轮询策略通过ds.rebalance()来调用

广播策略通过ds.broadcast()调用，所有的消息发送给所有并行度，重复数据



##### 分流合流

union省略过

connect 如果用process进行

##### 窗口

窗口如果不用keyby的话，则无论怎么设置并行度，强制变为1

用keyby的话，才有并行度







### ProcessFunction底层API

### Table API

### SQL

# Flink实时数仓

### 架构模型



### 数仓模型规划





### 个人理解与问题提出

未学之前个人理解，将商品表这种加载到内存中，然后对于实时变化的表，新增订单，新增用户这些通过抓取新增量，然后去关联其他新增的附属表，再去关联商品表这种已经加载到内存的。

对于新增的商品，也要抓去改动。



第二最后小文件问题，最后flink输出的ads数据肯定不存在hdfs上，因为实时指标ads是一直变化的，hdfs不支持变化可以查看数据也很慢，最后应该是导入到一个大数据数据库中，比如hbase



##### 问题需解决

就是通过mysql采集的数据，经处理之后，但是突然订单的完成状态修改了，从下单，到付款了，但是下单状态已经处理过了，这种修改操做后续怎么处理。

