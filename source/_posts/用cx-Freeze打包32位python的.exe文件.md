---
title: 用cx-Freeze打包32位python的.exe文件
date: 2017-10-11 15:34:37
tags:
cover_img: https://i.loli.net/2018/06/30/5b36597aac617.jpg
categories: Python
---


前段时间写了一个软件,按照正常的套路用pyqt4写,用sqlite3来搭数据库.刚开始用pyinstaller来打包，但没想到的是甲方的电脑竟然是32位的window XP,在google上说只需要把在32位的虚拟机上安装32位的python以及相应的库就能正确用pyinstaller打包。但就我最近水逆的势头，想想肯定不会那么顺利。到最后还是弄不明白所有的库都换成32位了而且是在32位的虚拟机上用pyinstaller打包的，还是用不了，提示“不是有效的win32位应用”。
    
###   换坑cx-freeze
如果不是因为pyinstaller实在没办法打包32位软件，实在不建议用cx-freeze。

1.用cx-freeze打包的软件会把整个pyqt文件给搬过来，其中包括很多用不到的模块，因此整个包会变得十分十分臃肿。

2.对pillow库不识别
    
3.cx-freeze对sqlite3数据库非常得不友好
   
 
下面专门讲讲最后两种情况
   
###   不识别pillow库
    
```
  File "build\bdist.win32\egg\pkg_resources\__init__.py", line 1854, in get_resource_filename
NotImplementedError: resource_filename() only supported for .egg, not .zip
```

出现如上报错，或者说pillow的路径找不到（windowError 3）

是因为用pip安装的pillow库是以.egg存在,在cx-freeze中识别不了,可以自己去官网下载zip自行安装。

参考[Pillow 2.9 and cx_Freeze](https://github.com/python-pillow/Pillow/issues/1438)
    
    
### cx-freeze对sqlite3的相关问题

1.
```
File "C:\Python27\lib\site-packages\cx_Freeze\hooks.py", line 826, in load_sqlite3
  dll_path = os.path.join(sys.base_prefix, "DLLs", dll_name)
```
假如出现了如上的报错信息，莫慌，不是你的错。。。
    
因为目前对python2的相关内容没有完善，据说在python3中已经完善这个问题了。目标文件hooks.py文件中识别不了base_prefix,解决问题的方法只有自己取定义这个base_prefix了。
    

只需要在该文件开头加上
    
```
base_prefix = getattr(sys,'base_prefix', getattr(sys, 'real_prefix', sys.prefix))
```
    
另外需要注意把826行的"sys.base_prefix"改成"base_prefix"
    
    
2.

在把如上的问题解决完之后就能如常地打包软件,感觉一切顺利,提前交付,终于能过上好日子了。。。终归还是感觉
    
在实际运行的时候把cx_freeze的命令窗口打开，你会很明显地看到提示：“drive not loaded”！！！数据库驱动没有启动，所有数据库相关的操作都动不了。
    
google百度了很久发现是因为本身cx_freeze在对pyqt4打包的时候会缺少相关的dll文件，我们需要在打包完成的工作目录下的PyQt4\plugins文件夹中添加sqldrivers相关文件
        
1.在打包好的文件目录的PyQt4\plugins中创建一个名为"sqldrivers"文件夹
        
2.复制python扩展包目录(E:\Python27\Lib\site-packages\PyQt4\plugins\sqldrivers)下的内容
        
3.全部粘贴到工作目录创建好的文件夹中
        
4.保险起见,我还在该目录中添加了"qsqlite4.dll" [点我下载](http://www.jb51.net/dll/qtsql4.dll.html#download)
    