#  【安全圈】开源论坛软件 phpBB 曝身份认证绕过漏洞，官方发布 3.3.17 版本更新修复  
 安全圈   2026-06-16 08:31  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
漏洞  
  
  
据 phpBB 通报，旧版 phpBB（包括 3.3.16 及之前所有   
3.x  
 版本，以及 4.0.0-a2 之前的全部   
4.x  
 测试版本）均含有一项身份认证绕过漏洞，黑客可利用漏洞直接登录任意用户账号（甚至包括论坛管理员账号），**目前官方已发布 3.3.17 版本安全更新，修复了相应漏洞**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz/sbq02iadgfyEiacVCLU3ngwDTXzgsJHJ297dB4vV2XD5c7G87Y7MyBdb9gSf7DQjy3rhNiagMu8icY9wYJKgT3n5jdKhINibyfYV8xAzHu5KOmLQ/640?wx_fmt=other&from=appmsg "")  
  
  
该漏洞由安全公司 Aikido 发现，并通过 HackerOne 平台向官方披露。Aikido 表示，这一漏洞已经存在超过 10 年，黑客仅需发送特定的 HTTP 请求即可触发漏洞并登录任意账号。由于 phpBB 默认会公开论坛用户列表，攻击者能够轻松获取用户名，从而冒充管理员或普通用户，读取用户私信等敏感信息；若成功获取管理员权限，还可进一步获得论坛内容的完整读取、修改和删除权限。  
  
不过，由于 phpBB 的后台管理面板（Admin Control Panel，ACP）采用独立密码保护机制，因此该漏洞暂时无法直接导致远程代码执行（RCE）风险。  
  
  
  END    
  
  
阅读推荐  
  
  
[【安全圈】飞书崩了！！！](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652077424&idx=1&sn=ec984294f58341f0a28c1897cf2c9796&scene=21#wechat_redirect)  
  
  
  
[【安全圈】现实版黑客训练场！FBI 重金搭建肉鸡小镇：200 台服务器随便黑](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652077424&idx=2&sn=cb1e1a52d5f8192c33350491297932a6&scene=21#wechat_redirect)  
  
  
  
[【安全圈】美国政府命令 Anthropic 暂停向外国公民提供 Fable 5 和 Mythos 5 访问权限](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652077424&idx=3&sn=876d48c0f4f7fdbe0c6e8b11761471e1&scene=21#wechat_redirect)  
  
  
  
[【安全圈】Meta AI 客服系统被黑，2 万+ Instagram 账户沦陷](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652077404&idx=1&sn=36c21bc27d56fe347897a1e418312aff&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEDQIyPYpjfp0XDaaKjeaU6YdFae1iagIvFmFb4djeiahnUy2jBnxkMbaw/640?wx_fmt=png "")  
  
**安全圈**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
←扫码关注我们  
  
**网罗圈内热点 专注网络安全**  
  
**实时资讯一手掌握！**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
**好看你就分享 有用就点个赞**  
  
**支持「****安全圈」就点个三连吧！**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
  
