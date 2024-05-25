<div align="center">

<h1>China Internationally-connected</h1>
中国的国际网络互联状态介绍</br></br>

[**速查表格**](./oneman.md) | [**精简版**](./mjj.md)

</div>



## 部分简写对照
|   中文全称  |  简写 |     英文全称   |
|   ---       |---  |       ---      |
|  中国联通   | CU  | China Unicom  |
|  中国移动   | CM  | China Mobile  |
|  中国电信   | CT  | China Telecom  |

## 前置知识
由于国内政策和网络安全要求,中国国内运营商在处理公网跨境流量时对骨干设备进行了分类,一般包括C-I-X-S-F四段。其中C段表示中国大陆国内段（核心路由器）,I段表示跨境段（国际出口路由器）,X和S段表示运营商海外互联中继（国际网间互联路由）,以及F段表示与海外其他运营商的互联路由（海外POP点）。    
一般而言,C-I段部署在国内,X段多数情况下也是在国内,F段则依据对端互联运营商所在地有所差异。国家跨境数据安全网关部署在C-I段中间,三大运营商此段容量也因为安全设备容量原因而有所限制。*1

## 各运营商线路
目前中国有七家骨干网运营商,分别是中国电信、中国联通、中国移动、中国教育和科研计算机网、中国科技网 、中国国际电子商务网和中国长城互联网
### 中国电信
中国电信目前维护三张网络：AS4134 ChinaNet (163), AS4809 ChinaNet Next Generation Carrier Network (CN2) 以及 AS23764 CTGNet      
据统计,在中国电信的总网络连接中,163 网络承担了 85% 的网络流量,其余的 15% 流量,由 CN2 网络承担
#### ChinaNet
ASN: AS4134     
特征路由: `202.97.x.x`    
别称: CN1,AS4134,163骨干网      
参考价格(大量购买的折扣价格,约8折):
| 国家/地区 | 价格 | 线路类型 |
| -------- | --- | -------- |
| 北美 | $0.6/Mbps | |
| 日本/新加坡 | <=$20/Mbps | |
| 香港 | $50+/Mbps |  |
| 香港 | $25/Mbps | "普通线路" |
| 香港 | $75/Mbps | 高保障QOS |

设计架构: ChinaNet骨干网核心层由北京、上海、广州、沈阳、南京、武汉、成都、西安等8个城市的核心节点组成。核心节点之间为不完全网状结构。以北京、上海、广州为中心的三中心结构,其他核心节点分别以至少两条高速ATM链路与这三个中心相连。另外各省还建立了二级节点。
**可信数据**: 广州出口1.8Tbps
**介绍**:   
ChinaNet(a.k.a.163),主要城市如LAX、SJC、FRA、LON,回国带宽大,但因电信超卖严重,因此尖峰时段较容易阻塞,同时也常有来自中国境内的流量攻击将出口塞满。价格较低。
**特性**:   
NTT在亚太地区和中国电信163的互联仅有60G（NTT曾试图联系电信扩容但价格过于昂贵）,而多数亚太的服务商（例如Vultr/Linode/DO)由于BGP路径选择,默认情况下会优先这类网络,因此原本狭小的互联更加拥塞。     
仅统计国际网络的访问质量,163 网络全天的丢包率在 5% ~ 10%左右,如果在夜晚间高峰期（UTC+8 18:00 至 23:00 时）,丢包率可达到 15% 以上

#### CN2(ChinaNet Next Generation Carrier Network)
ASN: AS4809     
特征路由: `59.43.x.x`     
别称: CNCN    
ChinaNet Next Carrying Network（ChinaNet 的下一代承载网络）”,在 2005 年投入使用,最初架设的目标,是提供一个拓扑合理,架构先进,建设科学,用于满足未来 10-20 年替代 163 网成为未来新骨干网的网络（这个 flag 还未实现）    
由于担负的网络流量并不高,冗余量不够,CN2 网络的防御力比较脆弱。比如说,VPS 商之间同行攻击的情况屡见不鲜,所以很容易发现某家 CN2 GIA 网络的 VPS 一旦被打（DDoS攻击）,毫无还手之力,整个网络会陷入瘫痪,只有等硬扛过去,该家的 CN2 GIA 网络才能重新恢复使用。  *2
趣事:   
1. 电信内部对于AS4809的路由隔离没有特殊的要求，向下发送路由表就能够改变路由策略，曾经在CN2骨干网负载较低时不少企业同时购买GT和GIA并使用GT的容量偷跑GIA，其中不乏较大的企业如Zenlayer等，早些时候都受到了处罚。当然也有阿里云这样反向操作的，本身阿里云在香港没有购买GT，因为轻量服务器为了差异化禁播了AS4809，去程收不到CN2的路由，回程变为CN2 GT，实际上在使用购买的GIA容量跑GT。 *8
##### CN2 GT(Global Transit)
别称: 半程CN2  
CN2 GT 与ChinaNet共享C-I段,也就是说在从市级 → 省级 → 国际出口这一段走的是 ChinaNet,国际出口 → 境外接入点这一段汇入 CN2 网络,回程同理,偶尔可能会走联通的国际网络      
CN2 GT 网络的数据包在国际间传送几乎不会出现丢包的情况,但依然非常容易在国内这一段拥堵时,出现被中国电信舍弃数据包的情况,CN2 GT 网络全天的丢包率在 4% ~ 6%左右,如果在夜晚间高峰期（UTC+8 18:00 至 23:00 时）,丢包率可达到 8% 以上       
~~GT业务仅存在于北美~~(此消息存疑,如果你知道确切信息,请提交PR并附上证明),欧美地区的CN2 GT或将在接下来规划下线,现有CN2 GT的客户（已知的包括QuadraNet AS8100, 俄罗斯DataLine等）将被切换至163。
对于跨境互联质量,电信官网承认CN2 GT与ChinaNet质量无异。换句话说就是,CN2 GT集成了电信两张网各自最烂的部分：CN2海外的憨批骨干质量和163出口的傻逼跨境段质量。 *1   
截止2022年，中国电信已经与国际Tier1/2 运营商以及主流OTT建立超过5000G的互联带宽 *7

##### CN2GIA(Global Internet Access)
别称: 全程CN2
参考价格(大量购买的折扣价格,约8折):
| 国家/地区 | 价格 |
| -------- | --- |
| 北美 | $6/Mbps |
| 香港 | $80-100/Mbps |

CN2 GIA拥有独立的C-I段,也就是CN2 GIA在从市级 → 省级 → 国际出口 → 境外接入点的过程中,全程走 `59.43.x.x` 的路由节点,部分偏远地区的电信用户（比如在县级、村级、非重点市级）访问境外 CN2 GIA 线路的服务器时,会经过 ChinaNet 的一条路由并入到已布设 CN2 网络的重点市级节点,随后再全程走 CN2 网络。    
**与AS4809直接互联的CN2 GIA产品在亚太和欧洲地区已经下线,目前新接客户需要通过CTGNet。**     
该产品在163与CN2两张网之间互连有独立的端口进行互连,互连点也较多,采就近接入原则,但实际上延迟不一定比较好。且因互连点不同关系,实际上有可能会出现某地至北美GT走上海出口,但GIA却走广东出口等等差异。另互连端口在有大量攻击时容易阻塞。特别是南京、北京互连点。 *1

##### 如何区别CN2 GIA与CN2 GT
比较简单的方法就是在本地追踪一个服务器IP的去程:GT一般会有两跳或以上的ChinaNet路由(`202.97.x.x`),且下一跳CN2路由为北京/上海/广州的出口路由(`59.43.244/245/246.x`);GIA最多有一跳ChinaNet路由到重点省/市的CN2路由(`59.43.x.x`)     
请时刻牢记一点：CN2是电信的,CN2是电信的,CN2是电信的。不要说什么回程三网都不是走CN2 GIA就说这是假的CN2,这个说法是完全错误的！GIA提供到国内其他运营商的流量穿透,因此**去移动联通走不走CN2完全取决于上游商家的决定**。 *1
#### CTGNet 
ASN: AS23764    
特征路由: `69.194.x.x` 或 `203.22.x.x`      
由于通过CTG/CTA/CTE购买GIA带宽需要中国电信集团级别审批合约,因此CTG自己搞了个CTGNet（自卖自销系列）目前电信正在将已有的AS4809客户迁移到CTGNet,亚太地区比如香港和新加坡已经迁移不少客户,包括阿里云香港、腾讯等,今后新开的大部分CN2合同都会变成CTGNet的合同。      
与过去直接和AS4809接入的CN2 GIA对比,从腾讯云新加坡订购的CTGNet的路由来看,除了中间多出来CTGNet的2-3跳左右路由以外,和之前CN2 GIA的路由看起来没多少差别了。但是CTGNet网内偶尔会出现莫名其妙的延迟增加情况,体现在香港地区CTGNet网内两跳路由之间延迟增加15-20ms,这个问题属于偶发。 *1

总的来说,VPS厂商标称的CTGNet并不可信,因为你不会知道它究竟接的是CN2还是163

#### CIA
ASN: /      
特征路由: /     
别称: /     
介绍:   
CIA去程走ChinaNet,回程走CN2GIA,这样设计可以抗住大规模DDOS,回程走 CN2 保证网络速度与质量。   
提供此线路的VPS服务商(部分): `kvmloc`,`hostdare`



### 中国联通
中国联通目前有3个主要ASN：AS4837（中国联通骨干网）,AS10099（中国联通国际,CUG）以及AS9929（中国联通工业互联网,CUII,以前的老网通的骨干,简称A网）  
中国联通宽带用户增长率不高，拥有的总用户数最近竟被移动赶超，目前市场份额处于垫底，但对用户来说无疑是极其利好的消息。因为其拥有全国仅次于电信用户的国际总出口带宽，这就意味着中国联通用户访问外网遭遇的烦心事儿比电信和/移动用户要少很多，如果不想在 CN2 GIA 上花一笔大洋，一个普通中国联通宽带用户的国际访问“人权”是最有保障的  *2      
联通海外POP接入其实卖的比电信贵一些，加上联通一直在吃老底，风险应对能力相对比较弱，国内机房专用其作为海外出口接入的寥寥无几。很简单的例子，比如在面对TPE、APCN2断缆时联通直接就是延时上天，而无法快速有效调度。 *8
#### 中国联通骨干网
ASN: AS4837     
特征路由: `219.158.x.x`   
别称: 169骨干网     
出口:  北京、上海、广州  
参考价格(大量购买的折扣价格,约8折):      
| 国家/地区 | 价格 |
| -------- | --- |
| 日本/新加坡 | <=$20/Mbps |
| 香港 | <=$25/Mbps |  
| 欧洲 | <=$5/Mbps |  

**介绍**:   
一般家宽客户接入的是4837网络，与电信4134类似。比较常见的路由如北京出口承载多数省份的跨境流量，包含日韩北美欧洲俄罗斯等地区；上海出口承载至东亚/东南亚以及北美圣何塞的跨境流量；广州出口承载至港澳/东南亚/北美（洛杉矶）/欧洲部分地区的流量。    
联通在亚太地区接驳NTT的容量足够大，互联体验是比电信好很多。北美方向因为TPE数次断缆，到联通北美AS19174似乎已经放弃治疗了，延迟一度达到300ms左右。联通在欧洲接驳的上游仅有Telia和GTT，并且设置了一定的门槛，这也导致联通到欧洲基本上都绕美的。
**堵塞情况**:   
169 网络全天的丢包率在 1% ~ 3%左右，如果在夜晚间高峰期（UTC+8 18:00 至 23:00 时），丢包率可达到 4% 以上     

#### 中国联通工业互联网
ASN: AS9929     
特征路由: `218.105.x.x` 或 `210.51.x.x`     
别称: CUII,A网      
出口: 北京、上海、广州      
**介绍**:   
AS9929是中国网通（China Netcom / CNC）曾经在用的骨干网，在改组重分并入联通后被联通作为精品网进行建设；在2017年更名为CHINA UNICOM Industrial Internet Backbone，简称CUII，常被叫做联通工业网、联通A网。与电信重金建设的CN2不同，AS9929表现优良更大程度是因为负载较低，其网络完备程度、设备可靠性都是低于CN2的。比如AS9929到达北美上游Verizon，曾经都是通过自家AS4837的互联点完成的互联，并没有完全独立出来。现在作为联通对标CN2 GIA的产品销售。
**堵塞情况**:  
通过 AS9929访问外网全天的丢包率在 1% ~ 2%左右，如果在夜晚间高峰期（UTC+8 18:00 至 23:00 时），丢包率可达到 3% 以上；
#### 中国联通国际
ASN: AS10099    
特征路由:   
别称: CUG   
出口:  北京、上海、广州     
CUG 9929参考价格(大量购买的折扣价格,约8折):      
| 国家/地区 | 价格 |
| -------- | --- |
| 新加坡 | $35-38/Mbps |
| 香港 | $76-78/Mbps |  
| 日本 | $35-38/Mbps | 

10099->4837参考价格(大量购买的折扣价格,约8折):      
| 国家/地区 | 价格 |
| -------- | --- |
| 新加坡 | $25-28/Mbps |
| 香港 | $52-54/Mbps |  
| 日本 | $25-28/Mbps | 

**介绍**:   
10099（CUG）与电信目前的CTGNet类似，提供至大陆方向的差异化接入，包括CUG（10099->4837）和CUG VIP（10099->9929->4837）    
#### CUVIP
ASN: /      
特征路由: /     
别称: /     
介绍: CUVIP 可以直译为联通高级线路。这条线路并不是联通官方推出的线路,而是由美国 CeraNetworks 机房基于联通线路优化后而来的一条线路,自称为 cuvip 线路。也只有 CeraNetworks 有 cuvip 线路。cuvip 线路是怎么回事呢？CeraNetworks 从上海联通拉了 2 条 100G 带宽的 AS4837 线路。从国内到 CeraNetworks 的去程,三网走各自的线路去程。回程均走上海联通 AS4837 线路。所以去程走三网各自的去程可以做到 DDOS 防御,回程走联通的 200G 带宽的 AS4837 线路,可以保证线路速度。因此三网都可以达到最高宽带。丢包极少,晚高峰 3%以内（现阶段）。
提供此线路的VPS服务商(部分): `pigyun`,`斯巴达 vps`

### 参考资料
由于资料过于庞大,很多信息都是直接复制来做简单修改和格式化的,所以这个列表也算作转载列表,从这里其实可以看出来,这个项目只是提供一些格式化的资料(当然也有很多是我自己写的)   
我们不保证参考此料中的信息全部正确,文档中的信息均结合多份资料和实际测试进行纠错           
1. [中国三大电信运营商国际网络互联相关资料](https://blog.sunflyer.cn/archives/594)     
2. [国内主流网络运营商国际连接线路简谈](https://zhuanlan.zhihu.com/p/64467370)     
3. [国外vps的选择线路时遇到的as9929、cuvip、cia线路是什么意思](https://www.vpsjxw.com/vps_tutorials/cia9929/)          
4. [网络线路科普之CN2,GIA,CIA,BGP以及IPLC都是什么意思？](https://zhuanlan.zhihu.com/p/378653499)    
5. [local-ISPs-to-CN](https://github.com/sjlleo/local-ISPs-to-CN/blob/cd760ca23cc9d1e4981af8993ab42a67885cc2f6/report_zh_CN.md)    
6. [互联网骨干网全面解析](https://zhuanlan.zhihu.com/p/32090927)
7. [中国电信CN2 GT、CN2 GIA、ChinaNet线路介绍](https://zhuanlan.zhihu.com/p/401604334)
8. [【杂谈】运营商链路简评&鉴别（163/169/CN2等)](https://luotianyi.vc/4242.html)
9. [vps线路判断：163骨干网/cn2 gia/cn2 gt/联通CUVIP AS4837和AS9929的区别](https://www.pigji.com/1010.html)
10. [细数国内到国际的各种线路（VPS国际线路大全）](https://zhuanlan.zhihu.com/p/161029409)
11. [长文|互联网骨干网全面解析](https://zhuanlan.zhihu.com/p/267213062)