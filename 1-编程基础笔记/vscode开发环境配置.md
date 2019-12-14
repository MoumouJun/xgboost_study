[TOC]

# VSCODE

## 1.工程构建 编译调试环境+三方库
### 1.win---vs2019+OpenCV_4.1.0
> win10，动态库，

**bin,lib-库目录,include-包含目录,**
*\Microsoft Visual Studio\2019\Community\VC\Tools\MSVC*
1. 环境变量
   1. **path**：*\opencv4\opencv\build\x64\vc15\bin*
   1. *opencv_world???.dll*和*opencv_world???d.dll*文件复制到 C:\Windows\SysWOW64
   syswow64文件夹---dll接口，32位api运行在64位os上，
2. 项目属性配置
   1. 编译方式：Release/debug|X64/debug
   2. vc++
      1. 包含目录:include<???.h>
      2. 库目录：lib目录
   3. C/C++
      1. 常规->附加包含目录：include<???.h
   
   4. 链接器
      1. 附加路目录:lib库目录路径
      2. 附加依赖项：lib库名称，api的具体实现，
3. 代码测试
```c
// vs2019+opencv4.1.0
#include <opencv2/highgui.hpp>
#include <opencv2/core.hpp>
#include <opencv2/imgcodecs.hpp>

using namespace cv;
int main(void){
	Mat image;
	image = imread("C:/Users/34536/Desktop/test.png"); // Read the file
	namedWindow("Display window", WINDOW_AUTOSIZE); // Create a window for display.
	imshow("Display window", image); // Show our image inside it.
	waitKey(0); // Wait for a keystroke in the window
	return 0;
}

```

### 2.linux---Makefile


## vscode 配置C/C++开发环境
[MinGW(gcc+gdb)下载](http://www.mingw-w64.org/doku.php/download)

```shell
|vscode-c
--|.vscode
----|launch.json           #gdb调试配置
----|tasks.json            #编译配置
----|c_cpp_properties.json #
--|test.cpp                #测试程序
```


## vscode 配置web开发环境
### vscode 配置php开发环境
#### 1. xdebug调试
[操作参考](https://www.cnblogs.com/phonecom/p/10340038.html)

[xdebug下载](https://xdebug.org/download)



```php
phpinfo();
//Architecture    7.3.4
//PHP Version     X64
//Zend Extension Build     API320180731,NTS,VC15
-------------------------------------------------
\phpstudy_pro\Extensions\php\php7.3.4nts\php.ini
[xdebug]
xdebug.profiler_output_dir="E:\phpstudy_pro\tmp\xdebug"
;xdebug.profiler_output_name = cachegrind.out.%t.%p
xdebug.trace_output_dir="E:\phpstudy_pro\tmp\xdebug"
zend_extension="E:\phpstudy_pro\Extensions\php\php7.3.4nts\ext\php_xdebug-2.8.0-7.3-vc15-nts-x86_64.dll"
;xdebug.show_local_vars=0
xdebug.profiler_enable = 1
;启用代码自动追踪
xdebug.auto_trace = 1;
xdebug.show_exception_trace = 1
;这里必须设置为0，为了能使用浏览器插件 xdebug helper配合调试
xdebug.remote_autostart = 1 ;
xdebug.remote_enable = 1 ;
;这里是开启远程调试,dbgp
xdebug.remote_handler = "dbgp" ;	
;远程调试的host
xdebug.remote_host = "127.0.0.1" ;
;远程调试的端口号，注意
xdebug.remote_port = 9001	 ;
;远程调试的对应 key
xdebug.idekey="VSCODE" ;
```
### 2. 其他配置
代码补全，代码提示，错误提示，格式化

# Linux环境

## 虚拟机
## 双系统
1. centos
2. ubuntu
### 工具安装