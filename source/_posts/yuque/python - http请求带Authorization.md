
---

title: python - http请求带Authorization

date: 2019-04-17 19:53:19 +0800

tags: []

---
<a name="42550742"></a>
# # 背景

接入公司的一个数据统计平台，该平台的接口是带上了Authorization验证方式来保证验签计算安全

<a name="466f7b97"></a>
# # 方法

其实很简单，就是在header中加入key=Authorization，value是协商好的协议即可；

如，我们这边是base64.b64encode(uae_name + ":" + uae_passwd);

因此计算就是：

```python
# 计算uae的authorization
def get_authorization():
    return base64.b64encode(uae_name + ":" + uae_passwd);

headers = {
            'Authorization': 'Basic {}'.format(get_authorization()),
            'ua': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.125 Safari/537.36'
        }

resp = requests.get(url, headers=headers)
get_result(json.loads(resp.content))
print ""
```


