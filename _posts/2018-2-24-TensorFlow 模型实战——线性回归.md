---
layout: post
title: "TensorFlow 模型实战——线性回归"
date: 2018-02-23
description: ""
tag: TensorFlow
---
# TensorFlow 模型实战

## **TensorFlow 线性回归**

该教程前面介绍了很多线性回归的基本概念，包括直线拟合、损失函数、梯度下降等基础内容。我们一直认为线性回归是理解机器学习最好的入门模型，因为他的原理和概念十分简单，但又基本涉及到了机器学习的各个过程。总的来说，线性回归模型可以用下图概括：

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180224172135783.png?raw=true)

其中「×」为数据点，我们需要找到一条直线以最好地拟合这些数据点。该直线和这些数据点之间的距离即损失函数，所以我们希望找到一条能令损失函数最小的直线。以下是使用 TensorFlow 构建线性回归的简单案例。

### **1、构建目标函数(即「直线」)**
目标函数即 `H(x)=Wx+b`，其中`x`为特征向量、`W`为特征向量中每个元素对应的权重、`b`为偏置项。
```python
# X and Y data
x_train=[1,2,3]
y_train=[1,2,3]

W=tf.Variable(tf.random_normal([1]),name='weight')
b=tf.Variable(tf.random_normal([1]),name='bias')
# Our hypothesis XW+b
hypothesis = x_train*W+b
```
如上所示，我们定义了 y=wx+b 的运算，即我们需要拟合的一条直线。

### **2、构建损失函数**
下面我们需要构建整个模型的损失函数，即各数据点到该直线的距离，这里我们构建的损失函数为均方误差函数：
$$cost(W,b)=\frac {1}{m}\sum_{i=1}^{m}(H(x^{(i)})-y^{(i)})^2$$
该函数表明根据数据点预测的值和该数据点真实值之间的距离，我们可以使用以下代码实现：
```python
# cost/loss function
cost = tf.reduce_mean(tf.square(hypothesis - y_train))
```
其中 tf.square() 为取某个数的平方，而 tf.reduce_mean() 为取均值。

### **3、采用梯度下降更新权重**
```python
# Minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)
```
为了寻找能拟合数据的最好直线，我们需要最小化损失函数，即数据与直线之间的距离，因此我们可以采用梯度下降算法。

### **4、运行计算图执行训练**
```python
# Launch the graph in a session.
sess = tf.Session()
# Initializes global variables in the graph.
sess.run(tf.global_variables_initializer())

# Fit the line
for step in range(2001):
sess.run(train)
if step % 20 == 0:
print(step, sess.run(cost), sess.run(W), sess.run(b))
```
上面的代码打开了一个会话并执行变量初始化和馈送数据。

### **5、结束——附完整代码**
```python
import tensorflow as tf
W = tf.Variable(tf.random_normal([1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')

X = tf.placeholder(tf.float32, shape=[None])
Y = tf.placeholder(tf.float32, shape=[None])

# Our hypothesis XW+b
hypothesis = X * W + b
# cost/loss function
cost = tf.reduce_mean(tf.square(hypothesis - Y))
# Minimize
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)

# Launch the graph in a session.
sess = tf.Session()
# Initializes global variables in the graph.
sess.run(tf.global_variables_initializer())

# Fit the line
for step in range(2001):
    cost_val, W_val, b_val, _ = sess.run([cost, W, b, train],feed_dict={X: [1, 2, 3], Y: [1, 2, 3]})
    if step % 20 == 0:
        print(step, cost_val, W_val, b_val)
```
