---
sort: 14
---

# DCDC converter

## boost

**Boost的拓扑结构**

我们先来看**拓扑结构，一切信息都在这个里面**。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-720862fc105336a2319fda48947652b3.png)

首先说下最基本的一个**工作原理**。  

上图中MOS管就是一个开关，只要这个速度够快（开关频率够高），控制好导通与关断时间（充放电时间），配合输出滤波电容，就可以得到基本稳定的Vo了，也就是输出电压。

我们来**简单看一下过程**。

在开关导通的时候，电感L接地，二极管截止，Vi对电感L进行充电，电感两端电压是Vi。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-6721163fb04690ad255ccae7e0a6774a.png)

在开关变为不导通的时候，因为之前电感L已经被充电了，有电流流过，电流向右，电感两端电流不能突变，所以会感应出电压，让右侧的二极管导通。  

输出电压Vo恒定，二极管导通压降为Vd，所以电感右端电压为Vo+Vd，电感左端电压是电源输入Vi。这是升压boost电路， 所以Vo+Vd>Vi，电感此时放电，给负载供电，以及给输出滤波电容充电。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-36e75fa73714007728db1c1f6a98aab4.png)

并且，此时电感的两端电压是右边电压Vo+Vd减去左边电压Vi，即：Vo+Vd-Vi  

**来个前菜加深理解**

Boost电路是升压电路，是直流转直流，不考虑纹波电压的话，Vi和Vo都是恒定的，Vo大于Vi。

**在开关导通的时候**

电感L一端是恒定电压Vi，另外一端接地。这说明在开关导通的时候，**电感L两端的电压是恒定不变的，就是Vi。**

根据电感**最最最最基本的公式：U=L\*di/dt**。

（虽然我不喜欢背公式，但是这个公式我觉得是电感最重要的了，我之前还专门讲过，它可以推导出电感储能公式等等。同样，电容的最重要的公式：i=C\*du/dt。）

好，电感两端电压U=Vi不变，电感量L也是常数，所以呢，**di/dt=U/L=常数**，这不就是说电流随时间**线性**变化吗？

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-04cdd6466afbce476bed595303640779.png)

如果我们规定电流流向负载的方向是正，根据电感此时电压，是左边大于右边，所以**电感的电流是线性增大的**。  

**当开关断开的时候**

电感两端的电压U=Vo-Vi-Vd，也是恒定的，电流同样随时间线性变化。只不过电压的方向是反的，右边大于左边，所以**电感的电流是线性减小的**。

**开关导通，电感电流线性增大。**

**开关断开，电感电流线性减小。**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-4924e9f4fa7c7e827553b67a9735178d.png)

我第一次看到电感电流波形是这样的时候，我就觉得好巧啊，**怎么就一定是线性上升****呢？不是曲线上升？**  

现在自然是知道了，当然，知道也好像没什么卵用，**那说点儿有用的**。

我们在**电感选型**的时候，一定知道有个参数叫**饱和电流**吧。

我们会要求，**电感的峰值电流不能超过电感的饱和电流。**

**为啥是峰值电流，不是有效值电流？**

因为，我们一般认为电感的感量是不变的，但是实际情况是，电流大到一定程度的时候，电感量L会随电流的增大而减小，所以会有电感饱和电流这一说。

并且，随着电感电流的继续增大，电感量下降速度加快。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-52f1cedb286ad97af19860ef59b9c584.png)

我们复习下电感这个曲线，很多电感手册都有，电感的饱和电流是指**电感感量下降到标称值的30%**（不同厂家这个值有差异）的时候的电流。  

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-cde208471367c147e76612ab6cc0c97a.png)

   

**如果选型的电感饱和电流太小会怎么样呢？**

开关导通，电感电流增大，增大到饱和电流的时候，那么L会快速减小，意味着di/dt=U/L快速增大。

也就是说，di/dt变大了，即电感电流随时间更快的增大。

电流更大了，那么进一步电感感量L更小了，di/dt更更更大了，电流又更更更大了。

如此，电流就突破天际了，**这就悲剧了**。

简单画个图，感受一下。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-92563246a4e3a15f33e587bfd4b8ff88.png)

好了，根据前面的分析，我们还是画出几个关键点处的电压和电流波形吧，这应该是没什么难度的，**最难的应该属于那个电感电流的波形**了，我们也解释过了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-16fd8dbb0c03fe5cbab669ab87686742.png)

**开始推公式**

我们推公式，自然是为了更好的选型，对吧。

目的为了计算出输入电容，输出电容，功率电感，都选择多大的值。

为了更好的理解，我们把**已知的条件**都说一下。

首先是输入电压Vi，输出电压Vo，输出电流Vo/R，咱总得知道自己想要什么吧，所以这些在设计之初都是已知的。

其次是开关频率fs，这个在芯片选型之后就是确定的了。

再然后就是设计的目标，输入纹波大小△Vi，输出纹波大小△Vo。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-32fbe9bdbe3f8a6c08fe8bffcf06da57.png)

我们根据这些已知的量，就可以求得电感感量，输入滤波电容大小，输出滤波电容大小。

好，我重新把图画一下，如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-eeaf29bf1ac41bfd6a909c6314356ed3.png)

  

因为**计算的基本原理**其实就是**电容和电感的充放电**。所以，我们首先要求的就是**开关导通的时间和断开的时间**，或者说是**占空比**。

这个也非常简单，**我们可以这么想**。

在**开关导通的时候**，**电感两端电压是Vi**。

在**开关断开的时候**，输出端电压为Vo，二极管导通，那么电感右侧就是Vo+Vd，电感左侧接的是电源输入，为Vi，所以此时**电感两端电压是Vo+Vd-Vi**。

整个电路稳定之后，因为负载电流恒定，那么一个周期时间之内，在开关导通时电感电流增加的量，要等于开关截止时，电感电流减小的量，即电感充了多少电就要放多少电，不然负载的电流或者电压就要发生变化。

**即一个周期内，电感电流增大量等于减小量**。

然后又因为U=Ldi/dt，di/dt=U/L，L不变，所以**电感电流变化速度与电压成正比**。

**简单说就是，电感电流上升或下降的斜率与电压成正比**。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-a618e6f2d56626cfb59efd925ed1c3a6.png)

斜率与电压成正比，电感电流上升的高度与下降高度又相同，那上升时间不就和电压成反比了吗？  

所以，自然就有了：

Ton/Toff=(Vo+Vd-Vi)/Vi

我们变换一下，就得到了**江湖所传的“伏秒法则”**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-76bebcb58009caeb520c9dea3cc311a3.png)

再根据T=Ton+Toff=1/f  

我们可以分别求得导通时间，关断时间，占空比。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-8c7e9f1a398b750f0966eac9a08fd017.png)

好，这里，我们已经推出了第一部分公式。  

其实从这里我们可以看到。

**占空比与电感量L没有关系，与负载电流的大小也没有关系，只跟输入输出电压有关系**。

**功率电感选择**

我们电感选型首先需要考虑两个参数，**电感感量**和**电感电流**。

**电感感量又决定了电感纹波电流的大小，为什么呢？**

还是因为U=Ldi/dt，di/dt=U/L=电流变化斜率

所以，当我们确定了输入输出电压，那么电感两端的电压就是固定的，那么**电感电流变化斜率与电感量成反比，电感越大，斜率越小**。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-f45a5da818f3fd1c86158304e07057f6.png)

一般来说，电感感量的确定，是让电感的纹波电流△IL等于电感平均电流的**20%-40%之间**。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-872cad38bb0478c2a3db1e56d6fbef90.png)

**那为什么会这样呢？电感过大或过小会有什么影响？**

如果电感感量过小，那么电感纹波电流会比较大，即流过电感电流的峰值会很高，电感饱和电流就要很高。如此同时，过大的电流，在开关切换时，会导致EMI问题会更加明显。

如果电感感量过大，那么电感电流纹波会比较小，会导致动态响应变差。

**啥叫动态响应变差？**

就比如输出一直是1A的电流，某个时刻，负载从需要1A的电流变成突然需要5A的电流。这个时候，如果电感过大，电感电流充上来需要较长时间，那么电感电流需要很多个开关周期才能升到5A，这期间，负载所需要的5A电流主要来源于输出滤波电容的放电，会导致输出电压跌落比较多，有可能出现故障。

简单说，就是这个boost不能及时响应负载电流的快速变化。

**好，我们下面来求合适的电感量**

**首先**先求**电感的平均电流IL**

输出电压是Vo，输出电流是Io，输入电压是Vi，那么根据**能量守恒定律**。

**输入功率\*n=输出功率**。（n为效率）

**输入功率**，就是电源的输入电压Vi乘以平均电流，显然，从boost拓扑结构上看，电源的所有电流都会流过电感，那么这个电源输出的平均电流也就是电感的平均电流IL。

即，Pi=Vi\*IL

**输出功率**

显然，就是Po=Vo\*Io

Pi\*n=Po

即Vi\*IL\*n=Vo\*Io

那么**IL=Vo\*Io/(Vi\*n)，估算时可以取n≈80%**

我在有一些文件里面看到boost电感平均电流用这个公式计算：

**IL=(Vo+Vd)\*Io/Vi**

**这个公式怎么来的呢？**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-bebd3721471557d91b7c7580778d6061.png)

这个公式是**假设只有二极管有损耗的，忽略其它的损耗**。

如上图，稳态时，输出端电容是不耗电的，电压也不会变化，所以其平均电流为0，也就是说，流过负载的电流，全部从二极管过来。所以二极管的平均电流也是Io，导通压降是Vd，那么二极管的平均功率是Pd=Io\*Vd。

所以有：

Po=P负载+Pd

即：Pi=Vi\*IL=Io\*Vo+Io\*Vd

**也就是：IL=(Vo+Vd)\*Io/Vi**

对于这个Boost来说，二极管的损耗是占比比较大的，估算确实可以采用这个公式。不过我们需要记住，这个公式仅仅考虑了二极管的损耗。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-5584dcd7f1378f4cc2a3f4d0707583e1.png)

**我们文章后面就用这个公式来计算吧**。

**其次**，我们来求**电感的纹波电流△IL**

从前面知道，电感电流就是个三角波，在开关导通时电感电流增大，在关断时，电感电流减小。

那纹波电流的大小求起来就简单了，就等于在开关导通时电感电流增大的值，也等于关断时电感电流减小的值。

我们就计算其中一个，**计算开关导通时电感电流增大了多少**吧。

这个也非常easy，开关导通，电感两端电压是Vi，导通时间Ton前面已经求出来了。

根据U=Ldi/dt就可以求出电感电流纹波△IL=di

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-7dea485696e94a05fa87126bd659c910.png)

可以看到，**电感电流的纹波跟负载电流的大小没有关系**。  

现在我们已经写出来了电感的平均电流IL，电感的纹波电流△IL，前面说了，△IL应该是IL的20%-40%为宜。

即：△IL=(0.2~0.4)\*IL

根据这个等式，就能求得我们的**电感值范围**了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-235936b80dcfbe8f3c912c14342a82be.png)

至此，我们已经求得了电感值的取值范围，下面开始推导输入输出滤波电容的计算。  

**输入滤波电容**

我们在确定输入滤波电容的时候，是有一个假设的，这个假设是什么呢？

**输入电源默认来自远方，是没法提供快速变化的电流的**。

正是因为这一点，所以才有输入滤波电容存在的必要，**如果输入电源总能快速响应Boost的电流的需求，那还要滤波电容干什么？**

比如如果用LTspice仿真，会看到，**仿真软件自己的boost示例，都是没有输入滤波电容的**。

下图这个LT1619仿真电路，就是没有输入滤波电容的，这个是官方给出的示例，不是我画的。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-3d9712684e14088322cae65155e66215.png)

这个官方仿真示例不要输入滤波电容，原因就在于它用的电源**V1是电压源**。  

电压源在仿真软件里面的意思就是，这个IN的电压就是3.3V，永远都是3.3V，不管后面电流咋变，反正我就能绝对的把Vin的电压控制在3.3V，电流都能供上，你想要多大我就能提供多大，所以就不需要滤波电容了。

**这一点，实际电路肯定做不到，所以需要输入滤波电容来提供瞬态的电流需求**。

**那为什么实际输入电源不能快速响应呢？**

实际应用中，输入电源可能距离很远，有了很长的走线，上上期**《buck振铃尖峰的实验与分析》**文章末尾已经详细说了，走线越长，电感就越大，这里不再赘述。

总的来说，就是相当于远处的电源接了一个电感到boost电路的输入端，电感电流不能突变，也就是说输入电源不能快速响应这个boost电流的需求。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-21da628cda009680eb31ea162c19fbc1.png)

既然等效串联了一个电感，而且Boost电路是开关电源，频率大概在几百Khz，周期也就几us左右。那么在这一个周期之内，我们可以把**电源输入过来的电流看作是恒定不变的**。  

当然，肯定有人会说，**如果我的电源输入很近，可以快速响应，那就不对了呀？怎么电流能是恒定的呢？**

这想法自然没问题。

事实上，即使是电感，那也是阻碍电流的变化，并不是完全让电流不能变化，所以对于动态的电流需求，还是能响应一点的。当然，线路电感越大，就越不容易马上响应，能提供的电流波形也就越平。

但是呢，我们**没法控制这个线路的电感有多大**，或者有的电路，电源上面更是直接使用了LC滤波器。

既然没法控制，我们**就按照最差的情况来处理**，即在一个周期内，把电源输入过来的电流看作是恒定不变的，Boost需要的动态电流完全由滤波电容来提供，**根据这种情况选择的输入滤波电容，就可以满足所有的情况了**。

好，又说了一堆，回到**我们的目标：计算输入滤波电容容量**。

输入滤波电容是用来控制输入电压纹波△Vi的，下面来看**如何根据△Vi得到输入滤波电容Ci的大小**。

我们**先理清下思路**，输入电压纹波就是输入电容上面的纹波变化。电容上面的纹波变化可以分成**两个部分**。

**一个是电容放电或者是充电，存储了电荷量发生了变化**，这个变化会导致电压变化，可以用公式Q=CUq来表示，**Uq**即是电压的变化。

**另一个是电容有等效串联电阻ESR**，电容充放电时有电流流过，电流流过ESR会产生压降，这个压降用**Uesr**表示吧。

所以，电压纹波应该是：

**△Vi=Uq+Uesr**

**1、电容电荷量变化引起的压降Uq**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-57de6cfd164537504ac74ce05a3ff222.png)

我们看输入节点，这个节点的电流有3个，一个是来自电源输入的，前面说了，在一个周期内，它可以看作是恒定的，一个节点是电容，另外一个节点是电感。  

根据基尔霍夫电流定律，节点电流和为0，并且电源输入的电流恒定，那么当**电感电流的变化量必然等于电容电流的变化量**，因为最终3者的和为0。

我们**画出三者的电流波形**如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-71589dddc3967453bed41e094a7bc11e.png)

根据节点电流和为0，那么输入电容的电流变化就是功率电感的电流变化（你增大时我减小，你减小时我增大）。我们从上图也可以很直观的看出来。  

显然，电容电流大于0时，电容在充电，电容电流小于0时，电容在放电。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-7b61dc600e8a00e4d0d0a96f86571da6.png)

可以看到，**电容充电和放电时间长度是一样的，都是周期的一半，T/2**。  

**那充放电的电荷量是多少呢**？

放电的电荷量，等于放电电流i乘以放电时间t，不过放电电流不是恒定的。从前面知道，电容放电电流它等于电感电流的变化量，所以电容电流的变化量也是△IL。

需要注意，电容电流是在大于0时充电，电流小于0时放电，也就是图中阴影部分，充电与放电的切换的时刻并不是开关导通与断开的时候，而是在中间时刻。

然后电容放电/充电的**总电荷量Q等于电流乘以时间**，这不就是图中**阴影三角形的面积**吗？

**三角形底部是时间，充电/放电时间等于T/2**

**三角形的高为电感纹波电流的一半，△IL/2**。

所以**总放电量为Q=1/2\*底\*高**

再结合Q=CUq，即可求得Uq了。

**具体计算如下图所示：**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-199a72354c20da4f33fe1ffc4aa8e142.png)

  

**2、电流流过电容的ESR造成的压降Uesr**。

前面波形图知道，电容的充电电流最大是**△IL/2，放电电流最大就是-△IL/2，负号表示电流方向，方向的不同，引起的压降的电压也是相反的。**

**那么ESR引起的总的压降是：**

Uesr=**△IL/2**\*ESR-(-**△IL/2**\*ESR)= **△IL**\*ESR

最终，我们求得Uesr的公式如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-fe5bac057805b21ab435571495633f40.png)

  

好，我们已经算出Uesr和Uq。

那么根据△Vi=Uesr+Uq，我们就可以△Vi的表达式了，如果知道△Vi，我们也能得到输入电容Ci的大小或者是ESR了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-6fe60d724c8c6acd50044502a013e3bc.png)

这个公式看着有点复杂，有两个参数都跟电容本身有关系，ESR和容量Ci。  

考虑到我们的电容实际使用情况

**陶瓷电容**ESR小，容量小，Uq对纹波起决定作用，所以可以近似△Vi=Uq

**铝电解电容**容量大，ESR大，Uesr对纹波起决定作用，所以可以近似△Vi=Uesr

根据上面两点，我们就可以去选择合适的电容了。

**陶瓷电容根据容量值去选**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-b599bc622f7816ce6f3e3375aac08d12.png)

**铝电解电容**根据**ESR**去选  

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-4b4261470bda859db1ae9694745f21e2.png)

当然了，这一段话很多资料都有，但是很少有实际比较过Uq和Uesr的大小的，**文章后面会做实验来实际看看**  

好，现在输入电容的理论计算已经搞定了，我们接着看输出滤波电容。

**输出滤波电容**

相比输入纹波△Vi大小，我们可能更关心输出纹波的大小，毕竟是要带负载的。

**同样，纹波由电容电荷量变化和ESR决定。**

**1、电容电荷量变化引起的Uq**

一个周期内，电容的充电电荷量和放电电荷量必然一样，我们计算出其中一个就行了。显然，放电的时候更好计算，因为放电电流就是负载电流，是恒定的，为Io=Vo/RL。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-a7b816f469b25697013f24a5fe7bf8ac.png)

  

放电的电荷量等于容量乘以电容电压的变化，也等于放电电流乘以放电时间，即：

Q=Uq\*C=Io\*Ton

根据这个公式，我们就可以求得Uq了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-39d694fe59fc4ac70ef8b4fd3f70036a.png)

**2、电流流过电容的ESR造成的压降Uesr**

Uesr如何计算呢？

我们调出输出电容的电流波形就知道了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-e3cdf649936a2709b299fa06270b71df.png)

这个波形我解释一下。  

**在开关导通的时候**，二极管不导通，负载的电流为Io，完全由输出滤波电容提供，即滤波电容的放电电流也为Io，而且还是在导通时间里面恒定不变的。

在**开关从导通切换到断开时**，电感的电流已经是充到最大的，因为先前开关导通时电感一直在充电，所以切换时电感电流最大，且等于电感平均电流加上纹波电流的一半，即为IL+△IL/2。切换时，这个已经充好的电流会通过二极管给负载供电，负载电流为Io。同时，电感还要给电容进行充电，根据节点电流和为0，那么电容的充电电流就是电感充到最大的电流减去负载的电流，即IL+△IL/2-Io。

**在开关断开之后**，电感电压反向了，所以电感电流持续减小，也就是说二极管的电流持续减小，而负载电流不变，所以输出滤波电容的电流持续减小。

根据上图，在开关切换之前，电容的电流为-Io，那么ESR两端的电压是-Io\*ESR。

在切换之后，电容的电流立马反向，为IL+△IL/2-Io，那么ESR两端的电压是(IL+△IL/2-Io)\*ESR，两者相减，就是ESR上电压变化量，也是ESR产生的纹波电压大小。

即

Uesr=(IL+△IL/2-Io)\*ESR-(-Io\*ESR)

\= (IL+△IL/2)\*ESR，

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-aeee15af9d9e784eab9a9094dc1e6e3f.png)

好，我们已经算出Uesr和Uq。  

那么根据△Vo=Uesr+Uq，我们就可以△Vo的表达式了，如果知道△Vo，我们也能得到输出滤波电容Co的大小或者是ESR了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-924eda91ff2ca144a859469607333818.png)

与输入滤波电容一样，考虑到我们使用的电容类型。  

**陶瓷电容**ESR小，容量小，Uq对纹波起决定作用，所以可以近似△Vo=Uq

**铝电解电容**容量大，ESR大，Uesr对纹波起决定作用，所以可以近似△Vo=Uesr

根据上面两点，我们就可以去选择合适的电容了。

**陶瓷电容根据容量值去选**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-06f948c80c71f809af168172d89ca627.png)

可以看到，公式里面没有电感L，也就是说，如果**使用陶瓷电容滤波，增大电感量对输出纹波不起作用**，**不要傻傻去增大电感啦。**  

 **铝电解电容**根据**ESR**去选

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-c5672d5c557c7c465f4e15e9225718b8.png)

不容易啊，现在公式都推导完成了。

**下面进入实验环节，以此来检验上面的公式是否正确**

**实验验证**

**实验已知条件及纹波要求：**

使用boost芯片LT1619。

开关频率是f=300Khz

输入电压Vi=3.3V

输出电压Vo=5V

二极管使用MBR735，导通电压约为：Vd=0.5V

负载R=3Ω，负载电流Io=Vo/R=1.667A

输入纹波要求：△Vi≤30mV

输出纹波要求：△Vo≤50mV

**1、首先需要确定电感值L**

根据前面推导出的公式计算，可得，电感的取值范围为：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-5e947e780f4b82c6a8cebe9d308fec8e.png)

我们求得电感的范围是3.96uH~7.92uH。

我们取现实中常用的电感值**L=6.8uH**吧。

当然，我们现实中电感选型也要**考虑电感的饱和电流是否足够**，饱和电流要大于电感会流过的最大电流值ILmax，并且要留有一定的裕量。

显然，这个ILmax=IL+△IL/2。

我们根据前面的公式计算得ILmax=3.1A

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-9b3dcabac58c82d490a4b18388858c41.png)

**2、如果使用陶瓷电容滤波**

**先看输入滤波电容Ci：**

Ci的值计算结果（**忽略了ESR**）如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-4781c9ada0ad9639064cd06ca6221f4a.png)

可以看到，Ci要大于8.98uF。

我们取现实中常用的电容值**Ci=10uF**吧。

并且，在Ci=8.98uF时纹波△Vi=30mV，**那么Ci=10uF时，纹波是△Vi=26.94mV**,我们记住这个值，后面仿真对比使用。

**再看输出滤波电容Co：**

Co的值计算结果（**忽略了ESR**）如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-e1567b92c7011c768e62756235025f4e.png)

可以看到，Co需要大于44.45uF。

我们取现实中常用的电容值**Co=47uF**吧

并且，在Co=44.45uF时纹波△Vo=50mV，**那么Co=47uF时，纹波是△Vo=47.29mV**,我们也记住这个值，后面仿真对比。

**仿真验证：**

好，现在电感L，输入滤波电容Ci，输出滤波电容Co都有了

输入电压：3.3V

输出电压：5V

L=6.8uH

Ci=10uF

Co=47uF

我们**LTspice**仿真电路图如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-36161bccf5edbb8ce23158b504a10bb2.png)

**有个问题先解释一下**，在电源输入端我加了一个**1uH的电感L2**，就是为了让输入电源过来的电流基本恒定，**模拟前面说的最差的情况**（电源比较远）。若果没有这个L2，那么Vin就是稳压源的电压，绝对的稳定，没有纹波的。

我们看**仿真结果**：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-809c177b9c1646cde9dca09f92f8a841.png)

输入纹波电压计算值为26.94mV，仿真值为28mV

输出纹波电压计算值为47.29mV，仿真值为47mV

可以看到，仿真的结果与计算值非常接近，也就**验证了计算公式的准确性**。

这里插一点，为了**方便同志们学习boost**，我将**关键点的电压，电流波形**截图出来了，分析Boost可以参考

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-f6ee2a78def9a156c01680717ff67995.png)

**有一点需要说明下：**图中**二极管的电流和输出滤波电容的电流**都有一个**向下的尖峰**，这个尖峰是因为二极管的反向恢复时间造成的

即二极管电压反向，它不能马上恢复截止功能的，需要时间，这个时间就是反向恢复时间，在这个时间里面，二极管可以通过较大的反向电流，所以就有了较大的反向电流存在。

文末会给出仿真的源文件，感兴趣的同学可以自己玩一玩**，不同类型的二极管反向恢复时间不同，向下的尖峰也是不一样的**，这里就不再展开了。

我们继续

**陶瓷电容ESR**

陶瓷电容我们都通常说ESR很小，可以忽略，前面的计算也是忽略。

不过想必大家也肯定想过，**总说ESR小，影响小，那到底有多小？**

我们上面用了两个陶瓷电容，10uF和47uF，那我们查查这两个电容的ESR情况。

这里**我找了两个型号**：

10uF/10V：GRM188B31A106ME69

47uF/10V：GRM21BR61A476ME15

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-2a3053bf52861a5257122b173eb3e8be.png)

10uF电容的ESR是4mΩ，47uF电容的ESR是3mΩ

**我们还是先计算一下，ESR对纹波的贡献有多少。**

**输入10uF电容**的ESR是4mΩ，引入的纹波电压是

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-4611df254f0cb362bf721568dac6a175.png)

**相比于容量引起的纹波26.94mV，这个约为十分之一左右，确实很小。**

两者加起来，新的△Vi=26.94+2.6=**29.54mV**

**输出47uF电容**的ESR是3mΩ，引入的纹波电压

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-7b0db7ef826b4a83c7300f2c8d30185c.png)

**相比于容量引起的纹波47.29mv，这个也是比较小的，大约是五分之一吧，但似乎达不到可以忽略的地步。**

两者加起来，新的△Vo=47.29+9.3=**56.59mV**

**下面我们把ESR加入到电路中**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-a928545f497a8c090337c5bc9ef157aa.png)

运行一下，结果如下图：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-5e3f5bd74cda43ea103b328bb5bd50ee.png)

加入ESR之后，可以看到，输入纹波电压还是28mV，**基本没有变化**，不过与计算值29.54mV也差得不多。

**这个输入纹波加了ESR基本没变化，确实是有原因的**。

原因是因为输入滤波电容的电流是变化的，我们计算的是Uesr的最大值，出现最大值的时刻并不在电容放电完成后的时刻（放电完成时Uq产生的压降最大）。放电完成的时刻电容电流为0，ESR上面没有压降，所以基本就不变了，所以咱们看到的就是△Vi没变化。

不过这个也不用细细区深究，本身Uesr太小了，影响不大。

这个问题在输出滤波电容上面不会出现，因为输出滤波电容是一直有电流的，这个可以从前面的波形图看出来，所以最终的纹波，是可以将Uesr和Uq直接相加的

因此，我们可以看到，输出滤波电容的纹波电压仿真是56mV，与计算值56.59mV也是非常接近的，**增加ESR后，纹波实打实增加了9mV左右**。

而且，可以看到，输出纹波在底部有一个突然的上升，这个就是电容电流突然变化，在ESR上面产生的压降，大致也可以看到是9mV左右。

**另一方面，这个波形与我们实际测试想比，还差了点啥？**

**实际测试经常有毛刺对吧，这里面看不到**

仿真软件，其实就是使用计算机进行数学计算，一般是不会出错的，不准确肯定是模型不够准确。

很容易想到，仿真图里面电容等效一个理想电容和ESR电阻串联构成，这跟真实的电容还是有差距的，怎么说也会有寄生电感存在吧。

我就不手动添加寄生电感了，直接使用厂家提供的电容spice模型吧。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-1309cc08537c436be548dc262cba21fe.png)

**仿真结果如下图：**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-aaa597d5680e885599445b84f122736f.png)

输入还是没毛刺，**输出毛刺出来了，是不是有点儿意思呢？**

算上毛刺，输出纹波大小大概是**250mV，这是预想的50mV的5倍**。

**先来看毛刺吧，毛刺是怎么出来的呢？**

其实这个很容易，从前面分析知道，输入电容和输出电容的电流波形如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-65b250b7f43a2ec27a62bfb635d5cff4.png)

由图可知，输入滤波电容的电流是没有突变的（有拐点，但是是连续的），而输出滤波电容的电流是有突变的（由负突然变为正）。

我们知道电容都是有各种寄生参数的，自然也有寄生电感存在，突变的电流意味着di/dt很大，这必然会在寄生电感上面产生高的电压，也就是图中的毛刺。

**如何搞定这个毛刺？**

去掉是不可能的，这辈子都不可能，**只能降低幅度**。

我们在输出端加一个100nF小电容，电路图变为如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-4094409a8af8e613f7761000c0b7443b.png)

输出纹波如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-25186a9b7d9f90bd3507ea94c53a6a25.png)

可以看到，毛刺下降了，总的纹波从250mV下降到了160mV左右，效果是有的。

**毛刺还是有点大，怎么办？**

简单啊，再增加一个100nF电容，总共放两个100nf滤波电容，**是这样吗？**

仿真一下，发现纹波变成了110mV左右，确实有减小。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-1d742ac52de9dc0698b42e04092d9061.png)

所以，我们**想要降低毛刺，可以多并联几个100nF的小电容**。

想必到这里，应该知道boost后面为什么有大电容也有小电容了吧。

**大电容决定了整体纹波的大小（去掉毛刺剩下的），小电容是为了降低毛刺的**。

除了毛刺这个问题，我们发现，**使用了spice文件构建的电容之后，输入纹波和输出纹波都变大了，而且还是变大不少的**。

输入纹波从28mV变到了35mV。

输出纹波从56mV变到了83mV（不算毛刺）。

使用**spice文件**生成的电容模型的仿真结果肯定是**更为准确**的，它是厂家提供的，能更真实的还原电容的特性。

我们前面的计算公式是从拓扑结构推出来的，只考虑了电容的容量C和ESR，所以是一个理想的结果。

虽说算出来与实际结果有差距，但是还是有其意义的，至少我们知道了纹波大概在多少mv，我们**留好裕量就好**了。

**那这个裕量留多少？2倍吗？**

比如计算输出滤波电容47uF，但是仿真纹波比50mV大不少，达到了83mV，那我使用100uF的滤波电容，**容量提升了2倍**，应该可以控制在50mV以内吧。

**选用标称值为100uF/10V的MLCC陶瓷电容可以吗？**

**答案是：no！no！no！**

陶瓷电容有一个特性，就是**容量会随所加的电压发生变化，这个变化很大！！**

这个特性叫**直流偏压特性**，MLCC有这个特性，**铝电解电容没这个**。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-aeb3916eb5e90a35f36ef275604003b1.png)

上图是GRM32ER61A107ME20（100uF/10V）的电容曲线。

我们输出电压是5V，在5V时，这个电容的实际容量只有标称值的50%，也就是说只有50uF左右。

所以，选择100uF/10V是不行的，应该要选择更大容量的电容，比如200uF。或者是2个100uF的电容并联，这样**真正的有效容量**才会有100uF。

另外一方面，这个是耐压10V的电容，在5V使用时，有效容量只剩下50%，如果输出是7V，容量就只剩下30%了，也就说**必须选择更大容量的电容**。

或者说**选用耐压值更高的电容**，这样有效电容量更高。

**关于Boost使用陶瓷电容滤波，我们小结一下：**

**1、我们使用公式计算出的电容量大小，往往是偏小的，真实纹波要比计算值要高一些。**

**2、MLCC陶瓷电容的直流偏压特性，因此使用时，往往实际容量要比标称值小很多。**

**3、boost输出会容易产生高频毛刺，需要加小电容降低毛刺。**

**因此，设计时，真正的电容要比计算的大，纹波要求严格的地方，可能需要4-5倍。**

说完了使用陶瓷电容的情况，**那使用铝电解电容会怎么样呢？**

**3、使用铝电解电容滤波**

还是先来计算一番

铝电解电容的ESR比较大，所以纹波主要由ESR决定，因此我们忽略容量的影响。

**输入滤波电容ESR**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-faecb7f05f6bfb56509006a1b6fe25dd.png)

即输入滤波电容的ESR如果控制在46mΩ，那么输入纹波电压可以控制在30mV

**输出滤波电容ESR**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-cc628c6b8fc804b67718d5064e962963.png)

即输出滤波电容的ESR如果控制在16mΩ，那么能将输出纹波电压控制在50mV

**如果我们使用常规的Low ESR铝电解电容，比如Leon的VZH系列**

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-9b8075e7961679f0ef6ac61e204df4d9.png)

可以看到，满足输入46mΩ的电容容量非常大，都到了**1500uF**以上。满足输出16mΩ的滤波电容都没有。

**所以说，常规的铝电解电容用在开关电源上面滤波，效果是比较差的**，**纹波很难控制得比较低**。

**不过，也有一种电容叫做固态铝电解电容，已经是在广泛使用的。**

假定我们现在选用**尼吉康**的**PCF系列**铝固态电解电容。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-f39c93e3dcacdb8286b2651505031277.png)

可以看到，33uF的可以大致是可以满足输入纹波要求，330uF大致是可以满足输出的纹波要求得。

从官网上面下载这**330uF电容的spice文件（PCF1A331MCL4GS）**，接到输出端滤波（输入就不看了），同时加了2个100nF的电容，构建仿真电路图。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-9e6478c714d38cb74e8831d3c540e3d0.png)

仿真结果如下：

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-3b6368d612f605ce183fdf51988edede.png)

可以看到，不算毛刺纹波不高，但是毛刺振荡非常明显，加了2个100nF滤波也只能控制在300mV左右。

现在讨论不算毛刺的纹波也没有意义了，因为去不掉，我们肯定是看总的输出噪声是300mV，这个有点大。

这个原因应该是固态电容的等效串联电感太大，我比较了一下这个电容和前面的47uF陶瓷电容的spice文件，确实是固态电容的电感更大的。

所以，总的来说，不论是铝电解电容，还是固态电容，都是没有陶瓷电容好的，这也是为什么很多dcdc芯片手册都**推荐使用陶瓷电容滤波**的原因吧。

当然，也不是说铝电解电容不能用，因为我举的例子负载电流达到了1.667A的，这个算是比较大的，如果负载电流减小到三分之一，输出纹波（包括毛刺振荡）噪声也降低了，如下图，降低到了110mV左右，纹波要求不严格的话也可以用了。

![](http://mianbaoban-assets.oss-cn-shenzhen.aliyuncs.com/xinyu-images/MBXY-CR-3fc72b4defae304dedf0dcdd7cf532c9.png)

这里有一个**使用电解电容滤波，输出振荡很严重的例子**，有兴趣可以看看，我觉得他如果将铝电解换成陶瓷电容滤波，可能会改善很多，链接如下：

_https://bbs.21dianyuan.com/forum.php?mod=viewthread&tid=289717_

**小结**

这文章字数破纪录了，1w多，我也不想如此，只不过之前还是有同志们说有点难懂，我只能写一句解释一下，不好文字说明的地方就画图，图片也60多张了吧，最后就这样臭长臭长了。

所以**总结的第1点就是：有点累**。

**第2点就是**，推导公式确实可以加深对boost的理解，对各处的信号都能了如指掌，这对我们设计boost还是很有用的，特别是出现问题的时候，自然而然都能想到改怎么分析。

**第3点就是**，**计算公式与真实值是有较大差距的**

推导的公式，是基于boost的基本拓扑来的，这跟实际使用还是有较大的区别。

其中最大的一点就是，**没有公式来计算毛刺（高频振荡）**，而很多时候，毛刺占输出波动最主要的部分。