# perecentile_approx_raw

## 功能

计算给定参数 `x` 的百分位数，若 `x` 是列，则先对该列升序排序，然后取精确的第 `y` 位百分数。

## 语法

```Haskell
PERCENTILE_APPROX_RAW(x, y);
```

## 参数说明

`x`: 支持的数据类型为 PERCENTILE，可为列、数值。

`y`: 支持的数据类型为 DOUBLE，取值为 [0.0,1.0]。

## 返回值说明

返回值的数据类型为 PERCENTILE。

## 示例

建表

```sql
CREATE TABLE `aggregate_tbl` (
  `site_id` largeint(40) NOT NULL COMMENT "id of site",
  `date` date NOT NULL COMMENT "time of event",
  `city_code` varchar(20) NULL COMMENT "city_code of user",
  `pv` bigint(20) SUM NULL DEFAULT "0" COMMENT "total page views",
  `precent` PERCENTILE PERCENTILE_UNION COMMENT "others"
) ENGINE=OLAP
AGGREGATE KEY(`site_id`, `date`, `city_code`)
COMMENT "OLAP"
DISTRIBUTED BY HASH(`site_id`) BUCKETS 8
PROPERTIES (
"replication_num" = "1",
"in_memory" = "false",
"storage_format" = "DEFAULT",
"enable_persistent_index" = "false"
);
```

插入数据

```sql
insert into aggregate_tbl values (5, '2020-02-23', 'city_code', 555, percentile_hash(1));
insert into aggregate_tbl values (5, '2020-02-23', 'city_code', 555, percentile_hash(2));
insert into aggregate_tbl values (5, '2020-02-23', 'city_code', 555, percentile_hash(3));
insert into aggregate_tbl values (5, '2020-02-23', 'city_code', 555, percentile_hash(4));
```

计算

```Plain Text
mysql> select percentile_approx_raw(precent, 0.5) from aggregate_tbl;
+-------------------------------------+
| percentile_approx_raw(precent, 0.5) |
+-------------------------------------+
|                                 2.5 |
+-------------------------------------+
1 row in set (0.03 sec)
```

## 关键词

PERCENTILE_APPROX_RAW
