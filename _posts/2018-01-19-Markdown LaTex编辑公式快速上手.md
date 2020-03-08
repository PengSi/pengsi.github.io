---
layout: post
title: "MarkDown中输入公式"
date: 2018-01-19
description: "在学习数学知识的时候，在计算机上写公式是比较头疼的事情。使用LaTex可以在一定程度上缓解蛋疼的状况——最起码看起来还挺拉风。"
tag: MarkDown
---

# **LaTex 公式**
在学习数学知识的时候，在计算机上写公式是比较头疼的事情。使用LaTex可以在一定程度上缓解蛋疼的状况——最起码看起来还挺拉风。下面我们一起看一下它的基本用法。具体其他符号的用法，可以直接查看[http://latex.codecogs.com/eqneditor/editor.php](http://latex.codecogs.com/eqneditor/editor.php)
#### **一、通用语法**
使用双`$$`围住表达式，可以居中显示
#### **二、空格**
需要使用 `\qquad`,`\quad`,`\`,应该是占位符和变量之间需要有{}相隔。
```
$$ C_{1} \qquad {C_2} $$
$$ C_{1} \quad {C_2} $$
$$ C_{1} \ {C_2} $$
```
最终结果如下：
$$ C_{1} \qquad {C_2} $$
$$ C_{1} \quad {C_2} $$
$$ C_{1} \ {C_2} $$

#### **三、下标**
使用`_`表示下标
```
$$ C_{1} + {C_2}$$
$$C_{m,n}$$
```
效果如下：
$$ C_{1} + {C_2}$$
$$C_{m,n}$$
#### **四、上标**
使用`^`表示上标
```
$$ c_{1}^{2}=a^{2}+b^{2} $$
```
效果如下：
$$ c_{1}^{2}=a^{2}+b^{2} $$
#### **五、希腊字母**
```$$\lambda,\xi,\pi,\mu,\Phi,\Omega,\alpha, \beta, \gamma,\Gamma, \Delta $$```
效果如下：
$$\lambda,\Xi,\pi,\mu,\Phi,\Omega,\alpha, \beta, \gamma,\Gamma, \Delta $$
#### **六、值比较符**
如果是大于或者小于，等于可以直接使用`>`,`<`,`=`符号，如果是不等，大于等于和小于等于，用符号：```\geq,\neq,\leq```,例如：
```
$$e^{x^2} \neq  {e^x}^2$$

$$ 3>2$$
```
效果如下：
$$e^{x^2} \neq  {e^x}^2$$
$$ 3>2$$

#### **七、平方根**
使用`\sqrt`或 `\surd`
例如：
```
$$\sqrt{x+y}$$
$$\sqrt[3]{x^{2}+y}$$
$$\surd[x^2 + y^2] $$
```
效果如下：
$$\sqrt{x+y}$$
$$\sqrt[3]{x^{2}+y}$$
$$\surd[x^2 + y^2] $$

#### **八、水平线**
水平线用`\overline`,`\underline`分别表示上水平线和下水平线
例如：
```
$$\overline{m+n} \quad \underline{m+n}$$
```
效果如下：
$$\overline{m+n} \quad \underline{m+n}$$

#### **九、水平括号**
水平括号用`\overbrace`和`\underbrace`表示上下的括号
例如：
```
$$ \underbrace{a+b+\cdots+z}_{26}$$
```
效果如下：
$$ \underbrace{a+b+\cdots+z}_{26}$$

#### **十、重音号**
`\widetilde` 和 `\widehat`

```
$$y'=3\widetilde a$$
$$y'=3\widehat a$$
```
$$y'=3\widetilde a$$
$$y'=3\widehat a$$

#### **十一、向量**
`\overrightarrow`,`\overleftarrow`
例如：
```
$$\overrightarrow {AC} = \overleftarrow {AB} +\overrightarrow {BC} $$
```
效果如下：
$$\overrightarrow {AC} = \overleftarrow {BA} +\overrightarrow {BC} $$

#### **十二、圆点**
用`\cdot, \cdots,\dot,\ddot`表示圆点
```
$$ \dot  {a}$$
$$ {a}\cdot{b}$$
$$ \ddot {c} $$
```
效果如下：
$$ \dot  {a}$$
$$ {a}\cdot{b}$$
$$ \ddot {c} $$

#### **十三、函数名**
```
\arccos \cos \csc \arcsin \cosh \deg \arctan \cot \det \arg \coth \dim \sinh \sup \tan

\[\lim_{x \rightarrow 0} \frac{\sin x}{x}=1\]

\exp \ker \limsup \min \gcd \lg \ln \Pr \hom \lim \log \sec \inf \liminf \max \sin

\tanh
```
例如：
```
$$ \lim_{x \rightarrow 0} \frac{\sin x}{x} $$
```
效果如下：
$$ \lim_{x \rightarrow 0} \frac{\sin x}{x} $$


#### **十四、数学符**
`\mathbf`
```
$$ x^{2} \geq 0\qquad \textrm{for all }x\in\mathbf{R} $$
$$x^{2} \geq 0\qquad \textrm{for all }x\in\mathbb{R} $$
```
$$ x^{2} \geq 0\qquad \textrm{for all }x\in\mathbf{R} $$
$$x^{2} \geq 0\qquad \textrm{for all }x\in\mathbb{R} $$


#### **十五、二项系数**
`{... \choose ...}` 或 `{... \atop ...}`。第二个命令与第一个命令的输出相同,只是没有括号。
```
$${n\choose m} \qquad {x\atop y+2}$$
```
$${n\choose m} \qquad {x\atop y+2}$$


#### **十六、前缀符号**
`\int,\sum,\prod`
```
$$ {\int_{0}^{\frac{\pi}{2}}} \qquad \sum_{i=1}^{n} \qquad \prod_\epsilon$$
```
$$ {\int_{0}^{\frac{\pi}{2}}} \qquad \sum_{i=1}^{n} \qquad \prod_\epsilon$$


#### **十七、转义符号**
有时保留字需要加入`\`来进行转义
```
$${a,b,c}\neq\{a,b,c\}$$
```
$${a,b,c}\neq\{a,b,c\}$$


#### **十八、括号层次**
正确的括号大小`\left`和`\right`。如果将命令 `\left` 放在开分隔符前,TEX会自动决定分隔符的正确大小。注意必须用对应的右分隔符 `\right` 来关闭每一个左分隔符 `\left`,并且只有当这两个分隔符排在同一行时大小才会被正确确定。
```
$$ 1+\left(\frac {1}{1-x^2}\right) ^3 \qquad 1+(\frac {1}{1-x^2}) ^3$$
```
$$ 1+\left(\frac {1}{1-x^2}\right) ^3 \qquad 1+(\frac {1}{1-x^2}) ^3$$
另外也可以手工指出括号大小，使用`\big,\Big，\bigg`。
```
$$ \Big( (x+y) (x-y) \Big)^{2} $$
$$\big(\Big(\bigg(\Bigg($$
$$\big\}\Big\}\bigg\}\Bigg\} $$
$$\big\|\Big\|\bigg\|\Bigg\| $$
```
$$ \Big( (x+y) (x-y) \Big)^{2} $$
$$\big(\Big(\bigg(\Bigg($$
$$\big\}\Big\}\bigg\}\Bigg\} $$
$$\big\|\Big\|\bigg\|\Bigg\| $$


#### **十九、垂直对齐**
使用`array`命令，并`\\`命令来分行。注意转义
```
$$\mathbf{X} =
	\left( \begin{array}{ccc}
	x\_{11} & x\_{12} & \ldots \\\
	x\_{21} & x\_{22} & \ldots \\\
	\vdots & \vdots & \ddots
	\end{array} \right) $$
  ```
  $$\mathbf{X} =
	\left( \begin{array}{ccc}
	x\_{11} & x\_{12} & \ldots \\\
	x\_{21} & x\_{22} & \ldots \\\
	\vdots & \vdots & \ddots
	\end{array} \right) $$


#### **二十、分数**
`\frac{}{}` 或者直接写
```
$$\sin \alpha = \frac{a}{c} $$
$$x^{1/2} $$
```
$$\sin \alpha = \frac{a}{c} $$
$$x^{1/2} $$
