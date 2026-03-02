#  漏洞组合拳与 JS 攻击面的博弈|Actuator 绕过、Shiro 反序列化、小程序后台全拿下  
一天要喝八杯水
                    一天要喝八杯水  渗透安全HackTwo   2026-03-01 16:01  
  
**0x01 前言**  
  
**在如今前后端分离的架构下，****JS 攻击面**  
早已成为渗透测试中最隐蔽、也最容易被忽视的突破口。从接口泄露、参数滥用到未授权访问，从单点漏洞到链式利用，挖洞早已不是单一技巧的比拼，而是**漏洞组合拳与攻击面的深度博弈**  
。  
  
本文不炫技、不依赖 0day，只靠真实 SRC 实战案例，带你复盘如何从 JS 文件、接口路径、参数逻辑中撕开缺口，再通过 Actuator 绕过、权限未授权、弱口令、信息泄露等漏洞打出组合拳。每一步都是可直接复用的挖洞思路，每一段都是对抗中的真实心得，献给每一位在细节里坚持的安全从业者。  
  
现在只对常读和星标的公众号才展示大图推送，建议大家把**渗透安全HackTwo“设为星标”，否则可能就看不到了啦！**  
  
参考文章  
：  
  
```
https://forum.butian.net/share/4619
https://www.hacktwohub.com/
```  
  
  
**末尾可领取挖洞资料/加圈子 #渗透安全HackTwo**  
  
**0x02 漏洞详情**  
### Actuator泄露403绕过  
  
又又又又又又是经典的不能再经典的登录框开局,习惯性的看看雪瞳接口老师傅可能已经锁定接口了/prod-api/  
接口,此接口是springboot  
端点泄露高频接口,往往在后面就放置了env  
 heapdump  
转储文件等  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAWhTGIFqIf6ejjNKpZtXGM7BTMQrIfiaJS7GsYfSNZFtrNGEL9927D73SZ1vfdUzibJvASicmvC47VP9G8hx8ZbZ85mxo1aa1qqro/640?wx_fmt=png&from=appmsg "null")  
  
dirserach  
全部回显403  
应该是做了Filter  
权限校验,如果是404  
肯定是路径错了比如/prod-api/  
前置或者后置需要新的这层, 那么我想办法如何绕过403  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXXiaGLjr0DpJlydQHOibGHLszvngmmoWlMeMAVmyGWj3pdia1yCIVX4mN7oCg1sBPcMqqeQvolibZSQPlRjlAETic4WPHsSopTfwxU/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUmKHhegvDnFbhUhgptkv3mkx61AhbkbEc2UJkpCypsicpHcibuQ82lBiaHP9pjxmpQeMB0JhA3xGMyZFibXD3hMwpUyyGvwVVibnqU/640?wx_fmt=png&from=appmsg "null")  
  
让actuator  
中的o  
编码为%6f  
，但常规url  
编码是不能对o  
进行编码的,他属于 "普通字符"（不在 URL  
 特殊字符集中，不会影响 URL  
 解析所以会原样输出），因此 **标准 URL 编码工具不会对 “o” 进行编码**  
，会直接保留为 o  
，而不是转成 %6F  
 ,我这里为了方便记忆才如此写下下,编码只会原样输出,正常情况下只能用ASCII  
 反推出值  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVU67vyrlwIVNjROH1brF4QPnrG1RsDrMXE1HOhaWMSKKcIVkCP4FhcM8bFgomTmA4WJ9GLQVyibFNIrAicYDibdibTQUN8Tn4asuo/640?wx_fmt=png&from=appmsg "null")  
  
%6f  
会解码为o  
  
%6F  
 是 **十六进制 ASCII 码的 URL 编码形式**  
（%  
 后跟着两位十六进制数，表示字符的 ASCII 码）。因为 6F  
 对应的十进制是 111  
，而 111  
 正是字符 "o" 的 ASCII  
 码，所以 URL  
 解码时，%6F  
 会被还原为 "o"  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAU4YuSa4FulAUHLOt12AB43k0KJicM5ATrxcic0HtrT9GfBF1vicEUNDpPbjkeuLibmDTRicEYrsZrFQRnzpgU5Jl4a1lac4ribqla0I/640?wx_fmt=png&from=appmsg "null")  
  
明白这一编码原理后又有读者要问了,主播主播**ASCII  
 **反推值又自己计算十进制再转换特别麻烦，有没有简单高效的方法,有的兄弟有的,把想要编码的值交给ai  
就完事了,通过回答可以知道r  
编码后为%72  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXVup1lvZ5dNicvNtqtrJkKpkHgpwAzboBN6xNvvjSFqL6XcxajsRG2gDaSicW7OIxgt4en3XIicBIzcQjyjAIV5rEC91uhu3oV5U/640?wx_fmt=png&from=appmsg "null")  
  
解码正确证明交给ai  
编码转换是可行的  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUMzqlnWmjk7QYITjXBJRItA4xGzfR6JichSBghj58fBbvj4nmIIYCDBwE4TaFVM8zt7643PFaGiauR1iaH8A2weZOcdVdiaPB8vFw/640?wx_fmt=png&from=appmsg "null")  
  
actuator  
中的o  
编码后就变成了actuat%6f  
r 那么这样携带去访问,成功绕过发现路径还有heapdump  
 env  
 nacosconfig  
 信息可以打开新的攻击面还有gateway  
路由更可以尝试是否可以路由rce  
, 后续访问其他接口详情也是需要在编码基础上访问,不然也是403  
```
https://xxxxxx.com.cn:8088/prod-api/actuat%6fr/  
https://xxxxxx.com.cn:8088/prod-api/actuat%6fr/env  ------ 成功访问
https://xxxxxx.com.cn:8088/prod-api/actuator/env  ------ 403
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUguaEGbwAA2m3jm0svWI6yHlKGrD27Vx44S87CibpaFl0ygF0OvmWq3VwxjLau9sPLaT4M74J9ezBnBZxTia9wWvOMOt2tVTgFs/640?wx_fmt=png&from=appmsg "")  
  
后面发现这款工具也可以帮助我们将字符进行ur  
编码推荐师傅下载使用,工具github  
搜索poc2jar  
即可  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXxWVjcGib9YibK8leQGMbZhPdjkDFGZmGrK9neD80ylS1j0sj3gARH3AIfiaYynEMIdVfKnnicxzD3IVY5DVLYIu0CnnjmXicLiaLic4/640?wx_fmt=png&from=appmsg "")  
### 小程序Host渗透后台  
  
小程序开局,一番测试并未发现问题,鉴权做的很安全,越权、注入、逻辑漏洞均未发现,随即转变思路,尝试是否存在web  
后台地址,打开新的攻击面  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVtP4fibBA9voqibTGFaQAo9f74c0tTxxsQVTKA5ErgoddykdNE7qrqY1Fohibmbr1sAZn9w7GpzS3vjO5cVAkaQgltWj1tfXLRpw/640?wx_fmt=png&from=appmsg "null")  
  
小程序一般会有后台管理页面,可能是其他子域名下放置的404页面 或者是小程序本身host  
域名其他目录下放置,这里首先尝试将host  
放置浏览器访问  
```
https://city.xxxxx.cc/
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWMfZoVZphvLcq0nYCCg7ewcNsfA7F0wl8MibjZO3YeLxcMwpmMSYEGMeXjiaftbVic0zicvk8LcEMSSkz6BXHdYmuZibd3C300Hruk/640?wx_fmt=png&from=appmsg "null")  
  
根路径正常回显那么开始上dirserach  
递归目录尝试能否发现后台地址  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWzXiaicTHRWG9pscDn8PU3o1FPhryNibhLjgv7Rh1NLZ1ib1OgH6WibNs3DuglOH0nJymtT5fNqjEOkyUNqc5rhOJLnaCqk3Oiaria0w/640?wx_fmt=png&from=appmsg "null")  
  
也是很幸运字典加持下不到二分半就得到有效地址,fuzz  
 出第一层前置目录manage  
 携带访问后直接跳转到后台地址,那么可以测试的点就多起来,弱口令未授权,JS  
接口等等  
```
https://city.xxxx/manage/------>
https://city.xxxxx/manage/login?redirect=/
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVMJjsXySIv6R04TibYeehIL1rjMYxL8zqQ4o1QxeTGNEX1OnfyoDgl5yTajkLFHoicZia9aSoCibMX3fbGjV9AXPOiaazBqxSKZGPE/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVaiaQaO7licEtZIo1sUNAr0g9OfRzvSVGJianhywwDPKm2ZDNwHsYkibDibFCSe5NrPDReBCwxswYA2NXr96AXxjEPD5FyDIVJd5PU/640?wx_fmt=png&from=appmsg "null")  
  
但是呢各位黑阔师傅们是否都会有先入为主的意识,我的目的就是为了找后台管理地址,现在我已经找到了,那么dirserach  
余下的会去完整的看完吗？域名下可能会设立不同的目录放置不同的登录口,耐心的看完所有信息至关重要,工具在这里又Fuzz  
出第二个前置目录,携带访问  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAWBmNxSHoZibjGTSlNOIicFTib7LjvibbFCcoNEr3jAFzB0ibwXoicVMrBdBIFxIQ0dvUxXaUzys38CrRo08RITM6eKl3egjllLrNAv8/640?wx_fmt=png&from=appmsg "null")  
  
同第一个地址一样,携带第一层后就会完整的调整到新的系统,在这里发现了第二个系统为后续渗透埋下伏笔,至此dirserch  
任务完成直接X掉  
```
https://city.xxxxxxxx/backend/----->
https://city.xxxxxx/backend/login?redirect=/
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAW8DjfLl5YFTpKzdYzEXha82lXXF11srcxpkxhR4vf0RoDZyh3Y7yQNX2BL3N7gb6sibXaRYkZrvuj3FMpSm2ic03rXUwy6icSsmU/640?wx_fmt=png&from=appmsg "null")  
  
回到第一个manage  
目录系统,纯粹的弱口令,后台地址藏在目录后往往也觉得这样很安全,密码设置也会相对简单,细致的信息收集找到入口漏洞往往已经呼之欲出了,查询小程序内所有用户实名信息三要素并控制余额  
```
admin/123456
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUwiaewibretWnbLoia7LvdGx0sdxtGGib1iaEIiabicia0fanlvhkSY5jZ8mibpbn4fMXM0CibV5zOA0gvpCXv6KdWJv58UBsg6KwYpK3aI/640?wx_fmt=png&from=appmsg "null")  
  
依旧是亲切的大头照  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXHXwg6MhibavKvmzib15nibM4fTRHBViaahNxib1STo3UJic8YtgWEeO3915mNCUe3HoV1rnK6VR5Dib3FBHOsWCyxBM1z2zWHHHgtOM/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUYziagWrnHEGupUY3SZ9Snv5kict4UV6UzKDt6b5fY9wrOibEjCtkFKu9nyqNAp8hYxmfJRZrYXtQZRAG7Ziaaz4ED870cniceb4mA/640?wx_fmt=png&from=appmsg "null")  
  
弱口令进后台还可以挖掘其他漏洞,更好的测试接口未授权问题,已经有了管理员权限那么调用所有一遍功能再把敏感信息接口删除token  
重发即可,当我测测时就发现了 ,第一个系统和第二个系统关联处,城市合伙人不就对应了第二个系统的名称嘛,账号大概率就是手机号了  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXLZDctymFkrjh3ax65wbSWmic3Z3ftM3ic1pkaRskZibkYDpYXbd60m7FtPXQu4oqJSaia5q9EX1dibbkCKIZrqn5Giay2m0cxiaSVoI/640?wx_fmt=png&from=appmsg "null")  
  
来到这里通过合伙人的账号配合初始第一个系统进入到弱密码,纯粹拿下第二个系统,渗透本质依旧是是信息收集  
```
手机号123456
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVEDVoX4PQj7rKLOd8B7pzvSVOEQ2a2RGZpHd7YDMHn5TZ6xfJ0NibyO2qZCQH7yX10vpLlK26gm7zCLGZVhU6n3MJoUg253lTI/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVZ8ibnekichzfPon5qU86ZU5Pmib3KljPHNToZNbFAerM7kicZhDZxBsDI9t8DficHraqajNLjrVcdnfb6V8Sgm2sWoxMMFF4aKghI/640?wx_fmt=png&from=appmsg "null")  
### Shrio721容器Rce  
  
渗透项目拿到大量子域名资产使用Ehole  
指纹识别发现Shiro  
框架,反手掏出压箱底的反序列化梭哈工具  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVffnDsiaLGMrXmFib0exwSfI34fsC19Hwm0cBlmSLFP9edzuhXXWoCHWVLaMsWiaGSLdoiapfE62XdDhv5mJ0joFHDxxYria6m1kfg/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVLDCx4UWdGGib28LxzZSicEqWkjic5hIfnceNQVjzQPpvwryLaicwFJuEgicFaBW1RvLhmH5XJ14Zek3SNERUxWgkaTVhVuIL8pUIw/640?wx_fmt=png&from=appmsg "null")  
  
填入地址马上给我爆破出aeskey  
 有种淡淡不详的预感,猜测可能是docker  
容器,如果爆破的时间最长一些我还不会这么觉得,地址填入不到2s  
完成给我感觉就是等着我来测呢  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUic3es1gJzwTXKxiatqKQSYq0znjqdPcLCGQhVQvEo5fuDSWL365bicv812MvscCyLK1QHRyHTFSUJTNZmRGCuZI3vQgzLq3BYwY/640?wx_fmt=png&from=appmsg "null")  
  
爆破一波利用链,如果是有key无链工具只能用JRMPClient  
分享其他师傅的文章工具可以去了解一下;这里出现了CommonsBeanutilsString  
这条链接,可以直接在功能区调用或者是生成内存马上线冰蝎  
  
Shiro无依赖链—Commons Beanutils_shiro 有key 无构造链  
  
https://cn-sec.com/archives/2673175.html  
  
检测非常规利用链工具：https://github.com/wyzxxz/shiro_rce_tool  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUceyGVf4vm143TqXoN9ZJTUicaKHD4kKPuOjlbUro0eCReYn5HHdZuPyLI1lGeL9QibTWebGicLIqPKEl9l8q6iaOib2MkF92liaibhc/640?wx_fmt=png&from=appmsg "null")  
  
为了确定是否是容器执行此命令看看初始进程,如 systemd  
、init  
 或容器中的主进程）所属的 cgroup  
 信息，如果系统运行在容器中比如Docker  
），路径中通常会包含容器 docker/abc12345  
 ，根据回显判断出了确实容器是容器开启的服务,哈基W......还是办不到吗....  
```
cat /proc/1/cgroup
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUStVvHZXWt9LLoA4jP65vKrDoX7SuUFLYDGIYAN2XpzibGXrFzHPePrCuVsI7aBP9IbbM1ibWS4AuLof1sqVyzLjJuSZNx3fKibQ/640?wx_fmt=png&from=appmsg "null")  
  
但是即使是容器不能逃逸并非没有利用价值,机器内网它可能会通其他内网网段,如172、198、10  
等典型的内网段,后续就是注入内存马冰蝎连接,但推荐各位师傅可以玩玩Vshell C2  
工具,代建正向反向代理都很方便,省去了frp  
等操作，权限够大搞还可以一键命令反弹上线,上线后交给队伍其他人员尝试内网刷分即可,地址和教程如下  
  
带web界面的C2工具vshell_4.9.3  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWEcL8lybcUVyDz0Cdneum76zpz4WaGunbibocSEq38tAicB7t3s82OXIWObRFF49vSeVPXWG2wc8nnWVxRn9Rs24oo8Wkbx3gAI/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXBNMsUmSwBKCX37ew5CweqRD5rM66SXfzeJicEMrqUoSQjyhT9iaXibHrKSwOSXbjuicfwkXdoGlRiaQCicGLkSObJr7moQSH8zAII8/640?wx_fmt=png&from=appmsg "null")  
  
有些shrio  
框架指纹识别工具并不能有效的扫描到,只有在我们对每个站点测试时通过响应和请求rerememberMe  
 字段发现,这无疑会错失很多,这里推荐一下P喵呜师傅提供的Shiroscan  
工具可以有效的识别出是否是shiro框架并进行shiro721  
测试  
```
https://github.com/pmiaowu/BurpShiroPassiveScan
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAV0UOh4D3I4ibRlbgguoCegAHNb6S6jTBTuxdWrLvoUxOtTTM18TrPeEWbdKBkNyJrFCCnAODg8dPIliawnX7Slb9H1zeNxNy23U/640?wx_fmt=png&from=appmsg "null")  
  
安装插件后在给出的配置文件内可以自己填充常见的Shirokey  
 进行补充,安装好后安心测站就行,时不时看下说不定会有意想不到的惊喜。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAX7aK8IOFh3FguNsJ9z2Km17uXE6M0nOj0ZIYC3L3tE6pqpYtnm85U7vcsl24vHFwkfjagUIDYoq9Wnpu7qickjcnkibLbZZYia0s/640?wx_fmt=png&from=appmsg "null")  
### Js同域名站点接口复用  
  
登录框Vue  
框架起手,先正常提取插件接口看看是否有信息泄露,无果可以再二次提取webpack  
接口  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUmUicRdDjwSX3cF1Ywicb1lNsWA2v0ibPglSNSicEbzmhby6LYSE9ibPEZ50ezJJsQibdXBX7fv1MqVI1tglZ0PsRlXytXnLwicJ4AcM/640?wx_fmt=png&from=appmsg "null")  
  
前端存在登录功能,调用接口后并经验应该是前置的目录api  
所以这里保留,直接放置接口作为payload  
进行爆破  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVhxwfHnshicibv0N2p7OfFt2EI9FhBgSyfoqtId7mm8o0icVBU99htNwBdMjLxEdQaQ6LrfWMHb8p8IEh8GPEIDch1dwtsyYAsC4/640?wx_fmt=png&from=appmsg "null")  
  
很可惜只有一处信息泄露但是这么大站点怎么会泄露这么二要素呢(姓名被unicode  
编码所以识别不出明文)  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVsha7uRcmPDw7nrlPhAxtGxZic9iciaaVB7fvkX2uhapppMibPmTvC2TG9icl2ibrfoyLMcg2ySd4TnwS0EAN0p8iaNIJEm1ISfAZJiaI/640?wx_fmt=png&from=appmsg "null")  
  
这么点信息够谁交啊,让人看到还以为我挖不到洞呢,直接上控制页面参数拿下全部的信息，将近7k  
也够交差了,这也是在接口泄露中提升危害的小技巧,至于具体应该跟上什么页码参数如果其他接口响应能返回一部分信息那么利用响应包的信息拿到请求包使用尝试是否可行,如果不可行只能是Fuzz  
 下面给出我常用的页码组合参数，平时在渗透过程中也可以自己记录并填充,形成自己的渗透字典  
```
/api/users/error_list ?page=1&limit=10000 
页码组合参数：
ET 
page=1&size=10000
page=1&page_size=100
pageSize-10&pageIndex=1&
pageNum=1&pageSize=10
POST
{"pageNum": "1","pageSize": "100"}
{"pageNo"1","pageSize":100,}
{"pn":1,"size":10}
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAX1upJEibNAxmGBNrFFiaytYLCpp4TCIDBaX3rpdq8hGyibS4QQmUoMmMlaNRSOyEia48S1YAq4ZltFYqjiaMotj78610ib9lLODufxk/640?wx_fmt=png&from=appmsg "null")  
  
常规JS  
并没有有效接口,那么最后一步提取chunk  
异步JS  
,也就只发现了一个有效接口所造成的信息泄露,和第一个接口一样泄露了二要素姓名电话还有一项家庭住址就去挖掘此域名下其他子域名了  
```
/api/test/test_user_list?page=1&limit=10000
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAU1mibkMYzO4ulF4hSIeb2mBTkIEohOsjkdKULrQXEm5PptpzfD9jSM9XdVdE9aC56t74q9QQcxou0ciaWACJXhSWefeiawM6Uials/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUjyibjjKBKAmIElXkMZPpweFt2ialia8a5B4VKcSQSSy1RCrUTBOvgsG3Tcknm0eUvJmXLmjPD3YkDCE6O88jKJ4tDPeZ7dDv1AY/640?wx_fmt=png&from=appmsg "null")  
  
hunter  
测绘到一处新站点,很明显二次一模一样,大家可以通过域名观察,第一个网站是我新发现的主域名站点,第二个网站test  
则是上文我进行挖掘的,那么可以进行最原始的利用方式了,同类似站点接口复用  
```
新发现站点: 根域名.com
已挖掘站点: test.xxxx.com
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUo1P1h6BficGHW2zscDHKibiacgRkLGC05XAQyBFic9y3qRsX6bWjvGV6G55ZBabmqnx7FPryicYN9oArGR0KwJdzet2Afmp4Bmu4c/640?wx_fmt=png&from=appmsg "null")  
  
复用倒是可以复用,但只有复用一个接口且经过处理信息量还是较少,不如第一个站点一根  
```
/api/test/test_user_list?page=1&limit=10000
# 复用成功
/api/users/error_list?page=1&limit=10000
# 复用失败
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVkMuqVrV0lVWIrt6Dv8WYkqFvObo9zQib3K6KJ3nNWeJKauSsjhEOuwPkmribb9cTEa7NoFiaGEnqFo8bXyBgYYwHniawPRY6C2JM/640?wx_fmt=png&from=appmsg "null")  
  
除了复用第一个站点接口,同时也要跟往常测站一样提当前站接口测试,但也只找到一个有效接口信息泄露也是量太少  
```
/api/users/search?page=1&limit=10000
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUhjCvYAqRiaepG4Iib0BtJLNRY5RO5iazDfgay4RVg4Zf8zFia3IybRmjy27l9jROPn9VOVXO9ZyUKSQNGne97TTibAibzMIpVOdrYI/640?wx_fmt=png&from=appmsg "null")  
### 业务功能注入+越权覆盖  
  
JS  
接口虽然是实用性较强的技巧,但是对于功能点挖掘也不容忽视,进入一处站点登录并注册,我会代理着BP  
大致浏览调用一整个页面的功能,然后在历史记录里稍微看看哪些接口可能会出现问题  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAULhaBqXsibjdfFBelXuSqwFHTQibedEaQrgn7u7iaIaULGicPbyiakbibYpIA2J9whGXHliaicD3RANnIyaHOQrc72XfiaKXKegpEOmmHw/640?wx_fmt=png&from=appmsg "null")  
  
熟悉业务结束后如果直接出漏洞当然最好不过,但没有也没关系,毕竟第一步也只是为了熟悉一下业务功能点而已,后续针对每个功能点发的每一个才应该去细细测试,在一处显示列表出发现tagId  
参数,加入单双引号都不行返回的都是500  
```
tagId=119
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAX3SWeXELx1cWgWX9WKicYBDpncSkQ8OBKJvOylKhvg4E0IPneuwicNibh5c5W2XBWWaC8FkZqBP6HHJLPl2sOMButhDJgvS6gialc/640?wx_fmt=png&from=appmsg "null")  
```
tagId=119'--+ -----> 500
tagId=119" --+ ----> 500
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUe8HibKS6iauXkffjlXiaVhDK1pW1MM5FvAMGlKqh9VGXQPa7tcpWNbpCicJIAdhNiaGOzwicorvwKUCz2miapjTSBtllxqUdTwES08c/640?wx_fmt=png&from=appmsg "null")  
  
字段行不通,尝试测试数字型,利用and  
两边都满足看看是否拼接到了数据库内,1=1  
正常返回  
```
tagId=119+and+1=1
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAX5trPHicGBqBOibEDlONv6ZRziaDtXjxMuxs9n1lwCBlMicg60DamkSOWgc3SstE7oBNLYcVibFdzx4jryw8NbBegKemEhib8sHMkLM/640?wx_fmt=png&from=appmsg "null")  
  
我们设定的条件为假所以肯定是不正常的,响应500  
不返回东西,确定了这个点肯定是有注入的,这种无回显情况除了典型的布尔测试,还可以利用延时函数等等  
```
tagId=119+and+1=2
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAU6yhiaWHPOGSBxKphcBJDfTdB01PX7gsrxomVBU1JrOtZPgrhGQXAG8cCQGGGCiaUbyDU3AM19OKwczm7miaGMrjsz92vayq0OGk/640?wx_fmt=png&from=appmsg "null")  
  
通过布尔注入在and  
后面利用if  
来判断出数据库的长度后续再判断具体的值,数据库长度确实大于某个数字则为真走exp  
报错,不是的话则是为假,正常返回  
```
tagId=119+and+if(length(schema())>7,exp(999),1)
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUgkiaENibFh6ibqrliaQmg1caTpxgBXDE3pBeliaXHAJ6pIfmia55lpXsVMHqnD5V7xsUXZ3ibmyDpt6SAb0mfiaPnibtVsCSTWvdk4G6Y/640?wx_fmt=png&from=appmsg "null")  
  
那么大于6  
呢.确实大于6 那么就为真,根据上面的信息,数据库长度是大于6  
小于8的,确定了数据库的长度就是7  
了（少截一张图,但确定了数据库的长度就是7）  
```
tagId=119+and+if(length(schema())=7,exp(999),1)
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWibO868jvibic583RQlu4lATTk66RZpwH1Bo7QgxwtkwibQKHia1tKrPnhNzQD9nKcnBu6GM7bicWsDf6msXtLbcla1qkcJOh7MpbIY/640?wx_fmt=png&from=appmsg "null")  
  
有了数据库长度再来测试具体的值substr  
函数截取数据库长度,从第一位开始,如果是a  
则是报错,不是则是返回正常,遍历26位字母和数字伴随其他标点符号,设置好两个payload  
位置这样可以快速出来,只跑7次就可以了,爆破的模式需要选择为集束炸弹  
```
tagId=119+and+if(substr(schema(),1,1)='a',exp(999),1)
tagId=119+and+if(substr(schema(), 爆破点 ,1)=' 爆破点 ',exp(999),1)
```  
  
  
最好成功的得出当前数据库名称为chatgpt  
 倒也符合这个站点业务基本都是ai  
生成相关  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAV3hLkBbZBcEOSDHnMrZ1NOSRo35icOwX0h6zfdHLQuQhcicvZgzvmRhDFObcib7aAOrpayzoasmJ0QEdibhdib1ehHtGKSsZKmVDnI/640?wx_fmt=png&from=appmsg "null")  
  
一个站点存在无任何过滤的注入,那么肯定不值一个注入,带着这样的想法继续测试;被动扫描显示modeIType  
参返回了数存在XPATH  
报错信息,这不典型的报错注入嘛,后续就是朴实无华跟上一首Payload  
成功爆出数据库名称,也没有任何WAF  
 ;有WAF  
的话主播也只是会简单的替换函数、注释、编码这样,高难度的绕过组合拳只能白嫖朋友套  
```
modeIType=video' and extractvalue(1,concat(char(126),database())) and '
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUfOfOqIj3cPI8G7e6f8ILM3PicmjJZaianpQKHOf6Ho3OPww6BsDlZDT8LEFlI5Mo3O8aKmodHVLGVZxIk2SwxTOJVmfDwFWXxY/640?wx_fmt=png&from=appmsg "null")  
```
%23 == #  
%0A == 换行
& == 连接符
%20 == 空格
version（）——> @@version 
hex()、bin() ==> ascii()
sleep() ==>benchmark() 
concat_ws()==>group_concat()
mid()、substr() ==> substring()
@@user  ==> user()  user()      system_user()   session_user() current_usercurrent_user()
database() schema() 数据库名
@@datadir  ==> datadir() 
1、十六进制绕过
eg：UNION SELECT 1,group_concat(column_name) from information_schema.columns where table_name=0x61645F6C696E6B
2、ascii 编码绕过
eg：Test =CHAR(101)+CHAR(97)+CHAR(115)+CHAR(116)
3、Unicode 编码
常用的几个符号的一些 Unicode 编码：
单引号: % u0027、% u02b9、% u02bc、% u02c8、% u2032、% uff07、% c0%27、% c0% a7、% e0%80% a7
空格：% u0020、% uff00、% c0%20、% c0% a0、% e0%80% a0
左括号：% u0028、% uff08、% c0%28、% c0% a8、% e0%80% a8
右括号：% u0029、% uff09、% c0%29、% c0% a9、% e0%80% a9
```  
  
往往一个站出现SQL  
注入就会伴随着多个,后面也是在此站点发现了多个注入,报错注入直接回显毕竟也是少数,大部分还是通过布尔页面利用页面回显不一样来判断,发现后和上文手住方法一样先测试出长度再遍历库名  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXHpfdEc6bRGoEAhHba8uz1xymibDbIyESSZk93OhsqnMdqqMzvrBMovEVt648bEjNiaSdAnl8TdEgr8piaibAo5yy5tOgcTjRicFL0/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAU98HYu6wCVPbRJbFLM85P6wJKAVVRibY6AiaeqqASAicLrLksaLfyoTTZlnQdjB0N98knYn0AWsXn85BPftibdaZL0erZSLW6uVw4/640?wx_fmt=png&from=appmsg "null")  
  
测试到头像上传功能点时,图片上传到存储桶,虽然无法打出一些webshell  
操作,但仍可以对存储桶进行列桶,htmlxss  
 、put  
上传等测试,不过这里我却发现了不寻常的地方,响应包上传后生成的图片名称回显在了请求包,经过测试上传文件名可控,并且下方目录也可控  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAW9YicQFSNibwdkRrP0tWf5VRRrFeDBdT39dc1HrIIyUxp0gAJFl8PsbTn7ibpM81J1QFnwyib8NoBlX5ZZB0ju8z7jS6TCXrHg1TI/640?wx_fmt=png&from=appmsg "null")  
  
数据包内出现文件名,经过大小号替换图片地址测试,可通过文件名覆盖对方同名上传文件,文件内容也由自己控制,但是这个漏洞利用较难,因为受害者的文件名是固定的,只要可以一直爆破原理上是肯定可以覆盖掉任意人的,但是利用难度较大没有意义  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAXS8hfQcpUkgiaGZpMr8YHnvRCBRODud5G1XBmdFrDScibuOUwBqX9QlSesjBK7MYOMypLzjls3fia0m8JTUCtsbL7GXOmvG2Mf0c/640?wx_fmt=png&from=appmsg "null")  
  
转而寻找可以带出用户信息的功能点,类似排行榜评论区这类,可惜一无所获,但是测试页面发现网站内的图片也是全部存储在存储桶的路径内,上传头像和网站图标只是目录不一致,但桶也是同一个桶  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVoSuWNQDYhyVZfbQmukRkUoHsicYGNgqkoamF65038v6vHJiaDJWsS4ps7y4WAvI77iaBItLs4aCxxY7cRsxllyLEbbZuGwDnibYw/640?wx_fmt=png&from=appmsg "null")  
  
保留下原始的svg  
 到本地为了测试后还原,不破坏网站；然后在头像上传处再次上传,将上传地址改为得到的图标目录地址  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWDYUVibIfqGodCkaR0x3qGicXEGROR1rLdkibvpE2aaA7yGiay3ibYW27t4vDibppbNopUQxChDecibOPcqicgDVdU7HdOHVWc3VkebCU/640?wx_fmt=png&from=appmsg "null")  
  
成功覆盖证明危害即可后面靠备份图片进行还原,如果写脚本爬到网站内所有图片名称,遍历上传也是可以覆盖全站图片的  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAX8rqlChBtCiaBlibSJW9cqe1hbkFCKxmG4dIXRPQSmgibib7dsicVoBicGJy9RRVLQtsWWaiaHoBJeGsa482y9M4Bet0GyS5iaOKEXibqM/640?wx_fmt=png&from=appmsg "null")  
### API接口组合拳Actuator  
  
Vue  
站点正常利用插件提取接口,正常跑跑看接口反馈什么情况再选择寻找前置目录还是进行Fuzz  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUHibLKctib5FmRpk3PXOOfJUB4rFA1n8E0DgtpUR8GZu0htxF7LO6jX3zal6kuhq2CeSjmhINBEcfy1JrUGqDENicWj9NcRHSEeg/640?wx_fmt=png&from=appmsg "null")  
  
发现全部重定向回到前台,那么盲猜会有前置目录情况  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAU1Qkic3SGVZChAl7mksJpkzhXSzFpOibA0zN0UqNoYy9N7vnsHNvXbVflokkjNVpBvhXfQC9rj0zVLek0EWibeCpr1jj3iaAMLVLU/640?wx_fmt=png&from=appmsg "null")  
  
老方法调用登录功能触发接口,依旧是熟悉的api  
 那么尝试固定此为前置目录在这个基础上爆破接口,GET POST  
均可以尝试  
```
/api/sys/login
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAWwZTW3pnlZdFXOoqfq50SMFUDP7vO1aMOQmB3YJQQZCBvY3hh2Mn0lRH6PDa1c9zJB6nnVQmibRLygurpg64g1mumQOL3OCH9k/640?wx_fmt=png&from=appmsg "null")  
  
如此操作后虽然没有和第一次一样重定向会首页,但接口怎么看着这么奇怪呢一半响应401  
缺少token  
余下一半报错从上游服务器收到无效响应,继续在接口方面进行测试,最终通过两处信息让我确定接口处理有误  
```
/api/gcwms-boot/packing/cylinder/basCylinder/list
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAXrm3eaM7Nvfic6klYTRBYzEjapn5yTud0KC04X9HnzypmD242TB7wKmJrN7w5QL0legXOMzCD2iaWW1lxOATjDpk90qJzTtovW0/640?wx_fmt=png&from=appmsg "null")  
  
上文我提到测试前置目录我一般会进行调用功能点的操作,我得到的接口全称是如下,那么我将插件接口放到一处记事本,搜索后置接口/sys/login/  
是否被我插件所提取,这个插件是我确定不了前置接口的惯用操作,只要登录口可以调用功能对比接口快速确定  
```
/api/sys/login
```  
  
搜索后置接口发现确实被插件所提取,但是前置的目录却是/gcwms-boot/  
那么不难猜出这个/gcwms-boot/  
接口对应的正是/api  
 想要正确的爆破需要将提取到的接口存在/gcwms-boot/  
的地方全部替换为api  
```
/gcwms-boot/sys/login
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAW9moZFsn2c5WUR4XVqm4esAbD0VIPFVfoNEYOnBA5n1hVfS6UfMod5c5o0nfiauBKic4ibfEPVPYhELib1D8ibLOpA6SiaMuF02oDv0/640?wx_fmt=png&from=appmsg "null")  
  
并且像这种前后端架构出现了Vue  
框架那么基本上可以确定为Vue+Springboot  
开发,我也习惯会去利用dirserach  
字典爆破,或者利用Onescan  
 TsojanScan  
等被动扫描插件辅助测试,在这个场景下插件就帮我扫描出了经典的actuator  
端点泄露以及swagger  
 druid  
等捡漏三件套  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAV0xcqS4LLbsOVYQ5Gans638gmukpfloicGyo4QPKQ4pZOEUEia4BKTCpCuR6ViagZlSeYl0KgEY5aqcFKD4cnZn1bv2JWuY5fqoM/640?wx_fmt=png&from=appmsg "null")  
  
当我浏览器访问actuator  
端口目录时,虽然没有出现env  
 heapdump  
等接口,但观察到页面显示的第一层目录为gcwms-boot  
但是浏览器对应的是api  
 既然我让gcwms-boot  
作为第一层目录会直接重定向会首页,那么全局修改提取到的接口,将前置目录修改为api  
再测试  
```
/gcwms-boot/actuator
/api/actuator/
gcwms-boot=api
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWl8oIMVobIL6ULh5juDD2icnzocusIlowXBaYia0Y0yg7dezZk89EllibwzAutWiae2fdAjV2TS3dulOCazqkicjN1g7hdIia71qw6c/640?wx_fmt=png&from=appmsg "null")  
  
全局搜索替换gcwms-boot  
为api  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVG9jF1gf5pkahUiavUeIJkWyWH2xz7G17lj0K7Q28XspHECylpYOIaVfMsTiaG8iargVfuaPCHzJAgibas2kuKsFxrkvp2r0ySnib0/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVU6mia0WI5MbdKkFrJz8SylFOrSLvrZyBrhcIFjcdYSKv6YgNCiaMSf9P4jictUicib9Hdmta6WrH60U4gcp4HFI5oHPkEAyeug6xs/640?wx_fmt=png&from=appmsg "null")  
  
接口全部正常响应没有出现502  
情况并且也出现了一些不痛不痒的配置信息,证明上文我的处理方式是正常的,但是大量接口响应401  
后端存在鉴权,无法获取有价值信息,依旧办不到。。。  
```
https://xxxxx.com/api/bas/basWarehouse/childList
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAXYdmddD6nDbtSgERooYEbOic74hyjRnQdRP6cKeEVPxofZMvPtZPGcmicRX6PW0b8ibpBpCUSmeFp06NN7kQ6hT94HBUec0VyP0E/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVvUOfjUUF5DSwLsG7bwYibTlHe3esR7WU9uhlhLXedSejicu2lDOEM5OGUHNjNV8cz49TIt1nuCOqRLMsHRtAE86ibo7P1eXyxibI/640?wx_fmt=png&from=appmsg "null")  
  
大家基本上都主要是关注env  
、heapdump  
，还有容易造成命令执行的、gateway  
、hystrix.stream  
、jolokia  
等等，httptrace  
和logfile  
 这类记录信息的接口却很容易被忽视，通常这二次不会一起出现,包括这里也只出现了httptrace  
,简单来说会记录登录鉴权信息,JWT cookie  
等  
  
httptrace  
可以记录每一个HTTP请求的信息，包括请求路径、请求参数、响应状态、返回参数、请求耗时等信息。一般会随着SpringBoot Actuator  
未授权同时出现  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUJLznF9RTTQ6KIW2sUrdnMVXqLJEYyg29afCnBrcc6W2gicykWsHkwX1X3kWpgV3nwkEGyNJxYB3SHGuKcic1c2aTd14MILwPr8/640?wx_fmt=png&from=appmsg "null")  
  
鉴权可能会过期所以下拉找到时间段最新的JWT  
```
x-access-token:eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE3NjA5Mjk4MjAsInVzZXJuYW1lIjoiYWRtaW4ifQ.PuGa-Fo5o7N0mH2Dxmllov3XpLyJX5oDNGrJT-
8fkz8
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWoG2AVMMIypQrj51PiaYN7BfEXtHCdatoTuWmCM1URwXTOjtKwEwvmTzIiaRapZSJnZ68tSPicSsry7VXsx7NUVrmB2cLgZAsJUk/640?wx_fmt=png&from=appmsg "null")  
  
携带token  
后再次遍历应该处理的接口,出现大量业务数据泄露,伴随很多xls  
表格记录着联系人列表供应商信息等等,拿下业务数据证明危害,已删除记录  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXBib0QZkp2h42AmsUpkicaGicCYXfo0LLnSxLb4sbRoMyUtKmF4Gp85rZvEgNOFz9S7icXb1yVC6DneiasnWJNPYPU1LtxzXF2aAOY/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAXAcFT3dBicPicibMfpXtmprcmd0818Q9bU22AUNbQbKrzNL74ST8y7OGqpibrvcmvnpiad19uPFzaUxlWSweibu1mBwRAiayW228u8hA/640?wx_fmt=png&from=appmsg "null")  
  
如在攻防中,如演练手册信息泄露并不局限于公民三要素,那么通过JS  
接口拿下更多的业务数据也是一个不错的选择,各位师傅不要放过这些点  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXtmejgeezsushpAkpXTtPgwxiaF6TuEZJtRwKjjn4XxNoKy3b1rc72KpF7a7GicuBibUMHib9jdWXgPpg3Hlicgg4ApmYP6ntMUibnY/640?wx_fmt=png&from=appmsg "null")  
### 民生业务信息收集思路  
  
某一众测项目,项目是某一城市水务公司,在web信息收集一圈并未发现比较好的目标,大多需要统一认证账号,转而去向业务点更丰富的小程序资产,但是即使我利用微信登录拥有了账号后,但还需要关键的居民用水账号,否则无法测试功能点,自此便开始了互联网信息收集之旅  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVosvGCeVrdjI8ygrfVLMZJeMk97XImic1voBib6fPFxncburlwpibNt9oXnoicibhEibXO2sKymJGZgrCfF2Xd71IZZiadZVTzCtj5PM/640?wx_fmt=png&from=appmsg "null")  
  
在小红书亦或者抖音等社交媒体发帖平台,检索对应的关键字可得到相关信息,关键字偏实用性这样检索更为精确,后续就是在众多的帖子细细发现是否出现了户号、姓名等  
```
xxx水费
xxxx水费缴纳
xxxxx水费太贵了
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAW4M5C1rzYib3ZkchjlQQEoFGicMMib7giaXjwlM9jygUiasoSrOePlanXoKZdVE4SpheWxqrBwP8ibuIkTbfhuK12DBntjibgJsv2mpM/640?wx_fmt=png&from=appmsg "null")  
  
不得不说现在的网友网络安全意识是越来越好了,基本都会把关键信息打码,这也为我们的信息收集增加了一点难度,但肯定会有漏网之鱼  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUZUV8M9XkibdgfiaNDK8UibC8EEH9MibvNPiayMPLvwWuaQsDIrycB5eibT9KrAUOKEG6O0Q5zvUQraFXQDzVF7Gsb8oljsunFZQ6Xs/640?wx_fmt=png&from=appmsg "null")  
  
应该长达两分半的小红书冲浪也是找到了一处比较全的信息,里面包含了户号还有后两位姓名等信息,但是姓名不全,接下来就是的单方面社工,浏览他的个人账号帖子、评论区、其他社交媒体,最终通过一个没打码的手机号配合sgk  
利用lm  
控制地区检索得到完整的姓名电话sfz  
三要素,各位师傅懂的都懂，不懂的慢慢学到后面也就懂了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWmfuvqG3iaQfrzU4He28jW5cUCAEOhlaZtLMaONEx4ecRBvdsKS5owSYesibyoDsicCWTqoQmqjYt1nvapP1o2m1TOBvaHBC821Q/640?wx_fmt=png&from=appmsg "null")  
  
凡事涉及大量居民日常必备的业务,如水务、电力、医院等行业均可以利用互联网公开资源进行信息收集,包括刚入门的师傅热衷于挖掘教育edu  
,见惯了谷歌语法学号密码，为何不试试看在抖音、小红书尝试检索xxx  
学校学生证掉了呢,这里我只起抛砖引玉的作用,思路有多好危害就有多大。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXE7sLWfibib7aBECBDRbjBopOUKgfSaCuwpN9mu3Oz7JEUf86slxPW1Qx7SjlSx51KTNkanmoBvCKmweODMDxXSEWWfxbFkYlf8/640?wx_fmt=png&from=appmsg "null")  
  
通过信息收集拿到完整的户号姓名后也是顺利绑定到了账号,测试到了几处越权也不算白费功夫,不要倒在信息收集这一步,有账号和没账号完全是天壤之别，如果身边有熟人那自然最好不过直接给个相关账号就行,如果没有只能老老实实信息收集  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAUibwFgfJ9UsL5QuqwxIom62MXiczUzHxJ85583YujQR9pc0u3uCcccEibs1Bib3OREGHR6sWY2gmMInaqP9SiaUGXBY0YEO7mzkxuc/640?wx_fmt=png&from=appmsg "null")  
  
****  
  
**0x03总结**  
  
挖洞从无华而不实的屠龙术，不过是在无数次发包与重试中，等到那一次关键突破。点滴坚持，终会在某个瞬间给你惊喜。本文以轻松实战的口吻，分享真实漏洞挖掘思路，愿大家看得开心、学得实用。祝各位师傅在 SRC 路上稳扎稳打，洞见高危、收获满满，一路顺利不踩坑！！🔥  
喜欢这类文章或挖掘SRC技巧文章师傅可以点赞转发支持一下谢谢！  
  
  
**内部星球VIP介绍V1.4（更多未公开挖洞技术欢迎加入星球）**  
  
  
**如果你想学习更多另类渗透SRC挖洞技术/攻防/免杀/应急溯源/赏金赚取/工作内推，欢迎加入我们内部星球可获得内部工具字典和享受内部资源/内部群🔥**  
  
🚀  
1.每周更新1day/0day漏洞刷分上分，目前已更新至5294+;  
  
🧰  
2.包含网上的各种付费工具/各种Burp  
漏洞检测插件  
/  
fuzz字典  
等等;  
  
🧩  
3.Fofa/Ctfshow/360Quake/Shadon/零零信安/Zoomeye各种账号高级VIP会员共享等等;  
  
🎥  
4.最新SRC挖掘/红队/代审/免杀/逆向视频资源等等;  
  
🧪  
5.内部自动化漏扫赚赏金捡洞工具，免杀CS/Webshell工具等等;  
  
🎯  
6.最新0Day1Day漏洞POC/EXP分享地址（同步更新）;  
  
https://t.zsxq.com/KcKlh  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq7cpyLKdrg9g0QRpvkvBaQicGaNoLjesB2YZuPd6RMJOmxdwV8BBcEX8hjsT3OL6DuGpSedCkl2Mhw/640?wx_fmt=png&from=appmsg "")  
  
🔥  
7.详情直接点击下方链接进入了解，后台回复"   
星球  
 "获取优惠先到先得！后续资源会更丰富在加入还是低价！（即将涨价）以上仅介绍部分内容还没完！**点击下方地址全面了解👇🏻**  
  
  
**👉****点击了解加入-->>2026内部VIP星球福利介绍V1.5版本-1day/0day漏洞库及内部资源更新**  
  
  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
回复“**app**  
" 获取  app渗透和app抓包教程  
  
回复“**渗透字典**  
" 获取 一些字典已重新划分处理**（需要内部专属fuzz字典可加入星球获取，内部字典多年积累整理好用！持续整理中！）**  
  
回复“**书籍**  
" 获取 网络安全相关经典书籍电子版pdf  
  
# 最后必看  
  
  
      
文章中的案例或工具仅面向合法授权的企业安全建设行为，如您需要测试内容的可用性，请自行搭建靶机环境，勿用于非法行为。如  
用于其他用途，由使用者承担全部法律及连带责任，与作者和本公众号无关。  
本项目所有收录的poc均为漏洞的理论判断，不存在漏洞利用过程，不会对目标发起真实攻击和漏洞利用。文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用。  
如您在使用本工具或阅读文章的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。本工具或文章或来源于网络，若有侵权请联系作者删除，请在24小时内删除，请勿用于商业行为，自行查验是否具有后门，切勿相信软件内的广告！  
  
  
  
  
  
# 往期推荐  
  
  
**1.内部VIP知识星球福利介绍V1.5版本0day推送**  
  
**2.最新Nessus2026.2.9版本下载**  
  
**3.最新BurpSuite2026.1.1专业版下载**  
  
**4.最新xray1.9.11高级版下载Windows/Linux**  
  
**5.最新HCL AppScan_Standard_10.9.1下载**  
  
渗透安全HackTwo  
  
微信号：关注公众号获取  
  
后台回复星球加入：  
知识星球  
  
扫码关注 了解更多  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6qFFAxdkV2tgPPqL76yNTw38UJ9vr5QJQE48ff1I4Gichw7adAcHQx8ePBPmwvouAhs4ArJFVdKkw/640?wx_fmt=png "二维码")  
  
  
上一篇文章：  
[Nacos配置文件攻防思路总结|揭秘Nacos被低估的攻击面](https://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247492839&idx=1&sn=b6f091114fbd8e8922153a996c8f4f1c&scene=21#wechat_redirect)  
  
  
喜欢的师傅可以点赞转发支持一下  
  
