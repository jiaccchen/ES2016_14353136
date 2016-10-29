# 在Ubuntu中安装ROS Jade #

## ROS基本介绍 ##

ROS Robot Operating System：机器人操作系统

 ROS是一个开放的标准平台，一套框架，底层提供硬件驱动，软件层面支持通用的文件格式。它提供了一系列的软件框架和工具以帮助软件开发者创建机器人应用软件。

* wiki中对ROS的解释：

> ROS（Robot Operating System）是一个适用于机器人的开源的元操作系统。它提供了操作系统应有的服务，包括硬件抽象，底层设备控制，常用函数的实现，进程间消息传递，以及包管理。它也提供用于获取、编译、编写、和跨计算机运行代码所需的工具和库函数。
> 
> ROS 的主要目标是为机器人研究和开发提供代码复用的支持。ROS是一个分布式的进程（也就是“节点”）框架，这些进程被封装在易于被分享和发布的程序包和功能包中。ROS也支持一种类似于代码储存库的联合系统，这个系统也可以实现工程的协作及发布。这个设计可以使一个工程的开发和实现从文件系统到用户接口完全独立决策（不受ROS限制）。同时，所有的工程都可以被ROS的基础工具整合在一起。


## 安装步骤 ##

1. 配置 Ubuntu 软件仓库

1. 添加 sources.list

	在这里我使用了[中山大学的镜像源](http://mirror.sysu.edu.cn/ros/)，在Ubantu终端输入如下命令：

	```
	sudo sh -c '. /etc/lsb-release && echo "deb http://mirror.sysu.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
	```

1. 添加 keys

	```
	sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
	```

1. 安装
	
	1. 	首先，确保Debian软件包索引是最新的：
	
		```
		sudo apt-get update
		```	
	1. 	Ubantu版本是14.04，ROS中有很多各种函数库和工具，在这里选择安装桌面完整版(包含ROS、rqt、rviz、通用机器人函数库、2D/3D仿真器、导航以及2D/3D感知功能)：
	
		```
		sudo apt-get install ros-jade-desktop-full
		```

1. 初始化 rosdep
	
	在开始使用ROS之前还需要初始化rosdep。rosdep可以方便在你需要编译某些源码的时候为其安装一些系统依赖，同时也是某些ROS核心功能组件所必需用到的工具。

		sudo rosdep init
		rosdep update

1. 环境配置
	
    如果每次打开一个新的终端时ROS环境变量都能够自动配置好（即添加到bash会话中），那将会方便很多：
	
		echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
		source ~/.bashrc

1. 安装 rosinstall

	rosinstall 是ROS中一个独立分开的常用命令行工具，它可以方便让你通过一条命令就可以给某个ROS软件包下载很多源码树。

	要在ubuntu上安装这个工具，请运行：
	```sudo apt-get install python-rosinstall```

1. Build farm 状态

	所安装的各种软件包都是通过[ROS build farm](http://build.ros.org/)来编译构建的。你可以在[这里](http://repositories.ros.org/status_page/ros_jade_default.html)查看每个软件包的编译状态。


## 实验感想与体会 ##
###出现的问题 ###

在步骤4的时候出现错误： `E:Unable to locate package ros-jade-desktop-full`，解决的办法是：在安装完整桌面版的ROS之前，要先对软件进行更新，`sudo apt-get update`，update是更新软件列表，回想起每一次在Ubantu上面安装一些软件之前都要执行该步骤，这个命令，会访问源列表里的每个网址，并读取**软件列表**，然后保存在本地电脑。软件包管理器里看到的软件列表，都是通过update命令更新的。区别于`sudo apt-get upgrade`，这个命令，会把本地已安装的软件，与刚下载的软件列表里**对应软件**进行对比，如果发现已安装的软件版本太低，就会提示你更新。

###思考与体会 ###
ROS的通用性、开源性、复用性、社区性等特点使它成为了目前世界上知名度最高，使用最广泛的机器人操作系统。在了解有关ROS相关背景的时候，看到一个基于ROS平台的公司，[Shanghai Gaitech Scientific Instruments](http://www.gaitech.net/index.asp)，可见现在国内在ROS系统支持的机器人领域还是取得了一定的成果，包括Baxter双臂智能协作机器人、OUR智能协作机器人、Anybody人体建模仿真系统等等。而在移动平台上的内置开源ROS安装系统，它的机体也同时带有扩展安装点能够比较容易的与其他设备集成。看完这些成果或产品，很期待下次的实验，可以体会在ROS系统下的cartographer。