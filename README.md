# -DIY--AXIDRAW-
# AXIDRAW写字机器人
####zucciot-yangfff 暑期15天实践
 -----

#8月28日：
---
##一、硬件了解
* 首先，我们需要认识制作写字机器人的硬件材料
>材料清单  

<table>
    <tr>
        <th rowspan="11">写字机材料清单</th>
        <th>品名</th>
        <th>作用</th>
    	</tr>
    <tr>
        <td>1）光轴（8mm和6mm）（2）2GT同步带和皮带轮（内径都为5mm）（3）直线轴承（8mm或6mm内径视具体情况定）（4）打印件</td>
        <td>框架部分</td>
    </tr>
	<tr>
        <td>导线；铜柱；12V电源；螺栓螺母</td>
        <td>固定整体支架</td>
    </tr>
	<tr>
        <td>42步进电机（XY轴运动）和舵机（抬笔)</td>
        <td>控制x,y,z轴方向上的指令</td>
    </tr>
	<tr>
        <td>CNC shield v3拓展板 和 Arduino UNO R3的开发板</td>
        <td>控制板</td>
    </tr>

</table>


* 下面我们开始看他的原理图：

>运动原理图：xy联动

![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/运动原理图.png)

>主板原理图：

![主板原理图](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/原理图.png)

>arduino既定图：

![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/arduino.jpeg)

> A4988原理图：

用arduino与A4988驱动步进电机[https://jingyan.baidu.com/article/ed15cb1bbf925b1be269814c.html](https://jingyan.baidu.com/article/ed15cb1bbf925b1be269814c.html)

![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/A4988原理图.jpg)

* 进行组装，线路连接得到成品

>中间运动平台组装;

>主支撑组装;

>机架组装;

>安装同步带;

>舵机及活动笔架组装;

##二、程序编写
**现在我们的基本框架出行已经出来了，让我们来编写代码吧**

>* 搭建前烧制固件和测试程序
>【ArduinoBuilder】软件//注意波特率
![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/烧录固件.jpg)


1、首先安装主板UNO驱动，驱动文件（CH341SER.EXE）；驱动安装成功的标志是在“计算机-属性-设备管理器-端口”中能查到本设备端口号，否则就没有安装成功；如没有安装成功，可能般是因为您的电脑系统缺少相关文件导致，这时可下载驱动精灵对电脑系统进行检测，并根据检测结果提示、补充完善相关系统文件(WIN10、MAC免驱动安装)。


2、 图片或文本生成G代码（使用三板块中的工具）：

>几条重要的指令：

>    G92 X242 Y0 Z148	//手扶定位

>    	G1 X242 Y0 Z148		//定位检查

>    	G28			//限位自动定位

>    	M306			//直接执行步进电机触发限位 不依赖坐标系

>    	M305			//自动计算限位触发位置的对应大小舵机角度值

>    	M84			//由于屏蔽了闲置时关步进电机的功能，
                      所以有时候需要手动关闭步进电机电源
`
   

##三、使用部分
至此，线路已经搭建好，但是我们只能控制x,y,z轴的走向，并不能去完整的完成一些图片绘制和文字的书写。不过别怕，我们已经有很多使用的软件等着我们去发现他们了，让我们现在一起去看看吧！


> 找了很多关于写字的软件，最快最实用的还是以下几个软件配合起来用。
> 软件需要以下三个：

>1、字体管家
		
>2、word文档
		
>3、奎享雕刻机

>（网络上可以下载安装）

* 首先我们下载字体，打开字体管家，找到合适的字体进行安装。
  输出时要调到小一号字左右。字符间距自己调


* 然后我们在word文档里进行排版，其中需要注意以下几个方面：

		
> 1、纸的大小，自己量自己要写的纸的大小，在设置里改动， 是多少就多少
	
>2、边距设置，自己测量，然后在边距设置里设置
	 
>    （可以把自己常用的几种纸张设置好）

>3、设置字体高度和宽度：在WORD里输入文字，和平常一样最后保存为PFD格式。

>	 设置好段落行高:我的方法是看自己的纸有多少行，例如18行，就从1编到18行，然后设置行高
	 
>注意，字体是否需要加粗，看在奎享雕刻机环境中字迹是否清晰。
		
>/*根据需求而定：
>
  4、在word文档中编程，使得我们每个字大小，上下高度，行间距，在一定范围内浮动，
		
>每个字的字体，在一些字体中进行随机切换，（前提是这些字体本身要比较接近。）
		
>实现原理：word有一套内置的编程语言，可以对整篇文章进行编程的。

>实现步骤：
   i.在Word中，依次点击“文件”->“选项”->“信任中心”->“信任中心设置”->“宏设置”，
	然后在跳出的界面中选择”启用所有宏设置”即可。

>   ii.然后在，依次点击“视图”->“宏”，输入名字：“防手写浮动”，在跳出来的窗口中，
>	进行编程。编程完毕，运行可得结果。（为不影响阅读效果，编程放在下面一块）

>实现现象：
>
>**原先文本效果：**
>
>![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/原来的文档.png)
>

>**仿手写浮动宏处理后：**
>
>![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/防手写浮动.png)
>*/
		
>5、保存为PFD格式。

* 使用奎享雕刻机

>1、打开-联接机器；

>2、找到设置 机器设置，修改x,y，z的值（参考80，80，80），保存；

>3、打开工具, 选择文件导入，更新，选择设置右边的参数；

>4、更新，点导出到路径，开始写字。


### 几点注意事项

>1、**设备组装完毕，连接线路时不要通电，不要连接数据线，**连接数据线、通电之前务必确认。

>2、在微雕管家“位图打印”界面上正确选择我们设备当前端口号和波特率（115200），然后连接设备（多数情况下设备会自动连接）。接着在“位图打印”页面调整设备起始位置（零点位置），通过调整“X+、X-、Y+、Y-”改变设备起始位置，初次调试时起始位置建议居中摆放。

>3、舵轮角度组装调试成功以后再装活动笔架。

>测试舵轮工作状态的方法：连接数据线和电源后，在微雕管家位图打印界面，来回点选强弱光，观察舵轮运转方向，舵机最高静止位最高静止位指向12点，最低静止位指向9:00-9:45左右；组装方法：点击弱光，然后把舵轮指向12点方向插好即可（确认正确后拧上小螺丝）。
 **在调试舵轮角度过程中如发现舵轮角度运动范围不符合要求则拔下舵轮换个角度重插，直到舵轮运转范围达到要求，在此过程中不可掰动舵轮，否则可能损坏舵机。**
 设备使用过程中如果发现抬落笔动作不合适的话，还可重复上述步骤进行微调。

>4“笔”的安装：笔的高度在笔架下落静止位（位图打印界面点击强光时）接触纸面即可，不是越低越好；笔的角度随您需要调整。

>5、**今后使用过程中需要结束工作时尽量通过控制“停止”“暂停”钮来实现，而不是硬性断电，以便机体、舵轮正常归位、方便下一次工作。**

>6、测试工作结束，设备正常调试、**使用应在“刀路雕刻”界面下进行**（不是“位图打印”页面！！！之后调试、使用必须转到刀路雕刻界面，位图打印界面通常是提供激光器使用的）。

>7、A、微雕管家“设备参数”界面中电机步进数80相当于1:1（实际输出大小与电脑中数据尺寸之比）；
微雕管家中加速度、移动速度需匹配组合设置，出厂20、2000，买家常用80、130或200、100等。

> B：“作为笔式绘图仪（写字机）使用”前不能打钩，否则舵机不工作。

>8、调试过程中注意设备不要碰边，如发生碰边请立即停止或断电，重新调整好起始位置和相关参数后再进行测试（比如减小步进数并保存，注意减小步进数会降低设备运动速度）。
##四、初步改进：添加蓝牙模块
>效果图

![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/蓝牙模块.jpg)

>使用方法

使用 HC 系列蓝牙，打开蓝牙识别串口

>蓝牙模块的实现原理

蓝牙hc程序源码（实现APP与单片机的通信）
[http://www.51hei.com/bbs/forum.php?mod=viewthread&tid=118640&extra=page%3D1&page=1&](http://www.51hei.com/bbs/forum.php?mod=viewthread&tid=118640&extra=page%3D1&page=1&)

	//寄存器开发版本：原子开发板stm32f103  
    #include "sys.h"
    #include "delay.h"
    #include "usart.h"
    #include "lcd.h"
    #include "key.h"
    #include "usart3.h"
    #include "HC05.h"
    #include "string.h"
    #include "lcd.h"
    #include "led.h"
    #include "beep.h"
    int main(void)
    {
        u16 reclen=0;
        Stm32_Clock_Init(9);
        delay_init(72);
        KEY_Init();
        HC05_Init(9600,36);
        uart_init(72,115200);
        LCD_Init();
        LED_Init();
        BEEP_Init();
        while(1)
        {
                if(USART3_RX_STA&0X8000)  //说明接收到数据，不懂就看usart3的中断处理函数
                {
                        reclen=USART3_RX_STA&0X7FFF;        //得到数据长度
                          USART3_RX_BUF[reclen]=0;                 //加入结束符
                        //指令的匹配函数，没有应用到工程，这里就直接用if逐个判断了
                        if(!strcmp("led0",(const char*)USART3_RX_BUF))
                        {
                                LED0=~LED0;
                        }
                        else if(!strcmp("led1",(const char*)USART3_RX_BUF))
                        {
                                LED1=~LED1;
                        }
                        else if(!strcmp("beep",(const char*)USART3_RX_BUF))
                        {
                                BEEP=~BEEP;
                        }
                        else if(!strcmp("lcdwrite",(const char*)USART3_RX_BUF))
                        {
                                POINT_COLOR=RED;
                                BACK_COLOR=DARKBLUE;
                                LCD_ShowString(30,160,200,16,16,(u8*)"blue templete  .......");
                        }
                        u3_printf("【发送数据】：%s\r\n",USART3_RX_BUF); //回显函数
                        USART3_RX_STA=0;
                }
                if(!KEY0)
                {
                        SendData((u8*)"你好啊");  //做测试用
                }
        	}
		}


>参考资料：

蓝牙、WIFI、无线模块的使用规范及开发指南
[https://blog.csdn.net/sanwzy/article/details/51118860](https://blog.csdn.net/sanwzy/article/details/51118860)


##五、效果图
>主板线路
![](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/主板线路.jpg)
>写字效果
![整体效果](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/写字效果.jpg)

>写字细节
![写字细节](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/写字细节.jpg)

>画图效果
![zucciot](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/zucciot.jpg)
![魔道祖师封面](https://lg-op7d4z6q-1257103730.cos.ap-shanghai.myqcloud.com/画图效果.jpg)

```
--------------
### 下面是word文档中“防手写浮动”宏
    Sub 字体修改()

      仿手写浮动 宏

    Dim R_Character As Range


    Dim FontSize(5)
    ' 字体大小在5个值之间进行波动，可以改写
    FontSize(1) = "21"
    FontSize(2) = "21.3"
    FontSize(3) = "21.7"
    FontSize(4) = "22"
    FontSize(5) = "22.5"



    Dim FontName(3)
    '字体名称在三种字体之间进行波动，可改写，但需要保证系统拥有下列字体
    FontName(1) = "陈静的字完整版"
    FontName(2) = "戴锦好字体X"
    FontName(3) = "方正静蕾简体"

    Dim ParagraphSpace(5)
    '行间距 在一定以下值中均等分布，可改写
    ParagraphSpace(1) = "12"
    ParagraphSpace(2) = "13"
    ParagraphSpace(3) = "20"
    ParagraphSpace(4) = "7"
    ParagraphSpace(5) = "12"

    '一般不建议修改下列代码

    For Each R_Character In ActiveDocument.Characters

        VBA.Randomize

        R_Character.Font.Name = FontName(Int(VBA.Rnd * 3) + 1)

        R_Character.Font.Size = FontSize(Int(VBA.Rnd * 5) + 1)

        R_Character.Font.Position = Int(VBA.Rnd * 3) + 1

        R_Character.Font.Spacing = 0


    Next

    Application.ScreenUpdating = True



    For Each Cur_Paragraph In ActiveDocument.Paragraphs

        Cur_Paragraph.LineSpacing = ParagraphSpace(Int(VBA.Rnd * 5) + 1)


    Next
        Application.ScreenUpdating = True


	End Sub




#8月29日
----

* **存在问题：**

```

>1、有时候突然停止工作，主机和舵机都不运转，原因是？
  
> 2、源代码还没有找到，找到一两个程序的C语言工程，继续找。
	  
> 3、使用蓝牙模块时，写字机器人接收速度慢。
	  
> 4、今天朱博询问时，电机转速是多少没有回答出来，说明对这个的硬件还不清楚。

> 5、让写字机器人y轴臂过于出去时，有可能造成写字机某处下笔较重。 

* **今日收获：**

```

> 1、安装了AD18软件，成功打开arduino板子的pcb文件（安装花费较多时间）
 
> 2、晚上向周琦学长请教，可以解决上面的问题3并得到想法的实践思路。

* 拓展的新思路：

```

<table>
    <tr>
        <th rowspan="11">拓展的新思路</th>
        <th>拓展</th>
        <th>思路1</th>
		<th>思路2</th>
    	</tr>
    <tr>
        <td>提升非连线类的传输速率</td>
        <td>换用WiFi模块</td>
		<td>换用lora模块（同为无限传输，但范围小，速度比蓝牙快）</td>
    </tr>
	<tr>
        <td>单侧下笔较重</td>
        <td>在y轴的另一端添加一个舵机，两边尽力保持平衡，</td>
		<td>调整Z轴舵机起落差，使它不会造成写字机器人的连笔，过度下笔，</td>
    </tr>
	<tr>
        <td>运用手机直接让写字机运转</td>
        <td>使用WiFi模块（单片机发送**wifi**协议到手机端)</td>

 		<td>wifi模块学习内容：单片机通讯协议（或许厂方有协议和外码）；
            APP手机通信与WiFi（有时产有app）
	    </td>
    </tr>
</table>

*** 学习部分：**

```

> lora:

物联网应用中的无线技术有多种，比如可组成无线局域网的2.4GHz的wifi，蓝牙、Zigbee，可组成广域网的比如2G/3G/4G等，但是都很难同时兼顾远距离和低功耗，于是促成了低功耗广域网（LPWAN）的出现，Lora即是LPWAN通信技术中的一种。
Lora是美国Semtech公司采用和推广的一种基于扩频技术的超远距离无线传输技术，该技术提供一种简单的能实现远距离、长电池寿命、大容量的系统，具有远距离、低功耗、多节点、低成本的特点。

 案例：公寓门锁组网、联网解决方案:[http://www.picasau.com/lora/50.html](http://www.picasau.com/lora/50.html)

LoRA基础知识：[https://blog.csdn.net/m0_38134493/article/details/72724600?locationNum=4&fps=1](https://blog.csdn.net/m0_38134493/article/details/72724600?locationNum=4&fps=1)

>三种无线传输技术在物联网应用的比较:[https://blog.csdn.net/u013063153/article/details/52755613](https://blog.csdn.net/u013063153/article/details/52755613)


1、ZigBee：低功耗、自组网
      主要的技术特点：

      一是数据传输速率低，只有10Kbps-250Kbps；

	  二是功耗低，低传输速率带来了仅为1毫瓦的低发射功率。

      三是成本低，因为ZigBee传输速率低、协议简单；
  
      四是网络容量大，每个 ZigBee网络最多可以支持255个设备，一个区域内可以同时存在最多100个ZigBee网络，网络组成灵活。ZigBee芯片主要企业有德州仪器、飞思卡尔等。市场调研机构ABI Reserch的一份数据显示，2005年到2012年，ZigBee市场的年均复合增长率为63%。

“ZigBee是从家庭自动化开始的，在瑞典哥德堡就是从智能电表开始，然后进一步用到燃气表、水表、热力表等家庭各种计量表。”在2011中国无线世界暨物联网大会上ZigBee联盟大中华区代表黄家瑞说，“ZigBee在智能电表里不仅仅是远程抄表工具，它是一个终端，也是一个网关，这些网关结合在一起，整个小区就变成了智能电网小区，智能电表可以搜集家里所有家电的用电信息。”
目前，ZigBee正在完善其网关标准，7月底发布了第十个标准ZigBeeGateway(ZigBee网关)。ZigBee Gateway提供了一种简单、高成本效益的互联网连接方式，使服务提供商、企业和个人消费者有机会运行这些设备并将ZigBee网络连接至互联网。 ZigBee Gateway是ZigBee NetworkDevicesp(ZigBee网络设备)这一新类别范畴的首个标准，这将使ZigBee发展进一步提速。
       

2、WiFi：大带宽支持家庭互联

       WiFi是以太网的一种无线扩展技术，如果有多个用户同时通过一个热点接入，带宽将被这些用户共享，WiFi的速率会降低，处于2.4GHz频段的 WiFi信号受墙壁阻隔的影响较小。WiFi的传输速率随着技术的演进还在不断提高，我国电信运营商在构建无线城市中采用的WiFi技术部分已经升级到 802.11n，最高速率从802.11g标准的11Mbps提高到50Mbps以上。在WiFi产业链中，最大的芯片企业是博通。
       “在过去几年里整个WiFi技术和产品发货量达到20亿个，整个Wi-Fi产品销售每年都是以两位数的速度持续增长。”WiFi联盟董事Myron Hattig说：“在2011年我们还会销售10亿个产品。”
       在笔记本电脑和手机上已经得到广泛应用的WiFi正在向消费电子产品渗透，Myron Hattig说：“除了手机外，已经有25%的消费类电子设备使用WiFi，在打印机、洗衣机上都在使用WiFi，家用电器生产商协会将WiFi作为一个更高级别的智能电器沟通技术。WiFi可以将设备与设备相连，从而使整个家庭的家用电器、电子设备相连。”
       最大WiFi芯片制造商博通正在推动WiFi Direct标准的商用，以支持这种设备到设备的直连。特别是在家庭互联中，相片、视频等大数据量的业务在手机、平板电脑、电视等设备中的直连应用前景广阔。Myron Hattig告诉记者：“直接技术可将平板电脑的内容展示在电视上，相关产品会在2012年发布。”
        基于WiFi上发展起来的WIGIG也是未来家庭互联市场有力的竞争技术。该技术可工作在40GHz～60GHz的超高频段，其传输速度可以达到1Gbps以上，不能穿过墙壁。目前英特尔、高通等芯片企业在支持WIGIG发展，目前该技术还在完善中，如需要进一步降低功耗等。
   
3、蓝牙：4.0进入低功耗时代

       蓝牙能在包括移动电话、PDA、无线耳机、笔记本电脑、相关外设等众多设备之间进行无线信息交换。蓝牙采用分散式网络结构以及快跳频和短包技术，支持点对点及点对多点通信，工作在全球通用的2.4GHz频段，其数据速率为1Mbps。
       2010年7月，以低功耗为特点的蓝牙4.0标准推出，蓝牙大中华区技术市场经理吕荣良将其看作蓝牙第二波发展高潮的标志，他表示：“蓝牙可以跨领域应用，主要有4个生态系统，分别是智能手机与笔记本电脑等终端市场、消费电子市场、汽车前装市场和健身运动器材市场。”
       NFC和UWB曾经是十分受关注的短距离无线接入技术，但其发展已经日渐势微。
       业内专家认为，无线频谱的规划和利用在短距离通信中日益重要。短距离通信技术目前主要采用2.4GHz的开放频谱，但随着物联网的发展和大量短距离通信技术的应用，频谱需求会快速增长，视频、图像等大数据量的通信正在寻求更高频段的解决方案。



* **单片机通信协议**

```
>单片机C语言之串口通信协议

[https://blog.csdn.net/a514371309/article/details/73481423](https://blog.csdn.net/a514371309/article/details/73481423)

>单片机通讯协议(非常经典)

[https://wenku.baidu.comview/69f5e6bb6e1aff00bed5b9f3f90f76c660374c16.html](https://wenku.baidu.com/view/69f5e6bb6e1aff00bed5b9f3f90f76c660374c16.html)

>单片机通信协议的处理方式介绍

[http://bbs.elecfans.com/jishu_1614718_1_1.html](http://bbs.elecfans.com/jishu_1614718_1_1.html)


***wifi-app**

>单片机无线控制应用

[http://blog.sina.com.cn/s/blog_68541adc0102y8mz.html](http://blog.sina.com.cn/s/blog_68541adc0102y8mz.html)

>WIFI模块实现物联网远程控制，附手机APP

http://www.cirmall.com/circuit/8192/WIFI%E6%A8%A1%E5%9D%97%E5%AE%9E%E7%8E%B0%E7%89%A9%E8%81%94%E7%BD%91%E8%BF%9C%E7%A8%8B%E6%8E%A7%E5%88%B6%EF%BC%8C%E9%99%84%E6%89%8B%E6%9C%BAAPP#/details

------

# **规划：**

1、8月29号到9月10号军训，无法进行实际操作，所以写字机器人这些推延到军训结束。

2、军训发了一本《大学生国防教育与军训训练教程》，这几天感兴趣看看，积累素材，
  下学期报了网站开发和app开发的公选，这一制作这一题材的网站或app(作为大作业)

3、微信小程序，之前朱博讲了前端后端的联系，后面“技术羊”qq网友给我做了远程演  示，有一些指导。
   
   重新看了知乎上小程序和微信小程序开发者文档。所以这几天晚上军训完如果还有精力，可以完成一下服务器的部署（现在后台还环境没有弄好），跑一下带前端后端的小程序demo，熟悉熟悉。

