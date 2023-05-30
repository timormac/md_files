####  使用场景

有一些数据没办法存到mysql里面，比如实时需求里面的记录用户今天首次登陆时间等等，这种没有存mysql没有意义。

#### 问题待解决



1.

ClickHouse 和 HBase 是两种不同的数据库系统，它们之间的主要区别如下:

1. 数据模型: ClickHouse 是一个列式存储的数据库，而 HBase 是一个键值存储的数据库。
2. 性能: ClickHouse 以高性能和大数据处理能力著称，而 HBase 则更适用于大规模数据存储和实时数据读写。
3. 数据处理: ClickHouse 支持 SQL 查询语言，支持复杂的数据分析和处理，而 HBase 更适合于实时读写数据。
4. 分布式架构: ClickHouse 采用分布式集群架构，而 HBase 是基于 Hadoop 的分布式数据库。

总的来说，ClickHouse 更适合于大数据分析和报表系统，而 HBase 更适合于大规模数据存储和实时数据读写。



clickhouse是一个为了OLAP而生的列数据库

clickhouse也支持join，那么这不是所有的都比hive+hdfs好了吗？

能存大量数据，还是个数据库，还有自己的数据计算引擎。

之前就有想过，怎么用hive对接habase的索引。这是类似的实现吗？

