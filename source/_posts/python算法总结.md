---
title: python算法总结
date: 2018-06-22 00:11:05
tags:
categories: 
-	算法
tags:
-	Python
-   算法
---
##   1. 题号：657判断是否回到原点
我的代码里面是通过累加的方式计算最后的综合坐标是否为原点，

```
        x = 0
        y = 0
        
        for i in moves:
            if i == "U":
                y += 1
            elif i == "D":
                y -= 1
            elif i == "L":
                x -= 1
            elif i == "R":
                x += 1
        if x==0 and y==0:
            return True
        else:
            return False
```

但其实可以直接判断字符串的个数是否抵消即可，这样就省了个中间变量的创建以及计算

```
return moves.count('L') == moves.count('R') and moves.count('U') == moves.count('D')
```