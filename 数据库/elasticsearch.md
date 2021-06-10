## Elasticsearch是什么

### 介绍

Elasticsearch 是一个分布式可扩展的实时搜索和分析引擎，一个建立在全文搜索引擎 Apache Lucene(TM) 基础上的搜索引擎。

- 分布式实时文件存储，并将每一个字段都编入索引，使其可以被搜索。
- 实时分析的分布式搜索引擎。
- 能胜任上百个服务节点的扩展，并支持 PB 级别的结构化或者非结构化数据。

### 基本概念

Elasticsearch是面向文档型数据库，一条数据在这里就是一个文档，用json作为文档序列化的格式。

```
关系型数据库		=>	数据库				=>		表					=>	行								=>	列
Elasticsearch	=>	索引（index）=>		类型（type）	=>	文档（documents）=>		字段（fields）
```

### 索引

Elasticsearch索引的精髓：一切设计都是为了提高搜索的性能

Elasticsearch在插入一条数据的同时，还为数据中的字段建立索引 - 倒排索引。

