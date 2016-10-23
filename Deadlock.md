#实验4： 死锁
##简述
死锁就是两个或多个进程，互相请求对方占有的资源，而导致互相请求不到，从而进程不能正常运行。
##死锁的四个必要条件
1. 互斥条件：一个资源只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对持有的资源保持不放。
3. 不剥夺条件：进程已获得的资源，在未使用完之前，不能剥夺其资源。
4. 循环等待条件：若干进程之间形成一种头尾相连的循环等待资源的关系。

##实验代码分析

1.  类A和类B  

		class A{
		  synchronized void methodA(B b){
		    b.last();
		  }
		  synchronized void last(){
		    System.out.println("Inside A.last");
		  }
		}
		
		class B{
		  synchronized void methodB(A a){
		    a.last();
		  }
		  synchronized void last(){
		    System.out.println("Inside B.last");
		  }
		}
   关键字synchronized。synchronized可以用来进行同步。当它用来修饰一块代码的时候，保证在同一时刻最多只有一个线程执行该段代码，也就是，只有一个进程拥有这个资源，其他需要这个资源的进程都会被阻塞。
2. 运行函数  

		
		class Deadlock implements Runnable{
		  A a=new A();
		  B b=new B();
		  Deadlock(){
		    Thread t=new Thread(this);
		    int count=20000;
		    
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
创建A类a和B类b进行实例化。
构造函数Deadlock()。创建线程t，t.start()后，线程就被插到调度队列里，循环20000次。再a调用methodA(b)，调用了B类里的last()函数。当调度到线程t的时候就跑run里的代码，b调用method(a),调用了A类里的last()函数。此时可能A的last()函数正被调用，资源被占用，使得a不能够正常运行，那么b也不能正常运行，就可能会产生死锁。

##实验过程
1. 写好Deadlock.java文件
2. 再照ppt写好.bat文件，将他们放在同一目录下，进行编译。
3. 运行
运行结果：  
实验截图：  
![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/lab4-deadlock1.png)  
  
    ![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/lab4-deadlock2.png)  

##死锁的四个必要条件
1. 互斥条件：一个资源只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对持有的资源保持不放。
3. 不剥夺条件：进程已获得的资源，在未使用完之前，不能剥夺其资源。
4. 循环等待条件：若干进程之间形成一种头尾相连的循环等待资源的关系。
##实验体会
 - 死锁会随机，在linux系统和在windows系统下都不太一样，同样的代码在linux系统下怎么改count值都不会死锁，而在windows系统下，可以跑几次就死锁。  
 linux系统下：   
 ![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/lab4-deadlockn.png)  
 Windows系统下：   
    ![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/lab4-deadlock2.png)  

