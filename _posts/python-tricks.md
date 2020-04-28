---
layout: post
title: 'Python一些使用常识'
author: envy
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: ubuntuPython
---

## Python多线程多进程问题
参考链接https://zhuanlan.zhihu.com/p/24311810

### multiprocessing.Pool

因为 GIL 的缘故 threading 不能用，那么我们就好好研究研究 [multiprocessing](https://link.zhihu.com/?target=https%3A//docs.python.org/2/library/multiprocessing.html)。（当然，如果你说你不用 CPython，没有 GIL 的问题，那也是极佳的。）

首先介绍一个简单粗暴，非常实用的工具，就是 [multiprocessing.Pool](https://link.zhihu.com/?target=https%3A//docs.python.org/2/library/multiprocessing.html%23module-multiprocessing.pool)。如果你的任务能用 ys = map(f, xs) 来解决，大家可能都知道，这样的形式天生就是最容易并行的，那么在 Python 里面并行计算这个任务真是再简单不过了。举个例子，把每个数都平方：

```python
import multiprocessing

def f(x):
    return x * x

cores = multiprocessing.cpu_count()
pool = multiprocessing.Pool(processes=cores)
xs = range(5)

# method 1: map
print pool.map(f, xs)  # prints [0, 1, 4, 9, 16]

# method 2: imap
for y in pool.imap(f, xs):
    print y            # 0, 1, 4, 9, 16, respectively

# method 3: imap_unordered
for y in pool.imap_unordered(f, xs):
    print(y)           # may be in any order
```

map 直接返回列表，而 i 开头的两个函数返回的是迭代器；imap_unordered 返回的是无序的。

当计算时间比较长的时候，我们可能想要加上一个进度条，这个时候 i 系列的好处就体现出来了。另外，有一个小技巧，就是输出 \r 可以使得光标回到行首而不换行，这样就可以制作简易的进度条了。

```python
cnt = 0
for _ in pool.imap_unordered(f, xs):
    sys.stdout.write('done %d/%d\r' % (cnt, len(xs)))
    cnt += 1
```

![image-20200428135746615](../assets/img/2019-8-14-python-tricks/image-20200428135746615.png)