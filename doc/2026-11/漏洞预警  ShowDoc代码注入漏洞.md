#  漏洞预警 | ShowDoc代码注入漏洞  
浅安
                    浅安  浅安安全   2026-11-04 23:50  
  
**0x00 漏洞编号**  
- # CVE-2026-120203  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
ShowDoc是基于thinkPHP开发的开源文档管理系统，支持使用Markdown语法书写API文档、数据字典、在线Excel文档等功能。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SXSjhhHib4Jz0yt06S7YHPTFibqzlrDoSLEUsdqYYXMeLfgic4rmxPobTq6jB1rd60icFMicu6WdVWGhPg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=0 "")  
  
**0x03 漏洞详情**  
  
**CVE-2026-120203**  
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
**简述：**  
ShowDoc存在代码注入漏洞，经过身份验证的用户可通过MockController::add存储包含转义括号的模板，mockjs会用eval()对其求值，然后通过MockController::infoByKey触发，进而以root身份执行任意命令。  
  
**0x04 影响版本**  
- 2.8.14 <= ShowDoc < 3.9.6  
  
**0x05 POC状态**  
- 未公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.showdoc.com.cn/  
  
  
  
