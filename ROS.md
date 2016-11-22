#实验5： 安装ROS（Robot operation system）

##简述
ROS是一个机器人软件平台，它能为异质计算机集群提供类似操作系统的功能。ROS是一套框架，底层提供硬件驱动，软件层面支持通用的文件格式。ROS是基于一种图状架构，从而不同节点进程能接受，发布，聚合各种信息。我们主要用到它的仿真功能，而ROS主要支持Ubuntu，所以我们要在Ubuntu中安装ROS。
##安装过程
1. Ubuntu版本  
  Unbuntu 14.04
2. 建立sources.list  
  `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
3. 建立keys
    `sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116`
4. 安装  
  更新刚刚的操作  
      `sudo apt-get update`  
 Desktop-Full Install：  
    `sudo apt-get install ros-jade-desktop-full`  
 DesktopInstall：  
    `sudo apt-get install ros-jade-desktop`  
 ROS-Base:  
    `sudo apt-get install ros-jade-ros-base`  
 Individual Package：  
    `sudo apt-get install ros-jade-PACKAGE`  
 （或者`sudo apt-get install ros-jade-slam-gmapping`）  
  找可用的包：  
    `apt-cache search ros-jade`
5. 初始化rosdep  
    `sudo rosdep init`  
    `rosdep update`  
![](https://github.com/Oujialing/ES2016_14353244/blob/master/pic/pic4.png?raw=true)
6. 环境配置  
    `echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc source ~/.bashrc`
7. Getting rosinstall  
  `sudo apt-get install python-rosinstall`
8. Build farm status  

##参考链接  
[http://wiki.ros.org/jade/Installation/Ubuntu](http://wiki.ros.org/jade/Installation/Ubuntu)