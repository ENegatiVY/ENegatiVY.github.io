---
layout: post
title: 'Ubuntu配置小结'
date: 2019-07-24
author: envy
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: ubuntu
---

## Ubuntu下命令行翻墙
参考链接 https://blog.csdn.net/Jesse_Mx/article/details/52863204
防链接失效搜索关键词： polipo



## Error: retrieving gpg key timed out错误

https://www.jianshu.com/p/e2a15336f174

用浏览器去官方网站找到信息手动添加到系统里面



## sublime 配置

在线自动安装试了很多都不行

https://zhuanlan.zhihu.com/p/42473428



## 安装cudnn

cudnn提供两种包，一种是解压包tar.gz，一种是安装包deb。

我用deb安装之后，弹出一些字之后就没有结果了，

用

```cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2```

查看版本显示找不到文件。

所以我选用了https://blog.csdn.net/ture_dream/article/details/52677619的方法直接拷贝。



```
 sudo cp cuda/lib64/* /usr/local/cuda/lib64

 sudo cp cuda/include/* /usr/local/cuda/include
```

其中cuda是解压tar后的根目录

然后

```cd /usr/local/cuda/lib64```

```sudo chmod 777 libcudnn*```

 目前先用着，别的暂不论

