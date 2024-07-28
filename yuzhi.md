#! https://zhuanlan.zhihu.com/p/649904273
# PS中的阈值调整图层

阈值调整图层，简单来说，就是通过将一个像素点，三个通道的数值，计算为一个数值，并再对这个数值进行一个二分法，大于就是白色，小于就是黑色
简单来说就是
$$
Pix=\left\{\begin{aligned}
    白&&Sum(Pix)\ge 阈值\\\\
    黑&&Sum(Pix)< 阈值\\
\end{aligned}\right.$$
这是$Sum(Pix)$的计算方法，就是
$$
Sum(Pix)=0.3RC+0.59GC+0.11BC$$
![Image](https://pic4.zhimg.com/80/v2-28afef255eec5b3c67f374cd0abaa676.png)
