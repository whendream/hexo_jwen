
---

title: SQL语句小tips（持续更新）

date: 2019-04-16 20:17:21 +0800

tags: []

---
<a name="7dcb1c28"></a>
# 统计非法数据

判断people_id是否是32为字母组成的，统计不满足要求的数据

SELECT COUNT(IF(BINARY people_id NOT REGEXP '^[0-9a-z]{32}',TRUE,NULL)) AS people_id_illegality_cnt FROM people_day

<a name="97def91a"></a>
### if 表达式

> `IF( expr1 , expr2 , expr3 )`


expr1 的值为 TRUE，则返回值为 expr2 <br />
expr1 的值为FALSE，则返回值为 expr3

其中TRUE，select出来是1

**BINARY**

where条件加入BINARY的话，可以对大小写敏感，默认mysql是不区分大小写的

**非空判断，如果为空置为0，非空取当前值**

两种方法：

1. 利用if+isnull判断是否为空

```sql
select if(ISNULL(sum(final_amount)),0,sum(final_amount)) as final_amount_total from day_income where day=20180821 and income_type=1;
```

2 . 利用COALESCE函数

COALESCE(value1,...) ：返回第一个非NULL的参数

说明：返回列表中第一个非空值，如果没有非NULL值，则返回NULL。

```sql
mysql> SELECT COALESCE(NULL,1); -> 1 mysql> SELECT COALESCE(NULL,NULL,NULL); -> NULL
select COALESCE(sum(final_amount),0)  as final_amount_total from day_income where day=20180822 and income_type=1;
```

where语句里使用if判断。好难，感觉不好

```sql
SELECT COUNT(1) FROM test_tabel WHERE month = ( SELECT CASE 1
WHEN DAY(CURDATE()) > 5 THEN date_format(curdate(), '%Y-%m') ELSE date_format(date_sub(curdate(), INTERVAL 1 MONTH), '%Y-%m') END AS correctMonth
);
```


