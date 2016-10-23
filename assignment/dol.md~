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

- consumer.c代码

- square.c代码

###example2代码分析  


##实验过程
1. 修改example2，让3个square模块变成2个。  
   修改代码  
   用gedit打开example2.xml  

   ` <variable value="3" name="N"/>`

   ` <!-- instantiate resources -->`
   ` <process name="generator">`
     ` <port type="output" name="10"/>`
      `<source type="c" location="generator.c"/>`
   ` </process>`

    `<iterator variable="i" range="N">`
      `<process name="square">`
        `<append function="i"/>`
        `<port type="input" name="0"/>`
        `<port type="output" name="1"/>`
        `<source type="c" location="square.c"/>`
     ` </process>`
   ` </iterator>`

    `<process name="consumer">`
      `<port type="input" name="100"/>`
      `<source type="c" location="consumer.c"/>`
`    </process>`
 
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



