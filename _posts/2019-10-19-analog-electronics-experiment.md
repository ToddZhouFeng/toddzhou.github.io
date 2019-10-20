---
layout: post
title:  模电实验总结
date:   2019-10-18 18:22:00 +0800
categories: document
tag: [Analog Electronics]
music-id: 481853665
---

> 跪求模电实验放过我~

<!-- more -->

# 吐槽

模电实验是个坑，调试半天蛋真疼。（原谅我的粗言秽语……）



# 单极放大电路的设计

这里主要讨论的是分压式电流负反馈偏置放大电路的设计（如图）

![分压式电流负反馈偏置放大电路](http://image.cntronics.com/kbupload/kb_1358308660.jpg )

## 设计静态工作点

先确定静态工作点 $I_{CQ}, V_{CEQ}$ （$I_{BQ} = I_{CQ}/(1+\beta)$），先想好你要多大范围内的 $I_{CQ}, U_{CEQ}$，比如对于 9013，它的输出特性曲线为：

![输出特性曲线](http://cache.amobbs.com/bbs_upload782111/files_25/ourdev_529061.GIF)

对于小信号， $I_{CQ}$ 大概是 0.2~2 mA，$V_{CEQ}$ 在图中对应 1.5~4 V

如果对输入电阻有要求，则根据 $R_i \approx r_{be} = r_{bb'} + \beta \dfrac{26}{I_{CQ}}$ 来确定 $I_{CQ}$ （$r_{bb'}$ 在低频时取 300Ω，高频时取 50Ω）

如果对输入的动态范围有要求，那么还要有 $V_{CEQ} \geq V_{om} + V_{CE(sat)}$ （一般比较难截止，主要是防饱和）



## 选取电源

电源电压 $V_{CC}$ 主要是防止出现截止失真，因此要满足：$V_{CC} \geq 2(V_{om} + V_{CE(sat)}) + V_{EQ}$ （注：$V_{om}$ 是最大输出不失真峰电压，$V_{CE(sat)}$ 是三极管饱和压降）



## 选择电阻

分压式电流负反馈偏置放大电路中，$R_{B1}, R_{B2}$ 主要是用来钳位（固定 B极电压），并且要想达到效果，要使 $R_{B1}$ 上的电流 $I_1 >> I_{BQ}$，$V_{BQ} >> V_{BEQ}$。硅管一般 $I_1 = (5\text{~}10) I_{BQ}$，锗则是 $I_1 = (10\text{~}20) I_{BQ}$。而 $V_{BQ} = (\frac{1}{5} \text{~} \frac{1}{3}) V_{CC}$ ，并且 $V_{BQ}$ 取大一些，则 $R_{E1}, R_{E2}$ 也要取大一些，会让负反馈较深，电路更稳定，但也会使三极管容易饱和。根据这些可以求出$R_{B1}, R_{B2}$

![](http://image.cntronics.com/kbupload/kb_1358308660.jpg)

射极电阻就容易求了：$R_E = \dfrac{V_{BQ} - V_{BE}}{I_{CQ}}$

集电极电阻要满足放大倍数 $A_v = -\dfrac{\beta}{r_{be}+(1+\beta) R_{E1}} (R_C \parallel R_L)$，同时也要满足 $V_{CEQ}=V_{CC} - I_C \cdot (R_C + R_E)$



## 选择电容

耦合电容应满足：
$$
C_1 \geq (3 \text{~} 10) / [ 2\pi f_L (R_S + R_i)]\\
C_2 \geq (3 \text{~} 10) / [ 2\pi f_L (R_C + R_L)]
$$
旁路电容应满足：
$$
C_E \geq (1 \text{~} 3) / (2\pi f_L R_{E1})
$$
其中，$f_L$ 为下限频率



## 指标核算 

计算：
$$
A_v = -\dfrac{\beta}{r_{be}+(1+\beta) R_{E1}} (R_C \parallel R_L)
$$

$$
R_i = R_{B1} \parallel R_{B2} \parallel [r_{be}+(1+\beta)R_{E1}]
$$

$$
R_o = R_C
$$

# 调试

要调试得快，就必须对各个量之间得影响了如指掌，比如：
$$
A_v \uparrow \rightarrow R_{E1} \downarrow R_C \uparrow \\
R_i \uparrow \rightarrow R_{E1} \uparrow R_{B1} \uparrow R_{B2} \uparrow \\
U_{CEQ} \uparrow \rightarrow R_C \uparrow R_E \downarrow V_{BB} \uparrow
$$
下面总结遇到各种情况应该调哪个：

* 放大倍数不够：

  减小 $R_{E1}$ ，副作用是输入电阻减小

  增大 $R_C$，副作用是 $U_{BEW}$ 减小

* $U_{CEQ}$ 过小（过大则反过来）

  减小 $R_{E1}$ 

  增大 $R_C$

* $f_L$ 过高：

  增大耦合电容和旁路电容

* $f_H$ 过低：

  在负载端增加 $C_o$

  