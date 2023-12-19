# HBase数据迁移

### 创建临时HDFS目录备份待迁移数据

```shell
hdfs dfds -mkdir /tmp/your_bak_path
```

### 删除HBase表

```shell
disable 'your_table_name'
drop 'your_table_name'
```

### 重新创建同名同结构的HBase表

```shell
create 'your_table_name',{NAME =>'f1',COMPRESSION => 'SNAPPY'}
```

### 获取临时路径下所有Region目录

```shell
hdfs dfs -ls /tmp/your_bak_path | awk -F '/' '{if($0 ~/^d && $NF!="hbase") print $NF}' |egrep -v ".tmp|.tabledesc" > /tmp/region.txt
```

### 使用Bulkload将数据加载到新建的表中

```shell
cat /tmp/region.txt | while read line;do hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmp/your_bak_path/$line your_table_name;done;
```

