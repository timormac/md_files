 

# 虚拟机集群

#### 虚拟机集群相关：

服务器用户:lpc  密码 121995

mysql 密码121995



环境变量配置在：/etc/profile.d/myenv

自定义脚本在 /usr/bin/里面，查找以前的脚本是 ll /usr/bin | grep lpc



unify   多节点统一执行指令

hpsh 分发修改目录或文件到各节点

hish 启动或者停止metastore和hiveserver2服务 ，用start或者stop 参数控制，status可以看metastore和hiveserver2服务的运行状态



# linux管理集群相关指令

--help   对于不懂的脚本指令,百度的版本不同，一般直接在linux --help能看到指令，

​			 比如hadoop --help可以查到fs等指令，然后hadoop fs --help 可以看到fs之后可以接哪些指令

free -h 查看内存剩余情况

tail -n 10  a.txt  查看文件最后几行

tail -f a.txt 持续查看文件末尾，直到推出





# Hadoop篇

#### 组件官网下载

在官网下载的时候，如果是src.tar ，有src的都是源码，需要编译的。

一般都是下载bin，二进制已经编译好的





集群分配

project1 是namenode管理  有namenode

project2 是yarn管理  有resourcemanager

project3是2nn管理  有seceond namenode



3个服务器同时存数据，且执行任务 所以都有nodemanager和datanode





#### hdfs组成

namenode :

datanode:

secondnamenode:辅助namenode,定期合并edits,,fsimage,发送个namenode

client: 文件切分成block的时候在这里切分，因为hdfs有个图形化管理界面，正常操作都是通过client来执行



#### yarn组成

resource manager

node manager

application master 

container 

#### hadoop启动

群体启动hadoop    1.namenode 节点  sbin下的start-dfs.sh   2.在resourcemanager节点启动 start-yarn.sh

群体停止hadoop    1.namenode 节点 stop-dfs.sh  2. resourcemanager节点下stop-yarn.sh 

单点启动关闭节点  hdfs --deamon start/stop  namenode/datanode/secondarynamenod

​                         yarn --deamon start/stop  namenode/datanode/secondarynamenod

#### hadoop常用端口

9870 是hdfs中namenode 查看界面 project1:9870

8088 是hdfs中yarn   查看界面 proeject2:8088

8020是hdfs内部通信端口，想访问hdfs的文件要通过8020端口



#### 客户端上传下载

```
hadoop fs -moveFromLocal  /local/a.txt  /hdfs/a.txt  --从本地剪切过去
hadoop fs -copyFromLocal  /local/a.txt  /hdfs/a.txt  --从本地复制过去
hadoop fs -put  /local/a.txt  /hdfs/a.txt  --从本地复制过去等同于copyFromLocal
hadoop fs -appendToFile   /local/a.txt  /hdfs/a.txt  --把本地文件追加到hdfs指定文件末尾

hadoop fs -copyToLocal    /hdfs/a.txt   /local/a.txt  --从hdfs下载到本地
hadoop fs -get    /hdfs/a.txt   /local/a.txt  --从hdfs下载到本地,等同于-copyToLocal 
hadoop fs -getmerge    /hdfs/*   /local/a.txt  --从hdfs合并下载多个文件

```



####  脚本解析



# 后期规划（集群设置）

1.设置自动添加mysql数据的job，并且设置一个任务监视java程序

2.配置集群监听

3.定期合并小文件脚本







# 问题准备

1.hadoop文件写入的时候，如果多个任务同时操作一个表会出现什么？（文档里是一个文件只支持一个线程执行）

2.linux脚本问题，ssh $i  "zkServer.sh  $* "  2> /dev/null  ，如果把2> /dev/null 去掉，ssh执行的时候会打印出启动日志，

问题是如果启动失败，不打印日志不会出问题吗？测试了一下，改其中一个project3 变成4，之后没有错误日志，所以有待改进。

3.对于已经成型的集群，有几千个任务，有的任务迭代很多次已经没人需要了，这种任务应该停止，怎么找到那些相关任务，相关的sql停下来。后续怎么跟进维护这种。 如果相关数据已经没有更新了，那么sql执行的也快。



# 数仓设计

1.在电脑业务库里面，有之前对数仓库的理解



# 已经解决问题



# linux方面

1.有时候一个脚本，需要 ./shell1.sh 才能执行，有时候直接/shell1 就行执行了，第一种是bash解析器执行，不需要可执行权限，第二种是脚本自己执行，需要权限。





