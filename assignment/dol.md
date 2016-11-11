# DOL实例分析和编程 #

## 一、实验过程 ##

-  任务1 

修改example2 ，让3个square模块变成2个, tips:修改xml的iterator；

1. 简要分析
在原来的代码中是通过设置value的值来设定迭代次数，原本的value=3即定义了3个模块，根据题目要求修改value的值=2，这样通过迭代生成连接connection。修改的代码如下：

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-2-0.jpg)

2. 运行结果
在编译之前要将原来/home/jiaccchen/dol/build/bin/main文件夹下的已经build好的文件删除，才可以继续编译和运行。

编译代码：

	cd /home/jiaccchen/dol/build/bin/main
	ant -f runexample.xml -Dnumber=2
    
![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-2-4.jpg)

编译成功的结果：
  
![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-2-3.jpg)

	
接着查看/home/jiaccchen/dol/build/bin/main/example2/example2.dot，查看dot图如下所示：

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-2-5.jpg)

---

- 任务2

修改 example1，使其输出3次方数，tips:修改 square.c；

原来输出的是i*i的过程，定义平方过程如下：square_fire信号处理函数，读入输入端信号i，将其平方后写出到输出端，也是重复length次之后就停止了。原来的输出如下图：

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-1-1.jpg)

按照题目要求即在square.c文件的square_fire函数中修改为i*i*i，如下图所示：

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-1-0.jpg)

修改后输出的结果即是对输入端信号求立方和。

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/3-1-3.jpg)

---

## 二、实验感想

本次实验涉及到Dol的编程实例，虽然分析代码的过程较为复杂，但是根据实验要求进行修改的过程较为简单，在这个过程中顺利的理解了生产者，平方模块，消费者的进程，也对连接各个通道的模块进行理解。感觉dol是很直观的图形，对于更加深入的问题研究情况下会有很大方便。
