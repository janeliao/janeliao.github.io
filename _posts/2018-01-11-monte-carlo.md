---
layout:     post
title:      蒙特卡洛算法-101
subtitle:   使用随机算法计算圆周率
date:       2018-01-11
author:     Siyuan
header-img: img/in-posts/algorithm.jpg
catalog: 	true
tags:
    -  python
    -  algorithm
    -  Monte Carlo
---

随机算法在解决复杂问题时可以用极少的计算成本得到近似解析解的结果。今年，在国内岩土届，随机算法也有大量的应用，例如可靠度分析以及随机有限元方面。

蒙特卡洛法是一种应用相当广泛的随机算法，近代的随机有限元也是建立在此基础上的。今天笔者第一次接触 mc 算法，尝试计算了圆周率，得到了不错的结果。

## 想法

假设我们有一个正方形， 在正方形中有一圆形，为了简化表达，这里的圆是正方形的内切圆，两图形的位置如下：

![init](http://www.siyuanxu.com/img/in-posts/2018-01-11-monte-carlo/init.png)

图形是关于原点中心对称的，因此，我们着重关心第一象限的内容。蒙特卡罗方法基于这样的思想：假想你有一袋豆子，把豆子均匀地朝这个图形上撒，然后数这个图形之中有多少颗豆子，这个豆子的数目就是图形的面积。当你的豆子越小，撒的越多的时候，结果就越精确。借助计算机程序可以生成大量均匀分布坐标点，然后统计出图形内的点数，通过它们占总点数的比例和坐标点生成范围的面积就可以求出图形面积。

假如我们在正方形的内随机加入100个点，如下图，那么一般会有较多的点落在圆内，较少的点落在圆外。这时

![dot-10](http://www.siyuanxu.com/img/in-posts/2018-01-11-monte-carlo/dot-10.png)

那么随着我们点数的增加，随机点将会逐渐铺满整个面，那么相应的结果的准确率也会越来越高。下图是覆盖了10000个随机点的示意图。

![dot-10](http://www.siyuanxu.com/img/in-posts/2018-01-11-monte-carlo/dot-100.png)

在这一逼近过程中，收敛速度很快，我分别计算了从 100~$2\times10^6$ 个随机点的pi值，收敛情况如下表右图，最后一次计算得到的结果为 3.1431953738541956 。nicoguaro 使用蒙特卡罗方法估算π值，见下表左图. 放置30000个随机点后,π的估算值与真实值相差 0.07%。

| 计算过程                                     | 收敛过程                                     |
| ---------------------------------------- | ---------------------------------------- |
| ![dot-10](http://www.siyuanxu.com/img/in-posts/2018-01-11-monte-carlo/Pi_30K.gif) | ![dot-10](http://www.siyuanxu.com/img/in-posts/2018-01-11-monte-carlo/convergence.png) |

计算过程：由nicoguaro - 自己的作品，CC BY 3.0，https://commons.wikimedia.org/w/index.php?curid=14609430

但是，也不难看出，虽然蒙特卡洛法计算效率非常高，但是由于其基于随机，所以极难达到很高的精度，即使取10的9次方个随机点时，其结果也仅在前4位与圆周率吻合）。用蒙特卡罗方法近似计算圆周率的先天不足是：第一，计算机产生的随机数是受到存储格式的限制的，是离散的，并不能产生连续的任意实数；上述做法将平面分区成一个个网格，在空间也不是连续的，由此计算出来的面积当然与圆或多或少有差距。所以，这种随机算法在应用时也应进行一定的适用性考虑。

最后 ，附计算代码如下：

~~~python
import numpy as np

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
    pool = Pool(7)
    pis = pool.map(mc_pi, sizes.tolist())
~~~





