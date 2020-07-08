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

>这学期在学计算机图形,对于数字媒体专业来说这门课算是十分基础的了,于是开个坑记录一下自己的学习过程。  

# 开发环境配置  

主要是配置Windows的vs环境下OpenGL开发环境,过程比较简单,就是把OpenGL库的头文件和库文件放到vs的文件目录里。
使用工具：  
__visual studio 2017, cmake, freeGLUT__  

## OpenGL库  
 
这里使用的openGL库是[freeGLUT][],一个跨平台的OpenGL工具包,包括许多常用函数,适合初学者入门。  

## 流程  

- [下载freeGLUT][]  
![](/img\in-post\2019-09-29-OpenGL-Learning-Process\freeglut.png)
- 将获得的压缩包解压  
- 使用[cmake][]生成项目文件  
![](/img\in-post\2019-09-29-OpenGL-Learning-Process\cmake.png)
- 使用vs编译该项目,debug和release都运行一次  
![](/img\in-post\2019-09-29-OpenGL-Learning-Process\vs.png)
- 将所需库文件拷贝至vs目录下
	- 将项目文件的lib目录下的`glut.lib`函数库复制到vs文件目录的lib文件夹下 `\VC\lib`
	![](/img\in-post\2019-09-29-OpenGL-Learning-Process\freeglut_lib.png)
	- 将`glut.dll`动态库文件放到操作系统目录下面的`C:\Windows\SysWOW64（64位系统）`  
	![](/img\in-post\2019-09-29-OpenGL-Learning-Process\freeglut_dll.png)
	- 将解压得到的`glut.h`等头文件复制到如下目录下：`\VC\include\GL `, 提示：如果在incluce目录下没有GL文件夹,则需要手动创建。  
	![](/img\in-post\2019-09-29-OpenGL-Learning-Process\include.png)

## 使用  
 
直接include所需的头文件就可以使用对应的库函数了

[freeGLUT]: https://www.khronos.org/opengl/wiki/Tools/FreeGLUT
[下载freeGLUT]: https://sourceforge.net/projects/freeglut/
[cmake]: https://cmake.org/download/

# 基础概念

## model pineline
三维模型的坐标经一系列变换映射到显示设备坐标的过程。如图所示。
![](\img\in-post\2019-09-29-OpenGL-Learning-Process\coordinate_systems.png)  
重点记录几个变换矩阵的推导与理解

设某点$$P(x,y,z,1)$$,其中$$1$$为齐次坐标,有变换后的点$$P'^T=MP^T$$,其中$$M$$为变换矩阵。
- translate 平移变换  
	十分简单,对于给定的平移矢量$$v=(a,b,c)$$我们可以生成如下的变换矩阵  
	$$M = \begin{bmatrix}
	1 & 0 & 0 & a \\
	0 & 1 & 0 & b \\
	1 & 0 & 1 & c \\
	1 & 0 & 0 & 1 \\
\end{bmatrix} \tag{1}$$  
使得$$P'=MP=(x+a,y+b,z+c,1)$$
- rotate, 旋转变换  
	旋转我们一般认为是绕着定轴旋转,而根据给定定轴的不同矩阵的形式会变得很复杂,这里我们先从最简单的坐标轴开始推导,再推广到一般情况。
	设物体绕着y轴顺时针旋转θ角度（OpenGL中y轴为上下方向的轴,z轴为前后方向）。  
	![](/img/in-post/2019-09-29-OpenGL-Learning-Process/rotate_y.png)
	有 $$sin(α)=y/r$$ 以及 $$sin(α-θ)=y'/r$$ 
	利用 $$sin(α-θ)=sin(α)cos(θ)-cos(α)sin(θ)$$ ,得到 $$y'=ycos(θ)-asin(θ)$$ ,同理 $$a'=asin(θ)+ycos(θ)$$ ,可以导出变换矩阵  
	$$M = \begin{bmatrix}
	cos(θ) & -sin(θ) & 0 & 0 \\
	sin(θ) & cos(θ) & 0 & 0 \\
	0 & 0 & 1 & 0 \\
	0 & 0 & 0 & 1 \\
\end{bmatrix} \tag{2-1}$$  
	类似的，绕着其他两个坐标轴旋转的变换矩阵也具有类似的形式。在实际应用中，我们更希望物体绕着自身的轴旋转而不是绕着世界坐标轴旋转。
	![](/img/in-post/2019-09-29-OpenGL-Learning-Process/any-axis%20.png)  
	可以考虑预先进行轴旋转，使得定轴对齐世界坐标轴，使用(2-1)的矩阵进行变换后，再将物体旋转给定的角度，使得定轴回到原来的方向，这样一来即可实现绕任意的定轴旋转。 
	预先的轴旋转由于也是绕着世界坐标轴的旋转，因此也可以使用类似形式的矩阵，而旋转的角度则要通过定轴在世界坐标轴平面的投影来计算。即有  
	$$M=M_x(-θ_x)M_y(-θ_y)M_z(θ)M_y(θ_y)M_x(θ_x) \tag{2-2}$$  
	计算$$cos(θ_x),cos(θ_y)$$的值,容易发现其值为另外两轴坐标除以该轴投影长度，但是要注意辨别哪个才是要旋转的角。如图所示。 
	对于任意的定轴$$(α_x,α_y,α_z)$$（标准向量）最后得到  
	$$M_x = \begin{bmatrix}
	1 & 0 & 0 & 0 \\
	0 & α_z/\sqrt{α_y^2 +α_z^2} & -α_y/\sqrt{α_y^2 +α_z^2} & 0 \\
	0 & α_y/\sqrt{α_y^2 +α_z^2} & α_z/\sqrt{α_y^2 +α_z^2} & 0 \\
	0 & 0 & 0 & 1 \\
\end{bmatrix} \tag{2-3}$$  
    $$M_y = \begin{bmatrix}
    	α_z/\sqrt{α_x^2 +α_z^2} & 0 & α_x/\sqrt{α_x^2 +α_z^2} & 0 \\
    	0 & 1 & 0 & 0 \\
    	-α_x/\sqrt{α_x^2 +α_z^2} & 0 & α_z/\sqrt{α_x^2 +α_z^2} & 0 \\
    	0 & 0 & 0 & 1 \\
    \end{bmatrix} \tag{2-3}$$  
- scale 缩放变换。 
	也很简单，对于缩放向量(a,b,c)，直接给出变换矩阵。  
	$$M = \begin{bmatrix}
    	a & 0 & 0 & 0 \\
    	0 & b & 0 & 0 \\
    	0 & 0 & c & 0 \\
    	0 & 0 & 0 & 1 \\
    \end{bmatrix} \tag{3}$$ 
- view 视见变换  
  从观察者的视角看模型，就是从世界坐标变换到摄像机坐标，本质上属于前两种变换的组合。使用位置（点）、朝向（向量）、上方向（向量）来定义一个摄像机，
- rendering pineline