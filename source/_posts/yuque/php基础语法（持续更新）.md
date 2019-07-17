
---

title: php基础语法（持续更新）

date: 2019-04-16 16:19:09 +0800

tags: []

---
<a name="array_chunk"></a>
# array_chunk
将一个数组分割成多个

<a name="f411d0f1"></a>
## 说明
```php
array_chunk ( array $array , int $size [, bool $preserve_keys = false ] ) : array
```

将一个数组分割成多个数组，其中每个数组的单元数目由 size 决定。最后一个数组的单元数目可能会少于 size 个。

<a name="226b5276"></a>
## 参数  
array<br />需要操作的数组<br />size<br />每个数组的单元数目<br />preserve_keys<br />设为 TRUE，可以使 PHP 保留输入数组中原来的键名。如果你指定了 FALSE，那每个结果数组将用从零开始的新数字索引。默认值是 FALSE。

<a name="Xvblj"></a>
# md5_file
计算指定文件的 MD5 散列值，支持stream即远程文件<br />md5_file在计算前用fopen打开，读取数据，然后做计算，远程open超时了<br />这个也是走的系统的配置php.ini<br />default_socket_timeout = 60

<a name="e7sCQ"></a>
# get_headers
get_headers — 取得服务器响应一个 HTTP 请求所发送的所有标头
<a name="zjwFG"></a>
### 说明[ ](https://www.php.net/manual/zh/function.get-headers.php#refsect1-function.get-headers-description)
**get_headers** ( string `$url` [, int `$format` = 0 ] ) : array<br />**get_headers()** 返回一个数组，包含有服务器响应一个 HTTP 请求所发送的标头。
<a name="x72XJ"></a>
### 参数[ ](https://www.php.net/manual/zh/function.get-headers.php#refsect1-function.get-headers-parameters)
`url`<br />目标 URL。<br />`format`<br />如果将可选的 `format` 参数设为 1，则 **get_headers()** 会解析相应的信息并设定数组的键名。
<a name="lwJJ0"></a>
### 返回值[ ](https://www.php.net/manual/zh/function.get-headers.php#refsect1-function.get-headers-returnvalues)
返回包含有服务器响应一个 HTTP 请求所发送标头的索引或关联数组，如果失败则返回 **`FALSE`**。



