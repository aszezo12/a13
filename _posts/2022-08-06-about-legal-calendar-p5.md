---
layout: single
title: "关于历法的一系列知识整编 - P5"
date: 2022-08-06 05:00:00 +0800
last_modified_at: 2022-08-06 07:50:00 +0800
Author: hedzr
tags: [c++17,timer,ticker,timing,calendar]
categories: c++ algorithm
comments: true
toc: true
header:
  teaser: https://cdn.jsdelivr.net/gh/hzimg/blog-pics@master/uPic/800px-World_Time_Zones_Map.png
  overlay_image: /assets/images/3953273590_704e3899d5_m.jpg
  overlay_filter: rgba(16, 16, 32, 0.73)
excerpt: >-
  历法知识整编 授时与授时服务...
---


> [历法知识整编索引页](https://hedzr.com/c++/algorithm/about-legal-calendar/)

> 摘抄、整编，已尽力罗列来源。


## 授时

**授时**的基本含义就是通过标准或者定制的接口和协议，为其他设备或系统提供[时间](https://zh.wikipedia.org/wiki/时间)信息。其基本渠道是短波、[电视](https://zh.wikipedia.org/wiki/电视)信号、[电缆](https://zh.wikipedia.org/wiki/电缆)、[网络](https://zh.wikipedia.org/wiki/网络)等等。**授时**精度从[纳秒](https://zh.wikipedia.org/wiki/纳秒)级（ns）到[毫秒](https://zh.wikipedia.org/wiki/毫秒)级（ms）不等，主要由[原子钟](https://zh.wikipedia.org/wiki/原子鐘)、[卫星](https://zh.wikipedia.org/wiki/卫星)系统、网络、高稳定度[振荡器](https://zh.wikipedia.org/wiki/振荡器)等作为时间源。

现在，中国大陆的官方标准授时由位于陕西的[国家授时中心](https://zh.wikipedia.org/wiki/国家授时中心)负责。

### 国际地球自转和参考系服务

在国际上，[国际地球自转和参考系服务](https://www.iers.org/IERS/EN/Home/home_node.html)（International Earth Rotation and Reference System, IERS）负责提供一个全球化的参考系统，目的或者说目标是为天文学，测地学等研究团体使用，以此为基础建立观测数据库。

由于地球自转的观测结果影响到授时系统的自我纠察，所以该机构的数据库具有重要意义。

国际地球自转服务（International Earth Rotation Service，IERS）由国际大地测量学和地球物理学联合会及与国际天文学联合会联合创办，用以取代国际时间局(BIH)的地球自转部分和原有的国际极移服务(IPMS)。



### 国际天文学联合会

[国际天文学联合会](https://www.iau.org/)（International Astronomical Union, IAU）是世界各国天文学术团体联合组成的非政府性学术组织，其宗旨是组织国际学术交流，推动国际协作，促进天文学的发展。国际天文学联合会于1919年7月在布鲁塞尔成立。 天文学联盟有73个成员国，其中包括专业天文学研究达到较高程度的大多数国家。天文学联盟的一个主要从事地面和空间天文学各学科的10528 多名成员的直接参与。



### 闰秒

在现行授时系统中，地球自转并非精确的 24 小时，这导致每过数年，天文观测所确定的时间与原子钟之间的偏差会大到超过 0.9 秒。此时就需要闰秒，将这多出来的一秒钟加入到国际日期和日历表中。

此前最近的一次闰秒，是 2016-12-31 23:59:60，即跨新年的一夜，23:59:59 之后不是 2017 年而是 23:59:60。

你需要注意到一个事实是，现行的绝大多数 App，H5 页面，小程序之类，均无法正确处理 60 秒这一时刻。这是因为它们在解释或者筛选日期时间的字符串片段时，针对秒数可能采用了 00-59 的范围，而不是 00-60 的范围。

但是基础系统，我是指操作系统、数据库等等植根于 tzdb/zoneinfo 的基础上的后端应用，则通常不会有这一问题。因为在 tzdb 中包含了历史以来的所有闰秒时刻。所以只要你保持系统更新，那么现代日期系统能够精确无误地处理 1970 年以来的时间点。

之所以是 1970 年开始，源于两个原因：

1. 1970 年通过的国际决议开始向 UTC 系统中加入闰秒，从而使得 UTC 时间系统保证了其精确性和稳定性。
2. 目前的所有主流操作系统均采用 Unix 时间戳记来做时间点的二进制表示。而 Unix 时间戳依据原因 1 并做了简化，形成了这样的基本版：一个 Unix 时间戳是一个有符号整数，代表从 1970-01-01 00:00:00 以来流经的秒数（或者负数代表反向）。 

> 简化：
>
> 实际上 1970 年 [国际无线电咨询委员会](https://zh.wikipedia.org/wiki/国际无线电咨询委员会)（CCIR）通过的 460 号建议案决议决定加入跳秒（即闰秒），这一决议自 1972-01-01 00:00:00 起执行。因此，Unix 时间实际上并不是以其为基准开始计时的。
>
> 历史上，Unix 时间远点被重定义过多次，最初使用 1971-01-01，每秒的数值增量为 60 （即以 60Hz 增加），这导致 32 位整数 在大约 2.5年后就会用尽。因而后来原点被重置过多次，重新定义过多次，直到最后采用的现行标准：1970-01-01 00:00:00 并以 1Hz 计时（即按秒数计时）。
>
> 在进入 21 世纪以来，由于 Unix 时间的 2038 年用尽问题，加上 64 位操作系统渐成主流，其定义也被扩展为使用 64 位有符号整数而不是 32 位的，从而避开了 2038 年 32 位整数被用尽的问题。
>
> 这当然治标不治本！
>
> 然而考虑到 64 位 Unix 时间直到 2922億7702萬6596年12月4日15時30分08秒 才会用尽，我就也不必说什么了。
>
> 那时候，我已经回归星汉灿烂了。我今天所写的每一个字，每一标点，每一思绪，尽皆湮没在时间长河中了，犯不着再提任何意见了。





## 授时服务

在真实世界，授时服务是国际化合作的，基于原子钟、天文观测与校准，会产生一个权威性的时钟，全球的各大权威天文台又同步该时钟源，并由各国的权威授时机构面向国内进行时钟发布。发布行为是多渠道的，广播、媒体、电网、其它国内基础网络，互联网等等。

在互联网中，授时服务被称作 NTP，由全球以及各国的授时机构进行服务，采用专有的 NTP 协议进行通讯，从而完成本地计算机到授时服务器的访问和时间同步。

NTP 协议的关键之处在于通过一种理论进行通讯时延的计算和消除，从而使得客户端能够在校时后与源服务器的时钟完全相同（误差许可内）。



## REFs

 [網路時間協定 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E7%B6%B2%E8%B7%AF%E6%99%82%E9%96%93%E5%8D%94%E5%AE%9A) 











:end:
