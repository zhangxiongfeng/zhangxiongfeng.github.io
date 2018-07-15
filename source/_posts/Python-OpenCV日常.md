---
title: Python+OpenCV日常
图: 片裁剪
date: 2018-07-15 23:23:31
tags:
cover_img:
feature_img:
categories: 
-	Python
-   OpenCv
---
## Python+OpenCV日常 -图片裁剪


#### python-opencv的裁剪例子其实已经有很多，但这次总结是因为网上都没有介绍numpy切片中每个参数的含义，导致在实际运用中走了一些弯路
 
在使用的过程主要参考了以下两篇文章：

[小强学Python+OpenCV之－1.4.2裁剪](https://blog.csdn.net/eric_pycv/article/details/72637086)

[深度学习中图像的指定图像位置的裁剪处理-使用python、opencv](https://blog.csdn.net/wingfox117/article/details/79314050)


##### 总结来说,裁剪主要用如下语句：
    
```
image = cv2.imread("res/pic.jpg")

mouth = image[85:250, 85:220]

# image[y_start:y_end, x_start:x_end]
```
因为图片是由像素三维数组组成的数组，所以img[]切片的参数由**y轴起点到y轴终点，x轴起点到x轴终点**组成。
