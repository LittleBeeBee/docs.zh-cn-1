# to_bitmap

## 功能

输入为取值在 0 ~ 18446744073709551615 区间的 unsigned bigint , 输出为包含该元素的 bitmap
该函数主要用于 stream load 任务将整型字段导入 StarRocks 表的 bitmap 字段, 如下例:

```bash
cat data | curl --location-trusted -u user:passwd -T - \
    -H "columns: dt,page,user_id, user_id=to_bitmap(user_id)" \
    http://host:8410/api/test/testDb/_stream_load
```

## 语法

```Haskell
TO_BITMAP(expr)
```

## 参数说明

`expr`: 支持的数据类型为 unsigned BIGINT

## 返回值说明

返回值的数据类型为 BITMAP

## 示例

```Plain Text
MySQL > select bitmap_count(to_bitmap(10));
+-----------------------------+
| bitmap_count(to_bitmap(10)) |
+-----------------------------+
|                           1 |
+-----------------------------+
```

## 关键词

TO_BITMAP, BITMAP
