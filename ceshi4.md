#! https://zhuanlan.zhihu.com/p/644912059
# 混合颜色带

混合颜色带一共有两个“带”，说到底都是为了控制当前图层哪些像素点变透明

## 混合颜色带如何打开

双击图层，即可打开

## 混合颜色带的依据

依据就是下方图层像素各个通道的取值范围

### RGB模式

例如在RGB模式下，选择红色，则此时按照红色通道的取值范围，对本图层的像素点进行判断，判断是否该将其置为透明

#### 本图层像素点决定本图层

上方的带，在两个滑块中间的就是要保留的像素，在滑块两端的就是要变透明的像素

$$Pix_{当前图层}=\left\{\begin{aligned}
    &透明&本图层像素在滑块内\\
    &不透明&本图层像素在滑块外\\
\end{aligned}\right.$$

#### 下方图层像素点决定本图层

下方的带，下方像素点如果在滑块之外，则该像素点上方也就是本图层像素点，将变透明
   $$Pix_{当前图层}=\left\{\begin{aligned}
    &透明&下方像素在下方带在滑块外\\
    &不透明&下方像素在下方带在滑块内\\
\end{aligned}\right.$$

#### 红色，绿色，蓝色

红绿蓝都是严格按照自身大小来判断
例如我们来验证一下

上方图层是这样
![Image](https://pic4.zhimg.com/80/v2-3236b583e0c769a69bbf5e11c01dd60e.png)

下方图层是这样
![Image](https://pic4.zhimg.com/80/v2-755ffd6c62cef6b3502f1d8a69d5211a.png)
这些是灰色，RGB，CMYK
如果我们选择红色
我们拖动上方滑块，选择范围$80⇒220$，通道数值不属于这个范围的，就会变透明
![Image](https://pic4.zhimg.com/80/v2-48ddd99aca8f3af46418921b5906356a.png)

如果我们将滑块分开，变成两个小三角(按住Alt或option单击)
第一组三角的范围是$[60,80]$
第二组三角的范围是$[220,230]$
我们可以看到透明度会产生一个渐变

![Image](https://pic4.zhimg.com/80/v2-ba61cedac80144a6b5eebfe80580d7f0.png)

如果操作下方滑块，则
范围也是$[80,220]$,则下方图层中，不在这个范围内的像素点，上方的像素变透明
![Image](https://pic4.zhimg.com/80/v2-43015e04767da7a3368057650bf46be9.png)
同样，如果我们将滑块分开，变成两个小三角(按住Alt或option单击)
![Image](https://pic4.zhimg.com/80/v2-e193ef0a94245f85bf29bbb814372351.png)
结果也和上方一样，透明产生渐变

#### 灰色

但是灰色要怎么处理

我们随便拉一下灰色看看

蓝色刚好消失，此时灰色在29的位置
![Image](https://pic4.zhimg.com/80/v2-db059d2623a4ad3fdd12243c63dbf5eb.png)
红色消失在77的位置
![Image](https://pic4.zhimg.com/80/v2-4bd96ba390c5e3da2dfe32e59f4964df.png)
绿色是151
![Image](https://pic4.zhimg.com/80/v2-4c124a3c4cb89df92f06d35db6f4cdff.png)
$$\dfrac{30}{29+77+151}\approx 0.11$$

$$\dfrac{77}{29+77+151}\approx 0.29$$

$$\dfrac{151}{29+77+151}\approx 0.59$$

于是按照这个比例
$$gray = 0.3r+0.59g+0.11b$$
我们只需计算出当前颜色的灰度值即可，按照上面这个公式，其他和单色情况一样。
