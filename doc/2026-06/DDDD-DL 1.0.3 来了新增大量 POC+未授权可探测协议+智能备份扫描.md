#  DDDD-DL 1.0.3 来了新增大量 POC+未授权可探测协议+智能备份扫描  
 AnWangsec   2026-06-16 08:52  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/Oiag47y540y80Cnkg4oGv0gIexsUXjEAdvxfTzMGWcX6Hib3dIFp3JAILthOg6ibyRLuI5VZXceVlVpmfgkTZaPxKnmW3M2I0YOPNXAYXibWACI/640?wx_fmt=gif&from=appmsg "")  
  
点击箭头处  
“蓝色字”  
，关注我们哦！！  
  
现在只对常读和星标的公众号才展示大图推送，建议大家能把  
**迷人安全**  
“  
**设为星标**  
”，  
否则可能就看不到了啦  
！  
  
【  
声明  
】本文所涉及的技术、思路和工具仅用于安全测试和防御研究，切勿将其用于非法入侵或攻击他人系统以及盈利等目的，一切后果由操作者自行承担！！！  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Oiag47y540y87BtnBeODn3mUYr2bF7Oa5dIcyqryqYDWJYJerFvhNHSEYYV4ausZaZpdL9KgjIicddibhnmwUxlgCsTwQpwBlyKDicxdmgdbVY8/640?wx_fmt=png&from=appmsg "")  
  
  
  
写在开头：本项目完全免费，重在总结问题与改进，喜欢的老铁可以点个关注+star 支持一下，欢迎大家提出issue！！下载地址见文末  
  
经过前一阵的测试和问题反馈，以及和诸多师傅们讨论，进行了以下更新，在此由衷感谢以下俩大ip 提供的支持和帮助：  
  
  
  
  
更新 POC 目前是 4668 个，还在持续新增中（dldl）  
![assets/96f121c4959b6c6fc0fe92b06e7b8faf_MD5.jpg](https://mmbiz.qpic.cn/mmbiz_png/Oiag47y540yib4dVgiaSAejhQA48jmwTRNVKQqfxicKYiayhzEFwUg742gOyGD8U90icEianXbUskvJ4evLnrOyzJm6WIq4qNjhAGhGIN1XcJDL0fY/640?wx_fmt=png&from=appmsg "")  
  
## v1.0.3  
  
修复 ksubdomain  
 在本机自动获取网卡超时导致不可用的问题。现在主程序在未显式指定 -ksi/--ksubdomain-interface  
 时，会优先自动选择可用的非 loopback、非点对点 IPv4 网卡，并以 --eth  
 参数传给 ksubdomain  
；仍可通过 -ksi en0  
 等方式手动指定网卡。  
  
新增 macOS BPF 权限预检。当前用户无法读写 /dev/bpf*  
 时，会直接提示需要使用 sudo  
 运行或执行 sudo chmod a+rw /dev/bpf*  
，避免只看到 ksubdomain  
 的“获取网络设备超时”。  
## v1.0.2  
  
新增网站备份文件扫描功能，通过 -bs/--backup-scan  
 启用。扫描会基于已存活 Web 资产生成常见备份文件名，覆盖域名原文、点转下划线、去 www  
、主域名和示例混合格式，例如 www_baidu_com.zip  
、www_baidu.com_com.tar.gz  
。默认后缀为 zip,tar.gz,tgz,rar,7z,bak,sql  
，可通过 -bext/--backup-ext  
 调整，并通过 -bmax/--backup-max  
 限制最大候选 URL 数量。  
  
修复 -sde auto  
 使用外部 ksubdomain  
 时，命令执行成功但未返回有效子域名会被误判为成功的问题。现在 ksubdomain  
 失败或零结果会给出网卡/权限排查提示，并在 auto  
 模式下自动回退 dnsx  
。  
  
调整 Interactsh 反连检测为默认关闭，默认排除反连模板；如需启用请使用 --interactsh  
，指定 --interactsh-server  
 或 --interactsh-token  
 也会自动启用。  
  
明确 -nsb/--no-subdomain-brute  
 的用途：该参数仅在开启 -sd  
 后生效，用于关闭主动子域名爆破，仅保留被动 subfinder 收集。  
## v1.0.1  
  
修复 -nt/--nuclei-template  
 指定外部 POC 目录时的模板源合并逻辑。外部 POC 现在作为额外模板源追加到内嵌模板基础上，重复文件名仍保持外部优先。  
  
启动时新增 Nuclei POC 加载统计，展示内嵌、外部和合并后的 POC 数量；指定外部目录时会单独输出对应目录的 POC 数。  
  
修正 -fy/--finger-yaml  
 等外部配置参数帮助文案，明确当前行为是追加/合并到内嵌配置，并在外部指纹加载失败或不存在时输出提示。  
  
Web 探针新增 301/302/303/307/308 跳转目标追加探测，跨域跳转后的页面会进入响应缓存和后续指纹识别流程，降低因跳转遗漏关键指纹的概率。  
## v1.0.0  
  
本次更新主要围绕 dddd-dl 实战使用体验、JSON 数据对接、跨平台编译和 GoPoc 协议未授权探测能力做增强。目标是尽量保持原版 dddd 的输出结构不变，同时补足实战中常见服务协议的检测覆盖。  
### JSON 输出兼容  
  
严格复刻原版 dddd 的 JSON Lines 输出格式，一行一个 JSON 对象，字段继续沿用 type  
、ip  
、ips  
、port  
、protocol  
、web  
、finger  
、domain  
、go_poc  
、uri  
、city  
、am  
、nuclei  
。  
  
修复 Nuclei 结果没有写入 JSON 输出的问题。命中 Nuclei 模板时会输出 type: "Nuclei"  
，并在 nuclei  
 字段中保留原始 ResultEvent JSON 字符串，便于后续系统对接。  
  
修复 -ot res.json  
 参数报错的问题。现在 -ot xxx.json  
 和 -ot xxx.jsonl  
 会自动兼容为 JSON 输出文件，等价于：  
  
./dddd -t 127.0.0.1 -ot json -o xxx.json  
  
保留原有标准用法：  
  
./dddd -t 127.0.0.1 -ot json -o result.json  
### HTML 输出控制  
  
调整 HTML 报告输出逻辑。未显式指定 -ho/--html-output  
 时不再自动生成时间戳 HTML 文件，避免在只需要 JSON 对接时产生额外 HTML 结果。  
### GoPoc 未授权探测增强  
  
新增以下协议的未授权或默认口令探测：  
<table><thead><tr><th style="background: #fafafa;font-weight: bold;color: #4a4a4a;border-bottom: 1px solid #f0f0f0;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">协议</span></section></th><th style="background: #fafafa;font-weight: bold;color: #4a4a4a;border-bottom: 1px solid #f0f0f0;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">默认端口</span></section></th><th style="background: #fafafa;font-weight: bold;color: #4a4a4a;border-bottom: 1px solid #f0f0f0;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">新增能力</span></section></th></tr></thead><tbody><tr><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">Elasticsearch</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">9200</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">检测 REST API 未授权访问</span></section></td></tr><tr><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">Zookeeper</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">2181</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">检测四字命令 </span><code style="background: #fafafa;padding: 2px 6px;border-radius: 4px;color: #333;font-size: 14px;border: 1px solid #eee;"><span leaf="">stat/ruok</span></code><span leaf=""> 未授权访问</span></section></td></tr><tr><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">MQTT</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">1883</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">检测匿名 CONNECT 连接</span></section></td></tr><tr><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">RabbitMQ</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">5672 / 15672</span></section></td><td style="border: 1px solid #f0f0f0;padding: 8px;color: #4a4a4a;font-size: 1em;font-family: KaiTi, &#34;楷体&#34;, serif;font-size: 16px;"><section><span leaf="">检测管理端未授权访问和 </span><code style="background: #fafafa;padding: 2px 6px;border-radius: 4px;color: #333;font-size: 14px;border: 1px solid #eee;"><span leaf="">guest/guest</span></code><span leaf=""> 默认口令</span></section></td></tr></tbody></table>  
新增 GoPoc 插件：  
  
Elasticsearch-Unauth  
Zookeeper-Unauth  
MQTT-Unauth  
RabbitMQ-Unauth  
RabbitMQ-WeakAuth  
  
所有新增协议结果均继续通过原有 GoPoc  
 输出结构写出，不增加新的 JSON 字段，便于和原版 dddd 数据格式保持一致。  
### 非默认端口协议对齐  
  
GoPoc 调度不再只依赖默认端口。协议识别阶段会同步保存服务 protocol  
 和 product  
 信息，GoPoc 分发时优先参考协议识别结果，识别不到时再回退默认端口。  
  
这样在实战中遇到非默认端口也能触发对应探测，例如：  
  
19200/http + Elasticsearch REST API -> Elasticsearch-Unauth  
12181/jute -> Zookeeper-Unauth  
1884/mqtt -> MQTT-Unauth  
25672/amqp -> RabbitMQ-Unauth  
  
默认端口列表补充 1883  
 和 5672  
，2181  
、9200  
、15672  
 已在默认扫描范围内。  
## 项目地址  
  
https://github.com/Yn8rt/DDDD-DL/releases  
访问 github 不方便的关注公众号后台回复dddl  
获取网盘🔗  
  
  
