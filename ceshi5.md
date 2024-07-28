# ! <https://zhuanlan.zhihu.com/p/644812215>

# 柔光模式来历

柔光模式是最复杂的一种混合模式，也是最巧妙的一种混合模式，柔光模式的本质是伽马矫正(gama correction)。配合混合图层的像素点的通道数值，再对原图层使用伽马矫正，二者通过配合就可以得到柔光模式。

如果我们想了解柔光模式，首先必须了解什么是伽马矫正，伽马矫正，简单来说就是，将原像素通道数值通过幂次方的方式进行修改，比如平方和根号，例如我们由一个归一化之后为0.5的通道数值，我们对其进行系数为2的伽马矫正，则结果是$0.5^{\dfrac{1}{2}}=\sqrt{0.5}$,如果进行系数为$\dfrac{1}{2}$的伽马矫正，则结果为$0.5^2$

### 公式

$$\begin{aligned}r&= SoftLight(b,a)\\\\&=\left\{\begin{aligned}&(2a-1)(b^2-b)+b&a\leq0.5\\&(2a-1)(\sqrt{b}-b)+b&a>0.5\end{aligned}\right.\end{aligned}$$

伽马矫正的数学表达式$$output = input^{\dfrac{1}{gama}}$$

其中$input$代表输入信号，$output$代表输出，$gama$代表伽马系数

系数为$2$的伽马矫正

$$output= input^{\dfrac{1}{2}}$$

系数为$\dfrac{1}{2}$的伽马矫正

$$output= input^{2}$$

![Image](https://pic4.zhimg.com/80/v2-5883b4328464f28318be6e1601ca2452.jpg)

于是对于系数为$\dfrac{1}{2}$的伽马矫正，稍微变换一下表达式

$$r =b^2= -(b-b^2) +b$$

然后再使用$(2a-1)$作为系数乘以差值项

$$r = (2a-1)(b-b^2) +b$$

![Image](https://pic4.zhimg.com/80/v2-9a0ac20c1195890ae269ae59a5a94c47.jpg)

于是对于系数为$2$的伽马矫正，稍微变换一下表达式

$$r =\sqrt{b}= (\sqrt{b}-b) +b$$

然后再使用$(2a-1)$作为系数乘以差值项

$$r =(2a-1)(\sqrt{b}-b) +b$$

再将结果合并，我们就可以得到柔光模式的表达式。

如果使用一句话概括柔光模式的数学表达式，就是“以混合图层为系数的系数为$\dfrac{1}{2}$和$2$的伽马矫正”

在 PS中伽马矫正可以在色阶工具和曝光度工具中找到👀

### 映射面和同图等效曲线

![Image](https://pic4.zhimg.com/80/v2-07b96372a38bce7f9cb1312ffef9f6d2.jpg)

### 程序模拟该模式计算结果

```java
    // 柔光
public static BlendColor SoftLight(BlendColor colorBase, BlendColor colorBlend, double fill, double opacity) {
    double red = SoftLightChannel(colorBase.red.get01Value(), colorBlend.red.get01Value(), fill);
    double green = SoftLightChannel(colorBase.green.get01Value(), colorBlend.green.get01Value(), fill);
    double blue = SoftLightChannel(colorBase.blue.get01Value(), colorBlend.blue.get01Value(), fill);
    return ColorUtils.Opacity(colorBase, new BlendColor(red * 255, green * 255, blue * 255), opacity);
}

private static double SoftLightChannel(double baseValue, double blendValue, double fill) {
    if (blendValue <= 0.5) {
        return (baseValue + (2 * blendValue - 1) * (baseValue - baseValue * baseValue)) * fill
                    + (1 - fill) * baseValue;
    } else {
        return (baseValue + (2 * blendValue - 1) * (Math.sqrt(baseValue) - baseValue)) * fill
                    + (1 - fill) * baseValue;
        }
    }
```

```json
柔    光(SoftLight)     RGB[105.40,  74.06,  63.42]~ HSY[15.21,  41.98,  82.29 ]~ HSB[ 15.21,  39.83,  41.33]
```

### 验证

![Image](https://pic4.zhimg.com/80/v2-89c8d49b8123a259c70588385be98d53.png)

### 用途示例

1:同图混合增加图片对比度
2:配合中性灰平面，实现局部提亮和压暗（dodge and burn）
