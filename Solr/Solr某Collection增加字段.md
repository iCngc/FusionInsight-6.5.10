# Solr某Collection增加字段

> FusionInsight 6.5.1.10版本中Solr（6.2.0）虽然可以托管Collection的schema.xml配置集，并可以在页面进行动态添加字段，但无法立即生效，只能通过重启Collection所在的SolrServer节点进行配置集同步。
>
> 另外，对于老版本schema.xml配置集并没有托管到Solr中，无法在Solr的Web UI页面进行动态添加字段，只能将配置从ZooKeeper中获取后修改并重新上传，下面的方式就是在这个背景下需要做的。

### 获取Solr的配置文件集到本地客户端

```shell
solr confset --get name path
```

- name为需要获取的配置文件集名称
- path为本地客户端路径

### 修改schema.xml文件 增加或删除字段

```xml
<?xml version="1.0" encoding="UTF-8">
<schema name="example" vesion="1.5">
....
<field name="new_field_name",type="string",indexed="true",stored="false">
</schema>
```

### 更新Solr配置文件集

```shell
solrctl confset --update name path -force
```

- name为需要更新的配置文件集名称
- path为本地客户端路径
- force为可选项，表示没有schema.xml和solrconfig.xml依旧可以更新配置文件集

### 找到Collection所在的SolrServer并重启

![avatar](E:\work\Typora-Image\Solr.jpg)

