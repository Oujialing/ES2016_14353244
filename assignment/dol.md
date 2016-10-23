#实验二： DOL实例分析&编程
##简述
在example文件下有如下文件：  
![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/3-1-2.png)   

 - 其中src文件夹里有各种进程的功能定义（生产者，消费者，处理模块等）  
 ![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/3-1-3.png)
 - example1.xml是系统架构（模块连接方式定义）
##实验代码分析
###example1代码分析
1.src文件夹里  


- generator.c文件  
 
        void generator_init(DOLProcess *p) {
        p->local->index = 0;
        p->local->len = LENGTH;
        }

        int generator_fire(DOLProcess *p) {

        if (p->local->index < p->local->len) {
            float x = (float)p->local->index;
            DOL_write((void*)PORT_OUT, &(x), sizeof(float), p);
            p->local->index++;
        }

        if (p->local->index >= p->local->len) {
            DOL_detach(p);
            return -1;
        }

        return 0;
        }
  其中generator\_init()是初始化函数。这里`p->local->index=0`\指当前位置置为0，`p->local->len=LENGTH`设置生产者长度。generator\_fire()是信号产生函数，用两个if语句来表示，当下标index小于生产长度，则把小标写到输出端（用DOL\_write()函数写）；否则销毁程序（用DOL_detach（）写）。    
- consumer.c代码  

		void consumer_init(DOLProcess *p) {
		    sprintf(p->local->name, "consumer");
		    p->local->index = 0;
		    p->local->len = LENGTH;
		}
		
		int consumer_fire(DOLProcess *p) {
		    float c;
		    if (p->local->index < p->local->len) {
		        DOL_read((void*)PORT_IN, &c, sizeof(float), p);
		        printf("%s: %f\n", p->local->name, c);
		        p->local->index++;
		    }
		
		    if (p->local->index >= p->local->len) {
		        DOL_detach(p);
		        return -1;
		    }
		
		    return 0;
		}


    其中consumer\_init()是初始化函数，含义跟generator\_init()类似。consumer\_fire()是信号消费函数，含义与generator\_fire()类似。 

- square.c代码  

		void square_init(DOLProcess *p) {
	   		p->local->index = 0;
	    	p->local->len = LENGTH;
		}
	
		int square_fire(DOLProcess *p) {
	    float i;
	
	    if (p->local->index < p->local->len) {
	        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
	        i = i*i*i;
	        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
	        p->local->index++;
	    }
	
	    if (p->local->index >= p->local->len) {
	        DOL_detach(p);
	        return -1;
	    }
	
	    return 0;
	}
    定义平方进程，重点就在i=i*i,这里在做平方操作，我们实验要修改的就是这里。
  
2.example1.xml  

- 定义一个进程  

		<!-- processes -->
		  <process name="generator"> 
		    <port type="output" name="1"/>
		    <source type="c" location="generator.c"/>
		  </process>
		
		  <process name="consumer"> 
		    <port type="input" name="1"/> 
		    <source type="c" location="consumer.c"/>
		  </process>
		
		  <process name="square"> 
		    <port type="input" name="1"/>
		    <port type="output" name="2"/>
		    <source type="c" location="square.c"/>
		  </process>
  定义了模块名，端口名，他们对应的端口。有几个端口就有定义几个port。generator、consumer、square是模块名称。1和2是端口名称。input和output是端口类型。
  
- 通道定义  

                 <!-- sw_channels -->
		  <sw_channel type="fifo" size="10" name="C1">
		    <port type="input" name="0"/>
		    <port type="output" name="1"/>
		  </sw_channel>
		
		  <sw_channel type="fifo" size="10" name="C2">
		    <port type="input" name="0"/>
		    <port type="output" name="1"/>
		  </sw_channel>
	10和20是缓冲区大小。	  

 
- 模块连接定义  
 
               <!-- connections -->
		  <connection name="g-c">
		    <origin name="generator">
		      <port name="1"/>
		    </origin>
		    <target name="C1">
		      <port name="0"/>
		    </target>
		  </connection>
		
		  <connection name="c-c">
		    <origin name="C2">
		      <port name="1"/>
		    </origin>
		    <target name="consumer">
		      <port name="1"/>
		    </target>
		  </connection>
		
		  <connection name="s-c">
		    <origin name="square">
		      <port name="2"/>
		    </origin>
		    <target name="C2">
		      <port name="0"/>
		    </target>
		  </connection>
		
		  <connection name="c-s">
		    <origin name="C1">
		      <port name="1"/>
		    </origin>
		    <target name="square">
		      <port name="1"/>
		    </target>
		  </connection>
		
		</processnetwork>
   每条线有两个connection。
###example2代码分析  
-  如example1

##实验过程
1. 修改example2，让3个square模块变成2个。  
   修改代码  
   用gedit打开example2.xml  
	
	    <variable value="3" name="N"/>
	
	    <!-- instantiate resources -->
	    <process name="generator">
	      <port type="output" name="10"/>
	      <source type="c" location="generator.c"/>
	    </process>
	
	    <iterator variable="i" range="N">
	      <process name="square">
	        <append function="i"/>
	        <port type="input" name="0"/>
	        <port type="output" name="1"/>
	        <source type="c" location="square.c"/>
	      </process>
	    </iterator>
	
	    <process name="consumer">
	      <port type="input" name="100"/>
	      <source type="c" location="consumer.c"/>
	    </process>
	 
    把`<variable value="3" name="N">`该为`<variable value="2" name="N">`即可编译运行。
运行结果：  
.dot的图：  
![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/3-2-1.jpg)  
命令行的图：  
![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/3-2.png)  

2. 修改example1，使其输出3次方数。  
 修改代码（square.c文件中）：  
	 
	    int square_fire(DOLProcess *p) {
	        float i;
	
	    if (p->local->index < p->local->len) {
	        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
	        i = i*i;
	        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
	        p->local->index++;
	    }
	
	    if (p->local->index >= p->local->len) {
	        DOL_detach(p);
	        return -1;
	    }
	
	    }
      
    把`i=i*i;`改成`i=i*i*i;`即可。  
运行结果：  
.dot的图：  
![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/3-1-1.png)  
命令行的图：  
![Alt text](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/3-1.png)  
##实验体会
 - 要用在.md文件里贴照片时，先上传文件到网上，我是传到github上面。但是总是忘记如何使用命令行上传文件到github。步骤如下：  
  `git init`  
  `git add 文件名`    
  `git commit -m “备注”`    
  `git remote add origin https://github.com/Oujialing/***.git`  
  `git push -u origin master`  
 - 修改代码之后，编译运行后，没有变化。在编译运行之前要把dol/build/bin/main目录下的example*文件删除。



