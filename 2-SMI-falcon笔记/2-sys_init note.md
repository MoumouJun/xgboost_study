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



