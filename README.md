# 嵌入式——Dol开发环境配置

## - Description
   Dol，the Distributed operation layer，分布式操作层是一个可以（半）自动映射到SHAPES架构平台的一个框架。它主要由三个部分构成：
  
 1. DOL应用程序编程接口
    
 2. DOL功能仿真

 3. DOL映射优化
    
## - How to install

实验环境在==linux==下进行，虚拟机下的ubantu已经在之前的学习中安装过，打开虚拟机的界面如下：

![虚拟机界面][1]

## 实验步骤
### 1. 安装必要的实验环境

 1. ```$	sudo apt-get update```
 2. ```$	sudo apt-get install ant```
 3. ```$ 	sudo apt-get install openjdk-7-jdk```
 4. ```$	sudo apt-get install unzip```

运行截图如下所示： ![安装指令截取][2]
  
### 2. 下载文件

选择直接在ubantu中运行终端下载，命令行如下：

 - ```sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz ```
 - ```sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip ```

### 3. 解压文件

 1. 新建dol的文件夹
    ```$	mkdir dol```
 2. 将dolethz.zip解压到 dol文件夹中
    ```$	unzip dol_ethz.zip -d dol```
 3. 解压systemc
    ```$	tar -zxvf systemc-2.3.1.tgz```

### 4. 编译文件

 1. 解压后进入systemc-2.3.1的目录下
    ```$	cd systemc-2.3.1```

 2. 新建一个临时文件夹objdir
    ```$	mkdir objdir```

 3. 进入该文件夹objdir
    ```$	cd objdir```

 4. 运行configure(能根据系统的环境设置一下参数，用于编译)
    ```$	../configure CXX=g++ --disable-async-updates```
    
    得到如下图所示的结果：

    ![configure的结果][3]

 5. 编译
    ```$	sudo make install```
    
    编译完后文件目录如下
    
![编译后的目录][4]

 6. 记录当前的工作路径
    
![pwd 工作路径][5]

从图中可以看到当前路径为==home/jiaccchen/systemc-2.3.1==

### 5. 编译dol

 1. 进入刚刚dol的文件夹
    ```$	cd ../dol```

 2. 修改build_zip.xml文件，如下所示

```markdown?linenums
  <property name="systemc.inc"   value="/home/jiaccchen/systemc-2.3.1/include"/>
  <property name="systemc.lib"   value="/home/jiaccchen/systemc-2.3.1/lib-linux/libsystemc.a"/>
```

 3. 接着编译
    ```$	ant -f build_zip.xml all```
    若成功会显示build successful，如图：
    ![编译成功][6]
 
4. 运行一个例子
    进入build/bin/mian路径下
    ```$	cd build/bin/main```
    运行example 1;
    ```$	ant -f runexample.xml -Dnumber=1```
    得到的结果是：
    ![测试结果][7]


## - Experimental Experience

关于DOL的配置过程还算顺利，全部按照配置的步骤并在最终得到了build successful的测试结果。创建版本库和远程库并使得他们连接在一起，实现了在github上的文件的远程提交，github是完全开源的，也实现了很好的沟通性和学习。在写这篇readme时对markdown的语法进行了学习，markdown是一个很好用的排版语言，清晰简单，在插入图片的时候我选择了将图片添加进了github的仓库中，然后通过download得到图片的地址，添加到md文件中。
  

  [1]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/0.jpg
  [2]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/1.jpg
  [3]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/2.jpg
  [4]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3.jpg
  [5]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4.jpg
  [6]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/5.jpg
  [7]: https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/6.jpg