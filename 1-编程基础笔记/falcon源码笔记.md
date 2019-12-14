[TOC]
#FW-任务管理与调度
***
##1 related func
1.1 `void esmx_init(void)//remotdisplay.c,application.remotdisplay`
/* Msg pool for idle task */ 空闲登记表？
/* Msg handle for idle task */
```
    Command_task = smx_TaskCreate((FUN_PTR)Commander, PRI_LO1, CMD_STACK_SIZE, SMX_FL_NONE, "Cmd task");
    smx_TaskStart(Command_task);
```
1.2 `TCB_PTR smx_TaskCreate(FUN_PTR fun, u8 pri, u32 stksz, u32 flags, const char *name)//xtask.c,sys.smx.xsmx.`

`xtypes.h`
```
CONFIG STRUCTURE//smx配置结构体
CONTROL BLOCKS AND OTHER STRUCTURES//控制块结构体和其他结构体
```
任务控制块TCB，
***
##2 任务管理与调度
###2.1 目录管理`":\Applications\RemoteDisplay`：
id|file|描述|位置
:-:|:-:|:-:|:-:
1|Bootloader0.c|BL0|smiDispatcher.c
2|Bootloader1.c|BL1|smicusor.c
3|Burner.c|烧录程序|smiconsumer.c
4|remoteDisplay.c||smiconsumer.c
5|smiConsumer.c|消费者task|smiconsumer.c
6|smiCursor.c|鼠标相关|smiconsumer.c
7|smiDispatcher.c|分发器|
8|smiProtocol.c|用host-drive协议|
9|smiUSBD2Host.c||smicursor.c

###2.2 多任务并发：

id|task|描述|pos/pri
:-:|:-:|:-:|:-:
1|USBDispatcher|usb分发器|smiDispatcher.c
2|RenderDispatcher||smicusor.c
3|SMIDecoder|YUV解码？|sc
4|Commander|命令操作|sc
5|VideoDecoder|视频解码|sc
6|JPEGDecoder|图片解码|sc
7|OverlayRender|Overlay渲染|sc
8|CSCBliter||sc
9|CursorRender|鼠标渲染|smicursor.c/max1
10|MonitorPNP|显示器拔插EDID检测|sc/lower

###2.3 `smiQueue.c,\Applications`
消息队列，Ring Buffer 环形缓冲区，
`FIQ_Handler     B   FIQ_Handler`
***
##3 smx_rtos_Manuals 4.2.1
###3.1 手册列表

id|name|content
:-:|:-|:-
1|rm/reference manual|常用系统调用:SSR,func,macro,|smiDispatcher.c
2|qs/quick start|全局概念,
3|usbo/OTG|
4|usbh/host|
5|usbd/device|网络支持，支持TCP/IP协议栈
6|ug/user's guide|用户手册
7|fs/file sys|文件系统
8|CSCBliter||sc
9|CursorRender|鼠标渲染|smicursor.c/max1
10|MonitorPNP|显示器pnp|sc/lower

MW-middleware:usb,file-sys,net-surport

##4 Bootloader 启动加载程序
***
##5 realease
###5.1 pre_test
id|opt|times
:-:|:-|:-
1|usb_pnp|hard_pnp_opt from host,10~15
2|monitor_pnp|two_set,10~15
3|set_mode|nomal_std mode,5
4|audio：play and pnb|hdmi or dp,10
5|video_play and drag_window|two ext monitor
5|update fw(all/fw/BL1)|driver log





2.20 Wednesday
二，language basic：
1.	结构体-位域的union实现，
2.	底层框架-频繁使用预编译, 
3.	Typedef：自定义数据类型，REMOTEDISPLAY_H，MDITYPES.H，int长度与compiler…
4.	… 可变参数(参考 printf实现)，可变参数列表(一组宏)va_list解决可变参数问题，
5.	Static 存储类function&variable/global&local：3个方面——storage duration，scope，linkage
6.	宏定义：（无参数对象宏 #define <宏名> 字符串（常数，表达式，格式串等））（带参数函数宏 #define <宏名>(<参数表>) 宏体），
a)	定义宏   在#ifdef …#endif  中有效，在表达式中无效。
b)	预编译器——include文件包含+define宏替换（宏展开）+条件编译 
c)	先替换在计算，严格注意替换后的运算符优先级问题！ #define MUL(x) x*x  => #define MUL(x) ((x)*(x))，
d)	方便修改，提高运行效率（加大占用空间），简单操作宏定义，复杂操作函数调用

7.	void *memcpy(void *dest, const void *src, size_t n);，从源src所指的内存地址的起始位置开始拷贝n个字节到目标dest所指的内存地址的起始位置中，
8.	常用内存C_LIB_FUNCTIONS：
a)	extern void *malloc(unsigned int num_bytes);申请连续且指定大小内存块，即动态内存分配函数/与void free(void *FirstByte)配对使用
b)	void * memcpy ( void * destination, const void * source, size_t num )，从源内存地址的起始位置开始拷贝若干个字节到目标内存地址中
c)	void * memset ( void * ptr, int value, size_t num )，将某一块内存中的内容全部设置为指定的值， 这个函数通常为新申请的内存做初始化工作
d)	int memcmp ( const void * ptr1, const void * ptr2, size_t num )，比较内存区域buf1和buf2的前count个字节，ptr1=ptr2返回0,>返回>0
e)	
9.	按字节比较函数：int memcmp (const void *s1, const void *s2, size_t n);函数说明：memcmp()用来比较s1 和s2 所指的内存区间前n 个字符。
10.	初始化函数： void *memset(void *s, int ch, size_t n); 将s中当前位置后面的n个字节 （typedef unsigned int size_t ）用 ch 替换并返回 s 。
11.	do…while结构，
12.	

2.21 Tuesday：

1.	buffer缓冲区,
2.	*(u32*)p-强制将变量p转换成指向u32类型变量的指针，再取指针变量指向的值,*指针运算符，类型强制转换，
3.	正反斜，杠斜率为正，正斜杠/，斜率为负，反斜杠\(表示program里的特殊含义而使用的专用标点，转义字符)
4.	不同操作系统与编译环境下的数据类型（各类型长度问题）
5.	Printf指针输出格式 %p，4B(32bit),
6.	函数参数为数组时指针的使用：
7.	知名软件工程师：马丁·福勒（英语：Martin Fowler，1963年－），链家CTO-php，知名书籍作者，
8.	NULL，0，nullptr,  #define ((void *)0)
9.	void *,即void指针，malloc返回void*类型（在c中，c++需明确int*），允许函数用于分配任何数据类型的内存， long和指针长度，
10.	无法取经#define定义的常量，eg（#define SIZE 4）预编译时直接将程序中的SIZE用立即数4代替，立即数本身没有地址，（&SIZE 编译时当做&4处理）是错误的。
11.	存储区-内存分配方式，堆栈自由存储区，全局/静态存储区和常量存储区，堆区heap，栈区stack，

2.25~3.1 a week（Monday~Friday）
1.	Variable function 存储时期storage duration(static, automatic)，作用域scope（code block, file ，function ）与链接linkage(external, internal, no)，
2.	可移植类型，即数据类型的可移植性，指针的类型和大小（long int，长整型）：c++可移植性与夸平台开发（操作系统）
3.	队列queue：
4.	指针概念：指向指针的指针，为什么要有指针，int *ptr与int* ptr，指针的大小与类型，函数指针,野指针、空指针与void*指针, typedef void (*Fun) (void)
5.	C语言堆heap栈stack，数据结构堆栈
6.	内联函数inline：函数频繁调用会不断入栈(开辟函数栈)不断消耗栈内存(放置局部变量与函数内数据)，inline函数累死静态库，直接调用代码而非不断开辟函数栈；
7.	结构体的强制类型转换

3.4~3.8 a week（Monday~Friday）
1.	enum枚举类型详解：第一个成员默认值整形的0，
2.	字符数组char[]——字符串string结束符，\0；
3.	#pragma预处理指令：
①	实例分析：#pragma pack() 规定(struct、union、class-data_mem)对齐长度
4.	结构体 点运算符“.”与箭头运算符“->”,
3.11~3.5 a week（Monday~Friday）
1.	结构体字节对齐问题，
2.	位操作符，<<,>>,&,^,|,-,;
3.	Signed unsigned，有符号/无符号 ，有符号采用补码表示数据；
3.18~3.22 a week（Monday~Friday）
1.


 Tech word：
parse 解析；declare/define 声明/定义；notification 通知；resolution 解析度，即image resolution 图片分辨率；pull push 拉 推；pixel pitch 像素间距，点距；
reserved 保留；destination 目的地；panel 面板(显示器面板)；pitch 间距；
 VESA Video-Electronics-Standards-Association 视频电子标准协会； DMT Display-monitor-timing 显示器计时，DMT是一个VESA标准与定义计时清单，广泛使用在计算机行业中；  pragmatic 务实；Linux enterprize Linux企业版； PCI peripheral component interconnect 一种连接计算机主板与外部设备的总线标准；
Verilog 电子系统硬件描述语言，IEEE-1364标准；Usb VBUS(voltage bus)，usb电源线，
edid_quirk_list：quirk 怪癖-特殊型号？
Mode，即timing，模式即指时序，so mode-param即timing-param；
Verify 验证，校验；instance 实例；
Flash memory 闪存，NOR ,NAND；adapter 适配器；MMIO memory mapped io 内存映射；
Cursor 光标(鼠标)；Overlay 覆盖(video显示模式)；render 渲染；
Intr：函数名？改变软中断接口，外部硬件在通过INTR发出中断请求信号的同时，还要向处理器给出一个8位的中断向量？
Dispatcher 分发器；subsystem 子系统；
DDC display-data-channel 显示器向计算机显卡发送的可用分辨率的数据标准，实际是一个iic通道； argument 与param——actual argument 实参 参值 调用函数传值，format parameter 形参 参名 定义函数；
Peek and poke：对I/O,reg，内存和数据块的操作，读取和写入；
Strap pin：
Stride：跨距(内存中每行像素所占的空间)

Mute 静音；




Abbr： 
ret return-value，返回值；	PTC protocol 协议； 
	EP endpoint 端点；	DRM：Direct Rendering Manager？，直接渲染管理器，
EF-event flag 事件组；	ptr pointer指针；	Cpnt component 部件/构件	DDK，device drivers kit，驱动开发套件；
EG-event flag事件标志；	buf buffer缓冲区；		DDR double data rate 双倍数据速率；
pDes-pointer-destination 指针目的地，即p指向的地址	Arg  actual argument 实参		J/VPU jpeg/video process unit 图片/视频处理单元
TCM Terminal Compliance Management 终端管理？	Bpp bit per pixel 每像素的位数，像素的位深度，		
DC display control 			DDC display-data-channel 显示数据通道协议
TMO timeout 超时			
 



Basic tech knowledge：
1.	LVDS（Low Voltage Differential Signaling)即低压差分信号传输，与TTL,RSDS,TMDS，
2.	VESA（视频电子标准协会，制定标准：
i.	 EDID扩展显示认证数据，basic_128B+extension_128B包括参数——头信息+厂商ID+产品ID+32b序列号+制造日期+EDID版本+基本信息（电源，最大height*width）+颜色特征+基本时序|定时|分辨率+标准时序及定时+详细时序及定时+扩展标志位（是否+128B）+checksum求和验证值。VESA标准128Byte数据格式，通过DDC与host进行通信。
ii.	Timing （Established, Standard, or Detailed Timing时序标准），DMT/CVT(协同视频计时)/GTF(通用计时规则，99年提出，22年被CVT取代，主用于模拟VGA)标准。视频输入信号格式（计时标准，分辨率，刷新频率），
iii.	DDC通道，显示数据通道协议——实现Windows即插即用功能的基础。Os从monitor和graphics card获取信息后决定二者最佳的配合状态，使系统在每种分辨率下都自动调整到最佳的分辨频率，
3.	VGA显示器工作原理之同步信号:频率，极性（电平信号，eg.负-高长低短），同步头宽度，Hsync，Vsync（行H场V同步信号）是EDID根本，
4.	视频像素格式——YUV420 ,YUV422,RGB565,RGBx888(24bit真彩色)...;
a)	YUV(亮度Y，色度CbCr，JPEG与MPEG均采用此模式):
i.	主要用于电视系及模拟视频领域，无UV仅有Y可显示灰度图像，
ii.	亮度Y-luminance，luma，灰度值；色度UV-chrominance，chroma，描述影像色彩及饱和度，制定像素颜色；
b)	RGB:
i.	
5.	SM768GB开发板器件：
a)	srom即存储 firmware的flash;winbond-W25Q128（serial flash memory）；
b)	
6.	显示接口：
a)	DVI接口，dvi-i（integrated dual link集成双通道，兼容数字和模拟信号，2560x1600/60hz,1920x1200/120hz），（SM768版为此接口）
b)	VGA模拟信号，嵌入式板内置DAC转换通道
7.	心跳(heartbeat)包机制（tcp/ip协议）：
a)	概念：客户端每隔一小段时间向服务器发送的一个数据包，通知服务器自己仍然在线，并传输一些可能有必要的数据；
b)	实现：timer定时器，
8.	Overlay显示模式（区别于video capture？）：视频帧直接写入framebuff，不经处理器处理（“覆盖”，不经显卡处理，仅通过显存直接显示在屏幕上），大部分支持YUV格式（区别RGB格式），主要用于优化视频video播放；
9.	Framebuffer:帧缓冲，逻辑存储空间， 对应物理内存或显存。屏幕上显示的内容对应的缓存，修改fb即修改屏幕显示内容，并非直接对应，fb包含颜色/深度缓存等，诸多缓存共同作用形成屏幕上显示图像。
10.	图形graphic 图像image 视频video区别于联系
11.	总线协议:
a)	IIS音频总线：
i.	音频基础：音频格式=WAV/MP3/WMA/MIDI;压缩标准=无损/有损(波形PCM，)
12.	MMIO memory mapping I/O内存映射输入输出，cpu-to-device(相关阅读：PCI规范)：
a)	概念：将外围设备映射到内存空间，便于CPU访问，区别于PortMI/O(另一种CPU和外设交流的渠道)，CPU访问内存(物理寻址，虚拟寻址)分配给I/O设备的地址，cpu便将数据总线连接到需要访问的设备硬件寄存器，区别于DMA方法(memory-to-device)；
b)	实现：Linux中，内核——ioremap()——io设备PM地址——内核空间的VM地址，用户——mmap(2)系统调用——IO设备物理内存地址——用户空间的VMA，
c)	
13.	PCM(pulse code modulation，A-采样量化编码-D)音频编码——audio音频处理：
a)	基本概念：存储大小=采样率*分辨率(时域量化深度，bit-Byte)*录音时间s*声道数(单/双)；振幅-响度-分贝(基准值20微帕~20pa)-对数描述-调节音量；
b)	解码器-FFmpeg：
c)	PCM-5个必须参数-采样率(sample rate,44.1khz)+符号(sign +-)+分辨率(sample size 16-bit)+字节序(byte ordering，big/little)+通道(channel,mono/stereo):
14.	IAR：
a)	eww、ewp、ewd···等各文件的含义和用途
15.	Segger——jlink：	
a)	J-link command常用命令：usb连接目标板/h-halt暂停cpu运行/r-reset重启目标板/g-go继续运行/s-single 单步执行调试/setpc/regs/wreg/speed/loadbin/verifybin/f-fwinfo jlin固件信息/hwinfo 硬件信息/////
b)	


Extension tech knowledge：
1.	的计算机图形学与显卡：栅格grid 像素pixel……
2.	北京大学 数字媒体研究中:视频编码器-基于AVS2编码标准的高清实时编码器；
3.	计算机底层是如何访问显卡的：（关键概念-MMIO，内核内存映射）
a)	MMIO：显卡寄存器(软件控制硬件的唯一途径)map到显存寻址空间？，显存map到cpu寻址空间，
b)	DMA：向dma控制器写指令通过其传到显存中去，
4.	Thick/thin/zero client几种不同客户机之间区别
5.	DirectX 或 OpenGL（graphics library，最为广泛接纳的 2D/3D 图形 API） 或 CUDA 或 OpenCL ：host端图形处理/计算API,
6.	即插即用 plug-and-play与热插拔hot-plugging（or hot-swap）
7.	嵌入式OS ：smx(simple multitasking executive)，freertos，ThreadX。SMX创始人Ralph Moore，嵌入式工程师，
8.	WDDM Windows display driver model 微软新一代图形驱动程序模型，Windows drive kit编程参考-显示驱动
9.	Silicon motion embedded graphics SMI嵌入式显卡（图形处理器），SM768,SM750，4k高清+低功耗，
10.	3D显卡z-buffer数据，monitor 显示数据都是存放在显示内存（暂存显示芯片处理的数据，video ram）中，2D显卡与3D显卡，有无GPU？
11.	字长-(word size，cpu主要技术指标，汇编角度-操作数字长)：同一时间处理二进制的位数，由处理器对外数据通路的数据总线条数决定。
12.	SVG格式-图像格式(scalable vector graphics 可缩放矢量图形，使用xml格式定义图像)：MATLAB保存图片格式与Visio可编辑图片格式。
13.	软件版本，一组编译选项的集合，编译器按照选定的选项行动。
a)	Debug：调试版本，不做任何优化；
b)	Release：发布版，由编译器进行各种优化；
14.	.bat文件格式：dos下批处理文件(或称为脚本文件)，调用cmd按照该文件中各个命令出现的顺序来逐个运行它们：
a)	 命令行常用命令：dos与linux；好用的命令行软件/工具；
b)	Linux的shell与Windows的cmd：
i.	Shell（外壳，区别于内核kernel） ，Windows的桌面（explorer.exe，资源管理器）即图形shell，cmd即命令行shell。
c)	Inf文件格式，ms为硬件设备制造商发布其驱动推出 的一种文件格式；
15.	代码版本控制工具（两者一些区别）：
a)	Git（分布式的）：
i.	廖雪峰教程
b)	SVN：
i.	trunk(主线) branch(分支) tag(标记)


1. Jlink调试问题：J-link不能有效识别接入设备，拿leif的jlink调试，jtag接线与jlink的匹配有问题，目前手头上的板子有问题。
(1)	使用leif的另一个板子，熟悉了一些jlink-command命令行基本操作，基本解决问题。
(2)	待解决问题：
①	手机可通过测试板的audio通道播放音频存在一些小问题，解决思路：
1)	Iar编译下载相关代码（？），代码在测试板上有效运行；
2)	
2. 业务代码：
(1)	Audio模块的代码熟悉，
(2)	Pcm解析原理的C操作（https://blog.csdn.net/leixiaohua1020/article/details/50534316，https://www.jianshu.com/p/50c697bec409 ）；
3.  Audio-Bug思路（实现基于****方法的不同host无极调音量问题研究）：
(1)	原始数据，获取不同host与不同音量大小的pcm文件，主要是现当前业务代码在Windows-host下有效调节音量的同时兼容Android；
(2)	能否获取host主机信息，基于主机型号使用不同参数的音频解析程序；
(3)	看到了一篇相关专利，明天研究一下看看是否有效。
4. 相关知识；
(1)	Iis https://blog.csdn.net/dddd0216/article/details/50897671  https://blog.csdn.net/zouli415/article/details/80111037 
(2)	
