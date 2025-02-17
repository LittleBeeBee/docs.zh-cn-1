# StarRocks

## StarRocks是什么

* StarRocks是新一代极速全场景MPP数据库。
* StarRocks充分吸收关系型OLAP数据库和分布式存储系统在大数据时代的优秀研究成果，在业界实践的基础上，进一步改进优化、升级架构，并增添了众多全新功能，形成了全新的企业级产品。
* StarRocks致力于构建极速统一分析体验，满足企业用户的多种数据分析场景，支持多种数据模型(明细模型、聚合模型、更新模型)，多种导入方式（批量和实时），支持导入多达10000列的数据，可整合和接入多种现有系统(Spark、Flink、Hive、 ElasticSearch)。
* StarRocks兼容MySQL协议，可使用MySQL客户端和常用BI工具对接StarRocks来进行数据分析。
* StarRocks采用分布式架构，对数据表进行水平划分并以多副本存储。集群规模可以灵活伸缩，能够支持10PB级别的数据分析; 支持MPP框架，并行加速计算; 支持多副本，具有弹性容错能力。
* StarRocks采用关系模型，使用严格的数据类型和列式存储引擎，通过编码和压缩技术，降低读写放大；使用向量化执行方式，充分挖掘多核CPU的并行计算能力，从而显著提升查询性能。

## StarRocks特性

StarRocks的架构设计融合了MPP数据库，以及分布式系统的设计思想，具有以下特性：

### 架构精简

StarRocks内部通过MPP计算框架完成SQL的具体执行工作。MPP框架本身能够充分的利用多节点的计算能力，整个查询并行执行，从而实现良好的交互式分析体验。
StarRocks集群不需要依赖任何其他组件，易部署、易维护，极简的架构设计，降低了StarRocks系统的复杂度和维护成本，同时也提升了系统的可靠性和扩展性。 管理员只需要专注于StarRocks系统，无需学习和管理任何其他外部系统。

### 全面向量化引擎

StarRocks的计算层全面采用了向量化技术，将所有算子、函数、扫描过滤和导入导出模块进行了系统性优化。通过列式的内存布局、适配CPU的SIMD指令集等手段，充分发挥了现代CPU的并行计算能力，从而实现亚秒级别的多维分析能力。

### 智能查询优化

StarRocks通过CBO优化器(Cost Based Optimizer)可以对复杂查询自动优化。无需人工干预，就可以通过统计信息合理估算执行成本，生成更优的执行计划，大大提高了Adhoc和ETL场景的数据分析效率。

### 联邦查询

StarRocks支持使用外表的方式进行联邦查询，当前可以支持Hive、MySQL、Elasticsearch三种类型的外表，用户无需通过数据导入，可以直接进行数据查询加速。

### 高效更新

StarRocks支持多种数据模型，其中更新模型可以按照主键进行upsert/delete操作，通过存储和索引的优化可以在并发更新的同时实现高效的查询优化，更好的服务实时数仓的场景。

### 智能物化视图

StarRocks支持智能的物化视图。用户可以通过创建物化视图，预先计算生成预聚合表用于加速聚合类查询请求。StarRocks的物化视图能够在数据导入时自动完成汇聚，与原始表数据保持一致。并且在查询的时候，用户无需指定物化视图，StarRocks能够自动选择最优的物化视图来满足查询请求。

### 标准SQL

StarRocks支持标准的SQL语法，包括聚合、JOIN、排序、窗口函数和自定义函数等功能。StarRocks可以完整支持TPC-H的22个SQL和TPC-DS的99个SQL。此外，StarRocks还兼容MySQL协议语法，可使用现有的各种客户端工具、BI软件访问StarRocks，对StarRocks中的数据进行拖拽式分析。

### 流批一体

StarRocks支持实时和批量两种数据导入方式，支持的数据源有Kafka、HDFS、本地文件，支持的数据格式有ORC、Parquet和CSV等，支持导入多达10000列的数据。StarRocks可以实时消费Kafka数据来完成数据导入，保证数据不丢不重（exactly once）。StarRocks也可以从本地或者远程（HDFS）批量导入数据。

### 高可用易扩展

StarRocks的元数据和数据都是多副本存储，并且集群中服务有热备，多实例部署，避免了单点故障。集群具有自愈能力，可弹性恢复，节点的宕机、下线、异常都不会影响StarRocks集群服务的整体稳定性。
StarRocks采用分布式架构，存储容量和计算能力可近乎线性水平扩展。StarRocks单集群的节点规模可扩展到数百节点，数据规模可达到10PB级别。 扩缩容期间无需停服，可以正常提供查询服务。
另外StarRocks中表模式热变更，可通过一条简单SQL命令动态地修改表的定义，例如增加列、减少列、新建物化视图等。同时，处于模式变更中的表也可也正常导入和查询数据。

## StarRocks适合什么场景

StarRocks可以满足企业级用户的多种分析需求，包括OLAP多维分析、定制报表、实时数据分析和Ad-hoc数据分析等。具体的业务场景包括：

* OLAP多维分析
  * 用户行为分析
  * 用户画像、标签分析、圈人
  * 高维业务指标报表
  * 自助式报表平台
  * 业务问题探查分析
  * 跨主题业务分析
  * 财务报表
  * 系统监控分析
* 实时数据分析
  * 电商大促数据分析
  * 教育行业的直播质量分析
  * 物流行业的运单分析
  * 金融行业绩效分析、指标计算
  * 广告投放分析
  * 管理驾驶舱
  * 探针分析APM（Application Performance Management）
* 高并发查询
  * 广告主报表分析
  * 零售行业渠道人员分析
  * SaaS行业面向用户分析报表
  * Dashbroad多页面分析
* 统一分析
  * 通过使用一套系统解决多维分析、高并发查询、预计算、实时分析、Adhoc查询等场景，降低系统复杂度和多技术栈开发与维护成本。
