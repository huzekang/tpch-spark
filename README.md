# tpch-spark

TPC-H queries implemented in Spark using the DataFrames API.
Tested under Spark 2.4.0

Savvas Savvides

savvas@purdue.edu


### Generating tables

Under the dbgen directory do:
```
make
```

This should generate an executable called `dbgen`
```
./dbgen -h
```

gives you the various options for generating the tables. The simplest case is running:
```
./dbgen
```
which generates tables with extension `.tbl` with scale 1 (default) for a total of rougly 1GB size across all tables. For different size tables you can use the `-s` option:
```
./dbgen -s 10
```
will generate roughly 10GB of input data.

You can then either upload your data to hdfs or read them locally.

### Running

First compile using:

```
sbt package
```

Make sure you set the INPUT_DIR and OUTPUT_DIR in `TpchQuery` class before compiling to point to the
location the of the input data and where the output should be saved.

You can then run a query using:

```
spark-submit --class "main.scala.TpchQuery" --master MASTER target/scala-2.11/spark-tpc-h-queries_2.11-1.0.jar $query
```

where $query is the number of the query to run e.g 1, 2, ..., 22
and MASTER specifies the spark-mode e.g local, yarn, standalone etc...

## localFS example
1. 生成1G数据
```shell script
cd tpch-spark-master/dbgen
./dbgen -s 1
```
执行完可以看到`dbgen`目录下多了如下`*.tbl`的数据文件。
![](http://image-picgo.test.upcdn.net/img/20210513150210.png)

2. 将编译好的jar放入`tpch-spark-master`目录中
![](http://image-picgo.test.upcdn.net/img/20210513143957.png)

3. 执行spark程序
```shell script
spark-submit --class "main.scala.TpchQuery" --master local[*] spark-tpc-h-queries_2.11-1.0.jar 

```
4. 查看测试结果
![](http://image-picgo.test.upcdn.net/img/20210513145745.png)

打开结果。
![](http://image-picgo.test.upcdn.net/img/20210513150257.png)


### 性能测试结果

| 文件系统 | master   | 数据容量 | 总耗时 |
| -------- | -------- | -------- | ------ |
| localFS  | local[*] | 10G      | 794s   |


### Other Implementations

1. Data generator (http://www.tpc.org/tpch/)

2. TPC-H for Hive (https://issues.apache.org/jira/browse/hive-600)

3. TPC-H for PIG (https://github.com/ssavvides/tpch-pig)
