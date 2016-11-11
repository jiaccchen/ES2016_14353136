# Lab4 死锁 实验报告

## 实验过程

- 一、实验截图

1.将下面代码存入Linux系统下的文件夹中（已经修改为可以产生死锁的）

```
class A {
    synchronized void methodA(B b){
        int count = 50000;
        while(count-->0);
        b.last();
    }

    synchronized void last(){
        System.out.println("Inside A.last()");
    }
}

class B {
    synchronized void methodB(A a){
        int count = 50000;
        while(count-->0);
        a.last();
    }

    synchronized void last(){
        System.out.println("Inside B.last()");
    }
}

class Deadlock implements Runnable{
    A a = new A();
    B b = new B();

    Deadlock(){
        Thread t = new Thread(this);
        int count = 1000000;
        System.out.println("hello");
        t.start();
        while(count-->0);
        a.methodA(b);
    }

    public void run(){
        b.methodB(a);
    }

    public static void main(String args[]){
        new Deadlock();
    }
}
```

原始Deaklock中的赋值20000是不会产生死锁的，程序可以跑到100times，没有出现死锁。

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-4.jpg)

2.增加classA、B中的count，这样在执行的过程中methodA(B b)以及methodB(A a)时间变长，并且调整Deadlock类中的count的值变为很大，约为【几十~一百倍】左右，这个取值取决于系统；

死锁停在了第83次，如下图所示：
 
![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-3.jpg)

3.如果想让死锁提前出现，可以减小A、B中对count的赋值，可以看到6times就出现死锁：

![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-2.jpg)

- 二、产生死锁的四个条件
	1. 互斥：至少有一个资源必须处于共享模式，即一次只有一个进程使用，如果另一个进程申请该资源，那么申请进程必须等到该资源被释放为止。
	2. 占有并等待：一个进程必须占有至少一个资源，并等待另一资源，而该资源为其他进程所占有。
	3. 非抢占：资源不能被抢占，即资源只能在进程完成任务后自动释放。
	4. 循环等待：有一组等待进程｛P0,P1,P2...Pn｝，P0等待的资源被P1所占有，P1等待的资源被P2所占有，依次类推，Pn-1等待的资源被Pn所占有，而Pn需要的资源为P0所占有，形成一个循环。
	
　　所有四个条件必须同时满足才会出现死锁，条件2与条件4意思相同，这四个条件并不完全独立。

- 三、对上述程序产生死锁的解释

　　- 关键字 synchronized:

　　在Deaklock中调用的两个函数a.methodA(b)、b.methodB(a)都是用关键字synchronized来进行修饰的，当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
　　当一个线程访问object的一个synchronized(this)同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。
  
　　- 关于死锁的产生
  
　　本次实验就是修改count的值，使得a.method和b.method时间相近，产生死锁；分析.sh文件中代码的含义就是调用文件100次，使得在调用100次之内，产生死锁；从下面的图中可以这样理解，主线程拥有锁a，申请获得锁b，t线程拥有锁b，申请获得锁a；当主线程去申请锁b时，由于t线程对锁b的拥有，此时主线程被阻塞。而t线程申请拥有锁a，由于主线程在此时拥有锁a，此时t线程也会被阻塞，这样便会产生死锁。

  ![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-5.jpg)



## 实验感想
这次实验理解java代码的执行过程并不难，由于有操作系统的基础也很容易理解死锁的过程，但是让程序跑起来的过程一波三折。

首先是出现如下报错：
　　
![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-0.jpg)
　　
出现此错误的原因是没有获得执行.sh文件的权限，因此我们在命令行执行./deaklock.sh前添加一行语句，```chmod 777 deadlock.sh```即可。
　　
接下来又会出现错误：
　　
![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-1.jpg)
　　
相同类型的错误还有```/bin/bash^M: bad interpreter: No such file or directory```，网上说此类错误是由于交叉编辑器的使用导致的，原本我是在windows下编辑好java和sh文件并粘贴到linux虚拟机下，不幸的是，经过在虚拟机中重新新建此类型文件并且编辑，仍然无法解决这一问题。
　　
最终我采取的办法是将那段代码直接粘贴到命令行中（如前文所示），顺利地运行了结果。

最后推送到远程仓库的时候，忘记```git commit -m ```这一步骤，直接想通过分支master推送到远程，出现下面的错误：
　　
![](https://raw.githubusercontent.com/jiaccchen/ES2016_14353136/master/image/4-6.jpg)

修改一下相关内容，重新执行git add便可解决。