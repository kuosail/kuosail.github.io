---
sort: 13
---

# DCDC converter


## Boost 
![Octocat](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-720862fc105336a2319fda48947652b3.png)<center>boost topology</center>

首先说下最基本的一个工作原理。
上图中MOS管就是一个开关，只要这个速度够快（开关频率够高），控制好导通与关断时间（充放电时间），配合输出滤波电容，就可以得到基本稳定的Vo了，也就是输出电压。

我们来简单看一下过程。
在开关导通的时候，电感L接地，二极管截止，Vi对电感L进行充电，电感两端电压是Vi。在开关变为不导通的时候，因为之前电感L已经被充电了，有电流流过，电流向右，电感两端电流不能突变，所以会感应出电压，让右侧的二极管导通。
![Octocat](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-6721163fb04690ad255ccae7e0a6774a.png)<center>boost topology</center>
![Octocat](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-36e75fa73714007728db1c1f6a98aab4.png)<center>boost topology</center>
输出电压Vo恒定，二极管导通压降为Vd，所以电感右端电压为Vo+Vd，电感左端电压是电源输入Vi。这是升压boost电路， 所以Vo+Vd>Vi，电感此时放电，给负载供电，以及给输出滤波电容充电。