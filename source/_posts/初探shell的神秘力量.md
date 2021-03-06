﻿---
title: 初探shell的神秘力量
date: 2017-05-03 15:31:37
tags:
cover_img: https://i.loli.net/2018/06/30/5b36596186b93.jpg
categories: shell
---

其实开博也有差不多一个多月了，今天才算刚刚从北京这个炼狱里面出来，能有那么一丝喘气的机会好好享受大学最后那丁点丁点的时光。在这两个月里面集中项目开发时间里面，虽然一天工作16小时，但更多的时间都被shell这个魔咒给消耗了大部分的时间。有人说，每次写shell，都让自己感觉像是个傻逼程序员一样。为此我将在下面总结一下在写shell脚本的时候自己常用的几个知识点以及小技巧。仅供参考，我自己参考。。。

### 1.shell的for基本循环

```
for i in $( seq 1 4)
do
	echo “准备打印：” $i
done
```
打印：1 2 3 4 
### 2.shell的for遍历数组循环

部分引用来自：https://www.coder4.com/archives/3853

```
arr=(1 2 3 4 5) # shell定义数字，用空格隔开

echo ${#array[@]} #打印数组长度

for var in ${ arr[@] };#遍历数组各个元素并开始打印
do
    echo $var
done

for i in "${!arr[@]}";#打印数组元素（带数组下标）
do 
    printf "%s\t%s\n" "$i" "${arr[$i]}"
done
```

例子：当前目录中数组各个文件名及其绝对路径

```
for i in ${dirName[@]} #dirName为当前目录文件名
do
	echo "此时正遍历该文件：" ${PWD}"/"${i}
	cd ${PWD}"/"${i}
done
```

### 3.遍历当前目录下的文件名

```
for i in `ls`
		do
			printf ${i%%.*} ##去掉第一个"."右边的所有元素
		done
```
打印：当前目录下的各个文件名（去掉文件后缀）

### 4.字符串替换（这是我看过最好记的字符串教学）

引用来自：
http://bbs.chinaunix.net/viewthread.php?tid=218853&extra=&page=7#pid1628522

首先定义：file=/dir1/dir2/dir3/my.file.txt

我們可以用 ${ } 分別替換獲得不同的值：

${file#*/}：拿掉第一條 / 及其左邊的字串：dir1/dir2/dir3/my.file.txt

${file##*/}：拿掉最後一條 / 及其左邊的字串：my.file.txt

${file#*.}：拿掉第一個 . 及其左邊的字串：file.txt

${file##*.}：拿掉最後一個 . 及其左邊的字串：txt

${file%/*}：拿掉最後條 / 及其右邊的字串：/dir1/dir2/dir3

${file%%/*}：拿掉第一條 / 及其右邊的字串：(空值)

${file%.*}：拿掉最後一個 . 及其右邊的字串：/dir1/dir2/dir3/my.file

${file%%.*}：拿掉第一個 . 及其右邊的字串：/dir1/dir2/dir3/my

記憶的方法為：

"#" 是去掉左邊(在鑑盤上 # 在 $ 之左邊)

"%" 是去掉右邊(在鑑盤上 % 在 $ 之右邊)

單一符號是最小匹配﹔兩個符號是最大匹配。

### 5.判断语句常见错误

```
错误示范：
if [ $str == 0000 ]
	then
		echo "DONE 0000"
	    mv $filename 2.MOV
	fi

```
在进行上述字符串判断语句的时候，虽然程序运行正常，该判断的还是会判断，但脚本会出现如下报错："[: =: unary operator expected"

强迫症的我真的忍不了，后来查明原因，在进行变量判断的时候，如果str为空，就变成了“[ == 0000”，这样不仅不相等且少了一个"["，因此会出现上述报错。在例子中由于我的str没有空的情况，故能正常执行。但为了程序严谨，正确代码如下：
```
正确示范：
if [[ $str == 0000 ]]
	then
		echo "DONE 0000"
	    mv $filename 2.MOV
	fi

```

本系列将不定期更新。港真，shell脚本值得每个程序员花时间去学习，本人学疏才浅，但在这两个月里断断续续开发的几个脚本里已经慢慢感受到其简单语句里隐藏的无穷力量。