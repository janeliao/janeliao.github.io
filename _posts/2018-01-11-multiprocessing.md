---
layout:     post
title:      python 多线程并行加速 for
subtitle:   并行计算初探
date:       2018-01-11
author:     Siyuan
header-img: img/in-posts/algorithm.jpg
catalog: 	true
tags:
    -  python
    -  并行
---



在[前一篇文章](http://www.siyuanxu.com/2018/01/11/monte-carlo/)里，我借助蒙特卡洛法对圆周率进行了估算，结果还是比较满意的，但是在试算过程中有一个问题很 annoying。更准确的结果需要更多的随机点，更多随机点意味着更大的矩阵计算，而且我在计算收敛情况时，需要多次执行求 pi 函数，但是我的 7700k 在计算时很不慌张，占有率仅在15%附近徘徊。矩阵乘法的并行加速我暂时还没有时间弄清楚，但是 for 的加速还是比较简单的，直接使用 multiprocessing 。代码如下：

~~~python
import numpy as np
from multiprocessing import Pool # 引用线程池

def mc_pi(size):
    ran_x = np.random.random([size, size])
    ran_y = np.random.random([size, size])

    is_c = ran_x**2 + ran_y**2
    is_c[is_c > 1] = 0
    pi = 4 * np.count_nonzero(is_c) / (size**2)
    return pi

if __name__ == '__main__':
    size = 2000
    sizes = np.arange(10, size, 10)
    pool = Pool(8) # 创建线程池，小于等于 cpu 的线程
    pis = pool.map(mc_pi, sizes) # 计算函数和传入参数的 list 或 array 返回的是计算函数的返回值 list
~~~

multiprocessing 的好处时显而易见的，不需要像 thread 一样对代码进行修改。面对 for 循环，直接自动 map 到多个线程中，计算效率有比较大的提升。在上面的循环中，相比常规 for，很容易跑满 cpu，计算时间减少了一多半，从10.5s降低至4.5s。