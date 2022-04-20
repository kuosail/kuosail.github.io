---
sort: 13
---
#DCDC converter

Boost topology
![Octocat](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-720862fc105336a2319fda48947652b3.png)

首先说下最基本的一个工作原理。
> 上图中MOS管就是一个开关，只要这个速度够快（开关频率够高），控制好导通与关断时间（充放电时间），配合输出滤波电容，就可以得到基本稳定的Vo了，也就是输出电压。
> 我们来简单看一下过程。
> 在开关导通的时候，电感L接地，二极管截止，Vi对电感L进行充电，电感两端电压是Vi。