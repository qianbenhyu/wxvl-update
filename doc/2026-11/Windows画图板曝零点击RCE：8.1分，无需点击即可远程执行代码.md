#  Windows画图板曝零点击RCE：8.1分，无需点击即可远程执行代码  
原创 Violet Walker
                    Violet Walker  黑白之道   2026-11-08 01:09  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/nGzNudUIJ6O5wDeMOicfCPW2VHrSXmU8cofTcn7Yib49wyhJKUS1icCTQKUibMVglnv6eBpib9Hq1RGHuPebSPlLiaQGAEJibfKXVWo8AIiaxr4YdNE/640?from=appmsg "")  
> **导语**  
：Windows自带的画图板，也能成为远程代码执行的入口。微软10月补丁日披露CVE-2026-83844，一个零点击RCE漏洞；修复补丁只走Microsoft Store，很多机器至今没收到。  
  
## 一、画图板成了入口  
  
2026年10月13日，微软在月度补丁日里夹进一条不起眼的安全公告：Windows Paint存在远程代码执行漏洞。编号CVE-2026-83844，根因是越界读（CWE-125），CVSS 3.1评分8.1，属"重要"级别，向量AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H  
。  
  
说到画图板，很多人想到的还是Windows 95/XP时代那个老工具。它当年还有个著名彩蛋：画完"不礼貌"的图再打印，系统会弹一句"你的画是坨屎，现在终止会话"。就是这么个连玩笑都开的应用，今天成了零点击RCE的入口。  
  
![Windows经典Paint彩蛋](https://mmbiz.qpic.cn/sz_mmbiz_png/nGzNudUIJ6PRNB9iclPBic4XSSMv0sTcEmFh5gX0cCibTCylyOuLHBKZnBxvtTDTfvjU2ibETvXib635A2Kib0nTK93KIHOsW0IA43n4lM5Cx1xzk/640?from=appmsg "Windows经典Paint彩蛋")  
  
（图源：X推文@RachelTobac引用的演示视频，Windows经典Paint彩蛋）  
  
拆开看，信息量很大：攻击走网络（AV:N）、无需权限（PR:N）、**无需用户交互（UI:N）**  
，保密性、完整性、可用性三个维度全是"高"。翻译成人话——攻击者可能在你毫无操作的情况下，借画图板在机器上跑代码。  
  
不过微软留了个"但是"：攻击复杂度是"高"（AC:H）。官方FAQ解释得直白，成功利用需要攻击者深刻理解目标系统，能否奏效还看环境、配置和额外防护，并不保证成功。而且微软始终没公布是哪个文件格式、哪条解析路径出了毛病，只给了句通用措辞"向受影响服务发送特制数据包"。  
  
安全研究者@0x_exit的演示动画，意思大致如此：  
  
![画图板零点击RCE演示画面](https://mmbiz.qpic.cn/sz_mmbiz_png/nGzNudUIJ6O3ia4ITT8hk78UwWgxrJQ0FHOVOAaqxlcL4UYptiag81sPl5wFWiafEic5uweqzrxyicibWe3ul91icgdZ0Yibe7UYxK02I9B1C3a6hiao/640?from=appmsg "画图板零点击RCE演示画面")  
  
（图源：X推文@0x_exit附带的演示视频画面，示意，非真实利用录屏）  
## 二、一个漏洞，三个版本号口径  
  
真正让这事从"又一个CVE"变成话题的，是版本号对不上的乱象。关于"修到哪个版本才算安全"，市面上有三个打架的口径：  
<table><thead><tr><th style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;font-weight: bold;background-color: rgb(255, 249, 249);color: rgb(239, 112, 96);"><section><span leaf="">来源</span></section></th><th style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;font-weight: bold;background-color: rgb(255, 249, 249);color: rgb(239, 112, 96);"><section><span leaf="">给出的修复版本</span></section></th></tr></thead><tbody><tr><td style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;"><section><span leaf="">CVE 记录 / NVD</span></section></td><td style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;"><section><span leaf="">11.2605.91.0 起视为已修复</span></section></td></tr><tr><td style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;"><section><span leaf="">微软 Security Update Guide</span></section></td><td style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;"><section><span leaf="">11.2609.401.0</span></section></td></tr><tr><td style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;"><section><span leaf="">用户/管理员实测</span></section></td><td style="font-size: 0.75em;padding: 9px 12px;line-height: 22px;border: 1px solid rgb(239, 112, 96);vertical-align: top;"><section><span leaf="">系统里往往还是 11.2603.251.0 或 11.2605.81.0</span></section></td></tr></tbody></table>  
麻烦在于，扫描器通常照着CVE记录判定。按它的口径，11.2605.91.0以上都算"已修复"，于是11.2605.91.0到11.2609.401.0之间整段版本的机器，在扫描器眼里"假性安全"，实际还带洞。有网友在微软问答论坛指出，上周公告给的修复版还是11.2605.91.0——这数字被悄悄改过，修订历史里却仍只有10月13日那一条，没有改动说明。  
## 三、补丁不走 Windows Update  
  
比版本号更头疼的是发货渠道。画图板早已从系统组件变成Microsoft Store应用，更新独立于Windows累积更新——你装了10月系统补丁，不等于画图板也补上了。它只通过Store的自动更新下发。  
  
设备管理软件厂商RFF的Brian Vitko这周抽查六国Store上架版本，最新版仍是11.2605.81.0，一个没修复的版本。他的测试虚拟机后来自己默默更新到了修复版，但那说明不了什么，别的机器多久能收到，没人知道。Windows Latest也在自家系统查到画图板停在11.2603.251.0，同样没补。  
  
更隐蔽的是，Store应用按用户账户分别安装，Windows还会留一份provisioned副本给新账户。一台机器可能躺着好几个不同版本的画图板，只查当前登录账户，根本发现不了另外几份老版本。部分杀软已据此把画图板标为"未打补丁"，但到底是分发问题还是画图板自身问题，微软至今没给说法。  
## 四、现在该做的三件事  
  
官方公告没给任何临时缓解措施，能做的就剩"确认版本、管好资产、能删就删"。  
  
第一步，确认每一个用户副本的版本。管理员PowerShell跑这两条命令，分别列出所有用户副本和provisioned副本：  
```
Get-AppxPackage -AllUsers -Name Microsoft.Paint | Select-Object Version, @{ n = 'Users'; e = { $_.PackageUserInformation.UserSecurityId.Username -join ', ' } }Get-AppxProvisionedPackage -Online | Where-Object DisplayName -eq 'Microsoft.Paint' | Select-Object Version
```  
  
把版本号跟11.2609.401.0比对，低于它就走Store手动更新；普通用户也可在"设置→应用→画图板→高级选项"里看版本。  
  
第二步，把画图板纳入资产清单。别再问"有没有打补丁"，改问"每个用户副本的版本是多少"——这次最值得记的一课，就是这种藏在Store管道里的应用，很容易从安全团队的资产清单里整体消失。  
  
第三步，用不到就卸掉。画图板在Windows 11 24H2起已可卸载，不需要的机器直接移除，或用AppLocker、WDAC、Store策略禁掉。  
  
这条消息最早由PCWorld报道出来，标题很直白——"你的电脑可能错过了这个补丁"：  
  
![PCWorld 报道截图](https://mmbiz.qpic.cn/sz_mmbiz_png/nGzNudUIJ6OYiafp25CIh9ic5pNhpwQB66micDtsQMojBSaLLF227ACCGy0KPUZMKr0RkR6C9pUUhic5RRoFwIIiawblKEZgqLqJN4nmEv3Dco28/640?from=appmsg "PCWorld 报道截图")  
  
（图源：PCWorld）  
## 五、紫队视角：胜负手不在漏洞，在分发  
  
这事儿得从两边儿看。  
  
红队为什么盯上画图板这种"人畜无害"的老应用？因为它几乎每台Windows机器都有，却在安全团队眼皮子底下隐形——EDR和SIEM很少给画图工具设告警规则。而且这洞不需要钓鱼、不需要诱导点击，攻击面就成了"只要机器装了画图板、又能处理到不可信内容"。AC:H的高门槛说明，真正的难度不在写利用，而在选对目标、搭对触发链路。  
  
蓝队这边，检测反而不是最难的——真出事，画图板异常起子进程、读写不该碰的内存，总能被看到。真正卡住的是分发：补丁走了Store这条和Windows Update完全并行的管道，很多企业根本不知道要单独管；再叠上per-user多副本和CVE记录的版本口径漂移，最后就是"公告说修了、扫描器说安全了，机器其实还带洞"。  
  
这就是红蓝协作最该记的一笔：一条漏洞的生命周期，从"公告"到"资产清单"再到"分发"最后到"验证"，这次恰恰断在第三环。修复从来不等于缓解，CVE记录说"已修复"，也从来不等于你的机器真的修好了。  
  
**版权声明**  
：本文由华盟网原创发布，保留所有权利。封面配图由华盟网生成并授权使用，其余配图图源见各图下方标注。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/nGzNudUIJ6PjVrLdDkMfUxNsZtTV0A1hmwp3gg0hlszFOVoFrGobZ4iaB1yG3R9xWBptrWTibjLU9zqUnF3HicePpic0yRRFUHmkV3nH4KeahXw/640?from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/nGzNudUIJ6NUVOROc8xkmwSureVaKjib6gm81BKEvEwicx7RUBaWBc9b6tXdKpJa7q3PgfB3f3C3bbZxhSibWlMjhKxyhI8zrECLxdWwZ1Sq9Q/640?from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/nGzNudUIJ6PhFDakV6xatOtlZbZcVtibQjic3niajEt9HGtqHDbp3YV1BqZP5ZnFicEJUsexppicBkiacmp1gPcDcyd6Xca4v8BqMCl42zoaricsPY/640?from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/nGzNudUIJ6PGyNMfAa4yNLwHd5ymn32QIEFNYib0Q7CBJCvHQib0ib1hgLaiaIgSbpp9YXO4m2HWY7ib317LyWOJibV303Hoa6dcMsNVTLa1Gxux8/640?from=appmsg "")  
> 👇 点击**阅读原文**  
，访问我的网站  
  
  
