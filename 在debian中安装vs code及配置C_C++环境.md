# 安装vs code

### 基本步骤
1. 在vs code的[官网](https://code.visualstudio.com/Download)下载对应版本的安装包
根据需要下载，我下载的是debian系统的.deb文件
2. 图形化界面可以直接双击安装，我使用的是命令行
* ```cd```进入.deb文件所在目录
* ```su```使用最高级权限
* 输入```dpkg -i filename.deb```安装完成
* 输入```code```即可启动

### 遇到问题的解决办法
按照上面的安装步骤有可能在启动vs code时遇到```The window has crashed```的情况，下面的方法有可能会有帮助
1. 命令行输入以下命令，并重启机器
```c
~$ cd ~/.config
~$ rm -rf ./Code/
```
* 原因：猜测可能是加了root权限，```code```在创建.vscode文件时会遇到问题

2. ```su```进入高级权限，输入以下命令并重启机器
```c
# apt-get install -f
# apt-get install apt-transport-https
# apt-get update
# apt-get install code
```
* 原因：猜测可能是由于未安装依赖及更新

# C/C++环境的配置

参考博客链接 > 作者：bat67
(https://blog.csdn.net/bat67/article/details/76095813)

### 安装插件
* ```C/C++```必装，提供C/C++支持，只安装这一个就可以debug了
* ```C/C++ Snippets```建议安装，提供一些常用的C/C++片段，如for( ; ; )，{}，安装后写代码方便(tip.如果想要添加自己写的代码段可以点左下角齿轮->用户代码片段)
* ```EPITECH C/C++ Headers```为C/C++文件添加头部(包括作者、创建和修改日期等)，并为.h头文件添加防重复的宏
* ````File Templates```文件模板，可以自己添加文件模板
* ```GBKtoUTF8 GBK```编码文件转换为UTF-8
* ```Include Autocomplete```头文件自动补全
* ```One Dark Pro```一个好看的vscode主题

### 修改配置文件(C++为例)
```tips：如果在更改配置的过程中出现了问题导致vscode打不开，可以将所有.vscode的隐藏文件都删除即可正常打开，只是会失去所有的配置(泪目.jpg)```
1. 新建一个文件夹，在里面建一个```.cpp```或```.c```文件，先随便写一段代码
	```c
	#include <iostream>
	using namespace std;
	
	int main()
	{
	    cout << "Hello world!" << endl;
	    return 0;
	}
	```
2. 点击vs code左侧的debug标志（从上到下数第四个），再点击右边的齿轮符号，在弹出框里选择```C++(GDB)```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/20190320132811383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjkwMDQz,size_16,color_FFFFFF,t_70)
3. 在工作目录下会生成一个```launch.json```的启动配置文件，修改部分内容，最终内容如下：
	```c
	{  
	    "version": "0.2.0",  
	    "configurations": [  
	        {  
	            "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示  
	            "type": "cppdbg",       // 配置类型，这里只能为cppdbg  
	            "request": "launch",    // 请求配置类型，可以为launch（启动）或attach（附加）
	            "targetArchitecture": "x86", // 生成目标架构，一般为x86或x64，可以为x86, arm, arm64, mips, x64, amd64, x86_64  
	            "program": "${workspaceRoot}/${fileBasenameNoExtension}.exe",// 将要进行调试的程序的路径  
	            "args": [],             // 程序调试时传递给程序的命令行参数，一般设为空即可  
	            "stopAtEntry": false,   // 设为true时程序将暂停在程序入口处，一般设置为false  
	            "cwd": "${workspaceRoot}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录  
	            "environment": [],  
	            "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台  
	            "MIMode": "gdb",  
	            "preLaunchTask": "g++", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc  
	            "setupCommands": [  
	                {   
			            "description": "Enable pretty-printing for gdb",  
	                    "text": "-enable-pretty-printing",  
	                    "ignoreFailures": true  
	                }  
	            ],
	        }
	    ]  
	}
	```
4. 在这个文件同级目录下新建```tasks.json```文件，放入以下内容：
	```c
	{
	    "version": "2.0.0",
	    "command": "g++",// c++为g++, c为gcc
	    "args": ["-g","${file}","-o","${fileBasenameNoExtension}.exe"], // 编译命令参数
	    "problemMatcher": {
	        "owner": "cpp",
	        "fileLocation": ["relative", "${workspaceRoot}"],
	        "pattern": {
	            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
	            "file": 1,
	            "line": 2,
	            "column": 3,
	            "severity": 4,
	            "message": 5
	        }
	    }
	}
	```
5. 在```return 0;```前面加一个断点，按```F5```就可以调试啦
