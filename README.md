# DaemonLibrary

## 使用方式
#### Step 1. Add the JitPack repository to your build file

```
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
```

#### Step 2. Add the dependency
[![](https://jitpack.io/v/ShihooWang/DaemonLibrary.svg)](https://jitpack.io/#ShihooWang/DaemonLibrary)
~~~
	dependencies {
	        implementation 'com.github.wangshihu123:DaemonLibrary:v1.2.0'
	}
~~~

## Android后台保活，这里有你需要的所有姿势。2019，基于API26 Android O 最新版本。
老规矩，先上项目地址：
https://github.com/wangshihu123/DaemonLibrary
  结合网上各路大神及自己的项目保活实战（在此不方便透露项目），给出了最新的保活姿势。（如有雷同，纯属巧合）

## 1.为什么要做Android保活？
 	首先我个人并不推荐也不喜欢手机应用通过各种手段后台保活，当我们确定一定以及肯定地需要这个功能的时候，
	也就只能硬着头皮去与各个手机的后台管理机制做斗争了。
(一句话，产品需求呗)

## 2.故事背景
  	我们的需求是：开启户外运动，需要永驻后台，采集收据，使用过咕咚、悦跑圈的都知道。
  	但是不同的机型及厂家，让我们的应用无时无刻地在后台被杀。泪牛满面。。。
  渡尽劫波兄弟在-----做IM推送的小伙伴同样有这样的情况：

  http://www.52im.net/forum.php?mod=viewthread&tid=429&highlight=%B1%A3%BB%EE

  还有应用保活终极系列（1-3）：（分析了咕咚，乐动力，悦动圈。捎带着科普了微信和QQ）

  http://www.52im.net/forum.php?mod=viewthread&tid=1135&highlight=%B1%A3%BB%EE

  十八路神仙，各显神通。
  
 
## 3.保活总结
  对于后台保活的各种手段，在网络上比比皆是，总结下来基本是如下几种：
  1.开启服务，设置服务杀死重生；
  2.开启服务，发送通知，设置为前台服务；
  3.双进程保活；
  4.检测各种系统广播启动应用；
  5.息屏打开1像素点Activity；（QQ这么干的）
  6.开启服务，播放无声音乐(七伤拳，定制OS出现锁屏 显示音乐播放界面，及其恶心，比如 miui）；
  7.优化应用内存（敲黑板，划重点）；
  以上这些方式在网上都可以查询到，但是因为android版本不同rom不同，不一定都能派上用场，可自行查找。

这七种方法，最优秀的无非是最后一种，但我总是不去考虑他，真是坏习惯。

## 4.保活战

在这次保活战中一共涉及了个品牌的手机：
##### 1）.随意蹂躏系：
  Nexus5、Nexus6、Sony Z5、LG G4、LG G5、Samsung S6 S7（未升级到最新版本）
##### 2）.尚有尊严系：
  小米5X、魅族Note6、OPPO R11、VIVO X9柔光双摄照亮你的美（...洗脑真可怕）、Samsung J3 J5(升级到最新版本)、华为P9 P10、荣耀8（当你在后台啥都不做的时候，或者稍微动了一下）
##### 3）.宁死不屈系：
  华为P9 P10 mate10 v20 荣耀8（当你在后台动个不停的时候）

对于随意蹂躏系，请你一定要好好照顾它。它们提供了原生或者接近原生的后台管理机制，是因为相信每个应用都是善良的，所以，不到万不得已，不要欺负他们；

对于尚有尊严系，多为定制程度较高的第三方ROM，杀死后台也多处于其定制的功耗管理机制，尝试过很多灵性方法，很难做到一招鲜吃遍天，但这些ROM都留下了功耗管理白名单，他们需要保证自己系统地流畅运行，同时他们也考虑到了有些应用有他们不得不说的苦(交)衷（易），所以尊重ROM厂商的限制，不要作妖，有需求，打开白名单，你好，我好，大家好。

最后是宁死不屈系，这也是遇到的最大的难题，前面有提到我的应用不仅需要常驻后台，更需要在后台接收设备发出的蓝牙数据，也就是说我需要在后台搞事情。

## 5.围攻光明顶
以下的故事发生于我按照华为的显示开启了功耗管理白名单、后台清理白名单、忽略电量优化白名单。
于是号称是18个月不卡顿的华为出现了，也成功制裁了我:
  首先是蓝牙广播模式，当你息屏五分钟之后，由后台发起的蓝牙扫描就被休眠了，GG；
  然后是连接模式，息屏后运行一小时，凉凉；
  定位和请求网络，也是被限制的不要不要；
  服务重生+前台服务+双进程守护，六神装+复活甲在手，依旧被华为按在地上摩擦。
  直到最后，武林中流传着这样一套拳谱，伤敌一千自损八百，名曰七伤拳：无声音乐保活大法；
  也就是在服务中循环播放一段无声的音乐，cosplay正在播放的音乐播放器。
  没错，确实在华为18个月不卡顿的后台管理下活了下来，但代价是飙升的功耗，以及多任务菜单提示的音乐播放icon。
  但对于我这种特殊的应用来说，能够常驻后台，持续监测和记录，才是最重要的。

## 6.再续前缘
At last，还是想聊一下各个rom做出的后台限制。
  对于开发者来说，最欢迎的当然是原生这种随意蹂躏系，但是汝之蜜糖，彼之砒霜，这种策略如果在流氓肆虐的国内市场，估计早被啃得渣都不剩了。
  所以我个人觉得在国内市场环境下，尚有尊严系的做法挺好的，有需求就手动开启，各取所需，一切由用户决定；
  至于宁死不屈的华为，为了达到18个月不卡顿的效果，做出这种惨绝人寰的后台三光策略，有点不近人情，有点过分。

希望国内的应用市场流氓越来越少，Android手机越来越好用。


## 7.关于监听推送通知拉活的办法
消息推送（华为）：https://blog.csdn.net/qingjiao233/article/details/79914893
Android端外推送到底有多烦：
https://mp.weixin.qq.com/s?__biz=MzA4NTg1MjM0Mg==&mid=2657261350&idx=1&sn=6cea730ef5a144ac243f07019fb43076%23rd

我想到市面上的推送（排名不分先后）：
小米推送（MiPush）
华为推送（华为Push）
友盟推送（U-Push）
个推
极光推送
阿里云移动推送（Alibaba Cloud Channel Service）
腾讯信鸽推送
百度云推送

如果每一个都接入，是不是要疯#24

