﻿---
sort: 15
---

# Dialog WIFI

## DA16200 功耗测量


根据[DA16200 GETTING START GUIDE](https://www.dialog-semiconductor.com/sites/default/files/2021-12/UM-WI-056-DA16200_DA16600_FreeRTOS_Getting_Started_Guide_Rev_1.1.pdf) 说明，烧录相应的程序到开发板上。需要注意对应的指令和固件类型，FBOOT及FRTOS分别为两个固件。打开Tera term（推荐）等串口工具，依照说明配置好debug接口。为了便于调试，推荐烧录支持ATCMD的固件。下载地址：[FreeRTOS Image](https://www.dialog-semiconductor.com/system/files/2021-12/DA16200_DA16600_IMG_FreeRTOS_Package_v3.2.2.0.zip) 。

如图

![](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wif1.png?raw=true)

两个串口中，低序号是debug接口（MROM），高序号是ATCMD接口。固件烧录完成后，通过开关SW6重启板子，可见串口打印如图。

![acdshfgg](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi2.png?raw=true)

打开一个手机热点，注意频段要为2.4Ghz才能被搜到。根据[DA16200 GETTING START GUIDE](https://www.dialog-semiconductor.com/sites/default/files/2021-12/UM-WI-056-DA16200_DA16600_FreeRTOS_Getting_Started_Guide_Rev_1.1.pdf) 4.6.1章节配置DA16200模块为station模式和开启DPM，并连接上热点。DPM中有两个要素需要注意，分别是KA time即Keep Active time和DTIM，可通过图示理解：

![](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi3.png?raw=true)

![](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi4.png?raw=true)



通过调整DTIM和KA time，平均功耗会有相应的变化，如图所示。当DTIM大于KA time的时候，会导致Power down mode出现异常。

![](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi5.png?raw=true)

![](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi6.png?raw=true)

`  `DTIM用于AP传统节电模式中，多点的应用，即由AP通过设置DTIM的间隔（缺省是一个beacon时间，100ms），根据这个间隔发送组播流量。这个值不会影响单播的流量传递，如果没有开启PS的用户使用组播也不会收到影响，但是会影响开启了PS的用户接收多播数据的传递，如果设置的太小，起不到节电作用，太大又可能会影响组播通讯的质量，这个过程是一个trial-error的调整过程，只能一个一个测试调整，以达到最佳，即既可以达到最佳节电效果又不影响应用。


测试功耗的官方工具有WIFI IOT power profile 
![avatar](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi7.png?raw=true)， 但也可以使用Toolbox来进行测量。方法跟用开发板测量客户蓝牙产品方式类似。连接方式如图，只需要将母板的供电给到DA16200开发板，Toolbox就可以通过流经测量电阻的电流来记录功耗。

![avatar](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi8.png?raw=true)

![avatar](https://github.com/kuosail/kuosail.github.io/blob/main/image/WIFI/wifi9.png?raw=true)
`


