---
layout: post
title: "OpenGL Learning Process"
subtitle: "学习笔记"
date: 2019-09-29 11:00:00
author: "Loniclue"
header-img: "img/in-post/2019-09-29-OpenGL-Learning-Process/OpenGL-logo.jpg"
catalog: true
tags: 
  - OpenGL
  - Graphic
  - Tutorial
---

>这学期在学计算机图形，对于数字媒体专业来说这门课算是十分基础的了，于是开个坑记录一下自己的学习过程。  

开发环境配置  
===========
主要是配置Windows的vs环境下OpenGL开发环境，过程比较简单，就是把OpenGL库的头文件和库文件放到vs的文件目录里。
使用工具：  
__visual studio 2017, cmake, freeGLUT__  

OpenGL库  
--------   
这里使用的openGL库是[freeGLUT][],一个跨平台的OpenGL工具包，包括许多常用函数，适合初学者入门。  

流程  
------ 
- [下载freeGLUT][]  
<img src="/img\in-post\2019-09-29-OpenGL-Learning-Process\freeglut.png">
- 将获得的压缩包解压  
- 使用[cmake][]生成项目文件  
<img src="/img\in-post\2019-09-29-OpenGL-Learning-Process\cmake.png">
- 使用vs编译该项目，debug和release都运行一次  
<img src="/img\in-post\2019-09-29-OpenGL-Learning-Process\vs.png">
- 将所需库文件拷贝至vs目录下
	- 将项目文件的lib目录下的`glut.lib`函数库复制到vs文件目录的lib文件夹下 `\VC\lib`
	<img src="/img\in-post\2019-09-29-OpenGL-Learning-Process\freeglut_lib.png">
	- 将`glut.dll`动态库文件放到操作系统目录下面的`C:\Windows\SysWOW64（64位系统）`  
	<img src="/img\in-post\2019-09-29-OpenGL-Learning-Process\freeglut_dll.png">
	- 将解压得到的`glut.h`等头文件复制到如下目录下：`\VC\include\GL `, 提示：如果在incluce目录下没有GL文件夹，则需要手动创建。  
	<img src="/img\in-post\2019-09-29-OpenGL-Learning-Process\include.png">

使用  
-------  
直接include所需的头文件就可以使用对应的库函数了

[freeGLUT]: https://www.khronos.org/opengl/wiki/Tools/FreeGLUT
[下载freeGLUT]: https://sourceforge.net/projects/freeglut/
[cmake]: https://cmake.org/download/