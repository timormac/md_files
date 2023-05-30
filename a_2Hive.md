# hive篇

### explain

可以使用explain命令，查看语法树，即hive先将sql语法转成对应的语法树  

explain extended 查看更多信息



### 执行hql方式

众所周知，我们执行一个HQL语句通常有以下几种方式：

$HIVE_HOME/bin/hive进入客户端，然后执行HQL；

$HIVE_HOME/bin/hive -e "hql"；

$HIVE_HOME/bin/hive -f hive.sql；

先开启hivesever2服务端，然后通过JDBC方式连接远程提交HQL。


### hiveserver2

hiveserver2是hive的链接服务器，要先启动这个，才能远程连接hive

### metastore服务

客户端连接metastore服务，metastore再去连接MySQL数据库来存取元数据。有了metastore服务，就可以有多个客户端同时连接，而且这些客户端不需要知道MySQL数据库的用户名和密码，只需要连接metastore 服务即可。

### 在线链接hive服务器

beeline -u jdbc:hive2://project1:10000 -n lpc



### hive脚本指令

```
hive -e "sql" > /home/a.txt  --把查询的sql 导入本地文件
```



### hql 语句

##### hive参数配置

```
在hive客户端中也可以手动设置mr配置

切换hive执行引擎，如果想临时切换，写在sql前面
set hive.execution.engine=mr/spark/tez ；

设置reduce数量
set mapreduce.job.reduces = num; 

设置reduce任务数
set mapreduce.reduce.tasks = num;  

开启hive最终输出数据压缩功能
set hive.exec.compress.output=true;

开启mapreduce最终输出数据压缩
set mapreduce.output.fileoutputformat.compress=true;

设置mapreduce最终数据输出压缩方式
set mapreduce.output.fileoutputformat.compress.codec = org.apache.hadoop.io.compress.SnappyCodec;

开启map join
set hive.auto.convert.join=true;

设置map join 启动时表的大小最大为1M
set hive.mapjoin.smalltable.filesize=1048576;

开启map端口预聚合
set hive.map.aggr = true

map端会预先聚合10000条数据
set hive.groupby.mapaggr.checkinterval = 10000

map端试聚合10000得到的数量与10000的比值，如果比值<0.3 则有继续聚合，如果大于0.3没有预聚合的意义
Hive.map.aggr.hash.min.reduction=0.3

//打开任务并行执行
set hive.exec.parallel=true; 
 //一个sql允许最大并行度，默认为8。
set hive.exec.parallel.thread.number=16; 


```



##### 查询库与表信息

```
desc database  db1   查询库信息
desc  database extended db1 查询库详细信息
desc  tb1  查询表信息
desc  formatted  tb1 查询表详细信息
```

##### explain执行计划

explain sql  查看执行计划

explain extended sql 查看详细计划

##### 建表语句

```
Hive上创建测试表test
create [external]  table test(
name string comment "aa",
friends array<string>,
children map<string, int>,
address struct<street:string, city:string>
)

partitioned by (a1,a2)  创建分区表
clustered by (a3,a4 )   创建分桶表
row format delimited fields terminated by ','  -- 列分隔符
collection items terminated by '_'    --MAP STRUCT 和 ARRAY 的分隔符(数据分割符号)
map keys terminated by ':'  	-- MAP中的key与value的分隔符
lines terminated by '\n';    	-- 行分隔符

stored as textfile  --存储格式
location '/home/hadoop/hive/warehouse/student' 表数据存储位置


字段解释：
external 为外部表，当删除表时，数据还在，元数据删除
row format delimited fields terminated by ','  -- 列分隔符
collection items terminated by '_'  	--MAP STRUCT 和 ARRAY 的分隔符(数据分割符号)
map keys terminated by ':'				-- MAP中的key与value的分隔符
lines terminated by '\n';					-- 行分隔符
```

##### stored储存方式

```
stored as  存储格式 textFile、SequenceFile、RCfile、ORCFile，parquet  这几种

textFile：
        默认的文件格式，行存储。

        优点：最简单的数据格式，便于和其它工具（pig，grep，awk）共享数据，便于查看和编辑；加载快；

        缺点：存储空间占用较大，I/O性能低；不可对数据进行切割、合并，不能进行并行操作；

        适用于小型查询，测试操作等。

sequqnceFile：
        键值对形式存储的二进制文本格式，行存储。

        优点：可压缩、可分割。优化I/O性能；可并行操作；

        缺点：存储空间占用最大，只局限于hadoop生态使用；

        适用于小数据，大部分都是列查询的操作。

RCFile：
        行列式存储。先将数据按行分块，每一个块数据转换成一个Record对象，避免读取一条数据需要读取多个block；然后块数据按列存储。

        优点：可压缩，高效的列存储，查询速度较快；

        缺点：加载时性能消耗较大，全量数据读取时性能较低。

ORCFile：
        优化后的RCFile，优缺点与RCFile类似，查询效率最高。

        适用于hive中、大型的存储和查询。

Parquet：
        列存储。

        优点：更高效的压缩和编码；不与任何数据处理技术绑定，可用于多种数据处理框架。

        缺点：不支持update，insert，delete，ACID

        适用于字段非常多，无更新，只读取部分列数据。

总结：

表的字段个数不多时文件按块进行压缩，行存储比较高效；

表的字段个数成百上千时，且只需要个别字段的查询时，列存储可较大提高效率；

数据仓库一次写入，多次读取，orc格式比较有优势。
```



##### 分区表

```
create table tb(
name  string,
age   int comment "年龄"
)
partitioned by (year int,month int) 
rowfomat delimited fields terminated by '\t';
```



##### 分桶表

```
create table if not exists tb(
	userid   string,
	orderno string,
	name string
)
clustered by (userid,orderno) into 4 buckets
row format delimited fields terminated by '\t'
;

哪些表需要分桶：
因为分桶表必须执行过一次mr，所以从mysql直接导过来的表，要处理一次才行。所以应该是经常使用的表，才会去分桶，比如订单信息聚合清洗之后，这种表分桶。

分桶的字段选择：
选择那些经常关联的字段

分桶表的优势：
获得更高的查询处理效率。桶为表加上了额外的结构，Hive 在处理有些查询时能利用这个结构。具体而言，连接两个在(包含连接列的)相同列上划分了桶的表，可以使用 Map 端连接 (Map-side join)高效的实现。比如JOIN操作。对于JOIN操作两个表有一个相同的列，如果对这两个表都进行了桶操作。那么将保存相同列值的桶进行JOIN操作就可以，可以大大较少JOIN的数据量
要想join能用到桶，那么必须桶的数量相同吧（猜测）

分桶数量
因为有多少个桶，必须有多少个reduce，所以你集群能同时运行多少个reduce就多少桶。
而且要想join能用到桶，那么桶的数量必须相同（猜测）


```

##### hive表数据导入

```
1.从hdfs或本地目录 中导入数据到hive
  load data [local] inpath  '/opt/a.txt'  [overwrite] into table t1 [partition (month=202012)]
  注意load数据之后,hdfs上的数据就没有了，如果想原文件继续存在，那么创建的表必须是外部表。

	提示当原数据是txt文本格式，用load 导入到设置为parquet的表中，不能将数据转为parquet
  
  

2.查询语句导入数据
3.创建表时，直接location指定hdfs目录位置，目录下的数据直接就在表中
```

##### hive表数据导出

```
1. insert overwrite local directory "/home/tb" select * from t1 ;   将查询结果导入本地
2. insert overwrite local directory  row format delimited fields terminated '\t' "/home/tb" select * from t1 ;   
将查询结果导入本地,按照指定分割格式
如果想导出到hdfs上，那么把local去掉；

3.不在hive客户端执行，在hive脚本中执行   hive -e 'select * from t1' > /home/data
4.sqoop 把hive导入到mysql里面

```

##### 查看锁表

```
hive> show locks;
```

##### cast转换

一般用于把string转为int，比如有的字段存为string， 实际是数字，有时候需要order的时候，18 和2如果是字符串 那么排序 18是小于2的。如果是把字符串转为数字，就会是null

##### sort by 局部排序

如果使用order by 那么只有一个reduce，因为为了全局排序，有时候为了快速，使用局部排序。但是如果不结合distribute,每个reduce是按什么分区的不确定，所以用distribute 可以指定分区的字段，这样就有用了，单独一个sort by 没有意义。

distribute by + sort by就是该替代方案，被distribute by设定的字段为KEY，数据会被HASH分发到不同的reducer机器上，然后sort by会对同一个reducer机器上的每组数据进行局部排序。

```
select * from tb distribute by  areaid  sort by num desc; 
```

### 常用函数

##### 条件函数

nvl(name,"姓名丢失")

if (age>18,a,b)

case when  
select 
case typeid  
when 1  then '酒店'
when 2  then '食品'
else '未知'  end as type 



##### 日期函数

date_format(date,yyyy/MM/dd) 将日期转换为/的格式，不过date必须是yyyy-MM-dd 或者yyyy-MM-dd hh:mm:ss	格式

##### 字符串函数

concat(col1,clo2,col3....)

concat_ws("," col1,col2,col3) 这个函数会跳过为null的字段，所以最好加上nvl判空

split(col,'/') 返回一个array



##### 多行转单行

collect_set(col) 将一个字段去重之后汇总，返回的是一个array



##### 单行转多行

explode   

explode(col) 将hive中的array,map 进行拆分,  配合lateral view用

select  movie,cate，tmp.type
from tb
lateral view explode( split(cate,',') ) tmp as type;



##### 窗口函数

    在sql中有一类函数叫做聚合函数，例如sum(), avg(),max()等等，这类函数可以将多行数据按照规则聚集为一行，一般来讲，聚集后的行数是要少于聚集前的行数的。但是有时候我们既想要显示聚集前的数据，又要显示聚集后的函数，这个时候，我们便引入了窗口函数。窗口函数又叫OLAP函数/分析函数.
    窗口函数最重要的关键字是partition by和 order by.具体语法是：over(partition  by  用于分组的列名  order  by  用于排序的列名)
    
    partition by用来限制窗口的范围，也可以结合current row ,n proceeding等 复合范围


select name, departid,salary

row_number( partition by departid )   from  tb ;

这里的窗口范围是指，当前行的所在的departid对应的组所有数据。



over()窗口限定范围，如果over中有order by 并且没有制定范围的话，默认是受用between 第一行 and current row 

unbounded preceding 起点行

unbounded following 终止行

current row 当前行

n preceding 向前n行

n following  向后n行

具体写法  rows between   unbounded preceding  and current row



rank()     相同的会重复

row_number()相同的不会重复

dense_rank()

lag(col ,n,default) 统计窗口中向前第n行的值  lag( name ,2,'无') over( partition by age  order by num)

lead(col,n,default)统计窗口中向后第n行的值 



### 积累hql案例

##### 部门前3（不开窗）

不用开窗，求每个部门前2

```
部门id   用户id   工资
11			1        100
11	    2        120
11      3        130




```

##### 连续3天



### hive优化

##### map join

map join主要是在关联的时候使用，hive开启map join后可以将关联的小表放入[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)中去执行，以此来提高脚本的运行速度

开启map join

set hive.auto.convert.join=true;

设置map join 启动时表的大小最大为1M
set hive.mapjoin.smalltable.filesize=1048576;



map join的一点小坑

map join虽然很好，但是会有如下问题：

1）map join关联多个小表时，都放入内存，则考虑内存大小需要针对上述小表大小进行累加

2）大表B表map join关联分区小表A表（200M）时，即使限制了A的分区（取10M），但依旧放入内存的大小依旧是A表的原先大小（200M）

##### 先过滤再关联

对于一些join的操作 先过滤再去关联，不要先关联再过滤，每个join都是开启一次mr，先过滤减少传输数据



##### 开启map端聚合

对于一些聚合函数比如 sum(),count() 这种，是可以提前在map端预聚合的。

比如按性别group by ，那么最后最多只有2个reduce任务，有10个map任务，在map端先聚合的话，会提升很多速度，而且传递的数据也少了很多。



并不是所有情况都是开启map聚合就是好

比如按用户id 去统计订单数，因为用户id少会出现多条重复订单，如果这时候开启map聚合，在map端聚合一次之后，数据量基本无变化，然后传递到reduce端，还要再聚合一次，反而更慢了，所以对于这种情况反而要关闭map聚合，用参数设置自动开关



开启map端口预聚合
set hive.map.aggr = true

map端会预先聚合10000条数据
set hive.groupby.mapaggr.checkinterval = 10000

map端试聚合10000得到的数量与10000的比值，如果比值<0.3 则有继续聚合，如果大于0.3没有预聚合的意义，会关闭预聚合

Hive.map.aggr.hash.min.reduction=0.3



##### 合理设置map,reduce数量

是不是map数越多越好？

答案是否定的。如果一个任务有很多小文件（远远小于块大小128m），则每个小文件也会被当做一个块，用一个map任务来完成，而一个map任务启动和初始化的时间远远大于逻辑处理的时间，就会造成很大的资源浪费。而且，同时可执行的map数是受限的。

是不是保证每个map处理接近128m的文件块，就高枕无忧了？

答案也是不一定。比如有一个127m的文件，正常会用一个map去完成，但这个文件只有一个或者两个小字段，却有几千万的记录，如果map处理的逻辑比较复杂，用一个map任务去做，肯定也比较耗时。

###### 合并小文件：

（1）在map执行前合并小文件，减少map数：CombineHiveInputFormat具有对小文件进行合并的功能（系统默认的格式）。HiveInputFormat没有对小文件合并功能。

set hive.input.format= org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;

（2）在Map-Reduce的任务结束时合并小文件的设置：

在map-only任务结束时合并小文件，默认true

SET hive.merge.mapfiles = true;

在map-reduce任务结束时合并小文件，默认false

SET hive.merge.mapredfiles = true;

合并文件的大小，默认256M

SET hive.merge.size.per.task = 268435456;

当输出文件的平均大小小于该值时，启动一个独立的map-reduce任务进行文件merge

SET hive.merge.smallfiles.avgsize = 16777216;

（1）在map执行前合并小文件，减少map数：CombineHiveInputFormat具有对小文件进行合并的功能（系统默认的格式）。HiveInputFormat没有对小文件合并功能。

set hive.input.format= org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;

（2）在Map-Reduce的任务结束时合并小文件的设置：

在map-only任务结束时合并小文件，默认true

SET hive.merge.mapfiles = true;

在map-reduce任务结束时合并小文件，默认false

SET hive.merge.mapredfiles = true;

合并文件的大小，默认256M

SET hive.merge.size.per.task = 268435456;

当输出文件的平均大小小于该值时，启动一个独立的map-reduce任务进行文件merge

SET hive.merge.smallfiles.avgsize = 16777216;



###### 合理设置reduce个数

调整reduce个数方法一

（1）每个Reduce处理的数据量默认是256MB

hive.exec.reducers.bytes.per.reducer=256000000

（2）每个任务最大的reduce数，默认为1009

hive.exec.reducers.max=1009

（3）计算reducer数的公式

N=min(参数2，总输入数据量/参数1)

2．调整reduce个数方法二

在hadoop的mapred-default.xml文件中修改

设置每个job的Reduce个数

set mapreduce.job.reduces = 15;

3．reduce个数并不是越多越好

1）过多的启动和初始化reduce也会消耗时间和资源；

2）另外，有多少个reduce，就会有多少个输出文件，如果生成了很多个小文件，那么如果这些小文件作为下一个任务的输入，则也会出现小文件过多的问题；

在设置reduce个数的时候也需要考虑这两个原则：处理大数据量利用合适的reduce数；使单个reduce任务处理数据量大小要合适；

##### jvm重用

JVM重用是Hadoop调优参数的内容，其对Hive的性能具有非常大的影响，特别是对于很难避免小文件的场景或task特别多的场景，这类场景大多数执行时间都很短。

Hadoop的默认配置通常是使用派生JVM来执行map和Reduce任务的。这时JVM的启动过程可能会造成相当大的开销，尤其是执行的job包含有成百上千task任务的情况。JVM重用可以使得JVM实例在同一个job中重新使用N次。N的值可以在Hadoop的mapred-site.xml文件中进行配置。通常在10-20之间，具体多少需要根据具体业务场景测试得出。

这个功能的缺点是，开启JVM重用将一直占用使用到的task插槽，以便进行重用，直到任务完成后才能释放。如果某个“不平衡的”job中有某几个reduce task执行的时间要比其他Reduce task消耗的时间多的多的话，那么保留的插槽就会一直空闲着却无法被其他的job使用，直到所有的task都结束了才会释放。



### hive遇到的问题解决

1  

hive执行一个hql时发现一直无法执行，然后在yarn界面看到的日志，找不到具体原因。具体日志如下

```
在hive的报错日志显示如下
ERROR : Job failed with org.apache.spark.SparkException: Job aborted due to stage failure: Task 0 in stage 13.0 failed 4 times, most recent failure: Lost task 0.3 in stage 13.0 (TID 44, project1, executor 2): UnknownReason

在yarn日志里的报错显示的是这个
java.lang.NoClassDefFoundError: org/antlr/runtime/tree/CommonTree
这个问题好像是只有用spark引擎才会出现这个问题，切换成mr引擎就不会出现，是偶尔出现的问题

最后的解决方法是自己好了，猜测应该是内存不够的原因，因为之前可获取内存最大是200m，后面释放出来了就够了。
```

解决方法 yarn执行任务的日志，要去/op/moudle/hadoop/logs/userlogs里面去找，这里有container的运行日志，

然后去找stuerr，然后看里面具体爆什么错误。因为集群节点很多，有时候执行的mr任务只在几个节点执行，所以要先找到出问题的节点是哪个，然后去对应的节点的hadoop目录去找日志



### 细节问题

hive第一次在客户端执行sql的时候非常慢，要在各节点建yarn容器，建立过一次之后，在8088yarn页面上能看到一个任务一直在挂起



