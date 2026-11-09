#  漏洞复现 | 用友NC qrySubPurchaseOrgByParentPk 存在sql注入漏洞  
 实战安全研究   2026-11-08 02:00  
  
**免责声明**  
<table><tbody><tr style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><td valign="top" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;visibility: visible;"><span style="color: rgb(255, 0, 0);letter-spacing: 0.544px;-webkit-tap-highlight-color: rgba(0, 0, 0, 0);outline: 0px;visibility: visible;font-size: 14px;"><span leaf="">本文仅用于技术学习和安全研究，请勿使用本文所提供的内容及相关技术从事非法活动，由于传播和利用此文所提供的内容或工具而造成任何直接或间接的损失后果，均由使用者本人承担，所产生一切不良后果与文章作者及本账号无关。如内容有争议或侵权，请私信我们！我们会立即删除并致歉。谢谢！</span></span></td></tr></tbody></table>  
1  
  
**漏洞描述**  
  
  
  
用友NC qrySubPurchaseOrgByParentPk 存在SQL注入漏洞，攻击者通过恶意SQL语句绕过验证，窃取、篡改或删除数据库敏感数据，甚至获取服务器控制权限，导致数据泄露、业务瘫痪或进一步渗透内网。  
  
2  
  
**影响版本**  
  
  
  
   
用友NC  
  
3  
  
**测绘语法**  
  
  
  
fofa语法  
```
app="用友-UFIDA-NC"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yIciaKAicYtopsC1CibpFGxGEq6HiayBuDRuwV7a3xp85oMCUjWXZMkJ4iaDdRqO8cg39SO3aNaPuwzdMS74rZR5gc5mo0mcbq1qd2mFPDF1NAtk/640?wx_fmt=png&from=appmsg "")  
  
  
4  
  
**漏洞复现**  
  
  
  
注入  
  
![](https://mmbiz.qpic.cn/mmbiz_png/yIciaKAicYtorV3KlTlN7icX3NUcfLuibTMRSqibH4GwABHCmpJQxCsdYibQ2B6RbkyRNqiaBLrhQGjmkJA2rflQAPtGDibLw6GJtRWicvbeDrcuOLzk/640?wx_fmt=png&from=appmsg "")  
  
  
5  
  
**检测POC**  
  
  
  
nuclei  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yIciaKAicYtopZ0anPicBTjc8VOciao8LmPg4IIk7pCEH0ic23lH4lhEM57D3BQ97hsOAetHWSTU5lCPLKG2Ze6ic8dUR2l5JrzXen2keAwqTicy44/640?wx_fmt=png&from=appmsg "")  
  
afrog  
  
![](https://mmbiz.qpic.cn/mmbiz_png/yIciaKAicYtoriaA1de49QWCw4bSg5vVxC3dyIiasAqoadVPQuwXFwFGDJzaOAvMia0Cerj8ibibWib70e9ticyI0Ggvpb2Q9Ic0yoBKCakWBxOlibU1w/640?wx_fmt=png&from=appmsg "")  
  
  
6  
  
**漏洞修复**  
  
  
1、建议联系厂商打补丁或升级版本。  
  
2、增加Web应用防火墙防护。  
  
3、关闭互联网暴露面或接口设置访问权限。  
  
7  
  
**内部圈子**  
  
  
**现在已更新POC数量 2500+（中危以上）**  
  
🔥 **1day/Nday 漏洞实战圈上线**  
 🔥  
  
还在到处找公开漏洞 POC？  
  
这里专注整合全网公开1day/Nday漏洞POC和复现，一站式解决你的痛点！  
  
🔍   
圈子福利  
  
✅ 整合全网 1day/Nday 漏洞POC，附带复现步骤，新手也能快速上手  
  
✅ 每周更新 7-15 个POC测试脚本，经过实测验证，到手就能用  
  
✅ 完美适配 Nuclei/Afrog 扫描工具，脚本无需额外修改，即拿即用  
  
✅ 临时福利：免费 FOFA 高级会员查询，无需账号也能高效资产测绘  
  
✅ 专属权益：提供指纹识别库，指纹库持续更新  
  
💡   
适合对象  
  
渗透测试🔹攻防演练🔹安全运维🔹企业自查  
🔹SRC漏洞挖掘  
  
⚠️   
重要提醒  
  
仅限授权范围内的合法安全测试，严禁用于未授权攻击行为！  
  
本服务为虚拟资源服务，一经购买概不退款，请按需谨慎购买！  
  
目前圈子已满200人，价格由66.9调整为69.9元（  
交个朋友啦  
），250人后调整为71.9元。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/yIciaKAicYtooOcDoeEYZtETDFQ4rjVvy92CZjibia4I6kmdiakLGDf8DbibXuyXo44ic8YGePNBib59bWUHegCYsgPATaVUw8r11f9B7uzwfczOyt4/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
