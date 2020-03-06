---
layout: post
title: "TensorFlow 基础"
date: 2018-02-23
description: ""
tag: TensorFlow
---

# TensorFlow 基础

```本文将从张量与图、常数与变量还有占位符等基本概念出发简要介绍 TensorFlow。需要进一步了解 TensorFlow 的读者可以阅读谷歌 TensorFlow 的文档，当然也可以阅读其他中文教程或书籍，例如《TensorFlow：实战 Google 深度学习框架》和《TensorFlow 实战》等。```

## **一、图**

TensorFlow 是一种采用数据流图（data flow graphs），用于数值计算的开源软件库。其中 Tensor 代表传递的数据为张量（多维数组），Flow 代表使用计算图进行运算。数据流图用「结点」（nodes）和「边」（edges）组成的有向图来描述数学运算。「结点」一般用来表示施加的数学操作，但也可以表示数据输入的起点和输出的终点，或者是读取/写入持久变量（persistent variable）的终点。边表示结点之间的输入/输出关系。这些数据边可以传送维度可动态调整的多维数据数组，即张量（tensor）。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180224165436594.gif?raw=true)

在 Tensorflow 中，所有不同的变量和运算都是储存在计算图。所以在我们构建完模型所需要的图之后，还需要打开一个会话（Session）来运行整个计算图。在会话中，我们可以将所有计算分配到可用的 CPU 和 GPU 资源中。

```python
node1=tf.constant(3.0,tf.float32)
node2=tf.constant(4.0,tf.float32)
node3=tf.add(node1,node2)
```

```python
sess=tf.Session()
print("sess.run(node1,node2):",sess.run([node1,node2]))
print("sess.run(node3):",sess.run(node3))
```

如上所示我们构建了一个加法运算的计算图，第二个代码块并不会输出计算结果，因为我们只是定义了一张图，而没有运行它。第三个代码块才会输出计算结果，因为我们需要创建一个会话（Session）才能管理 TensorFlow 运行时的所有资源。但计算完毕后需要关闭会话来帮助系统回收资源，不然就会出现资源泄漏的问题。

TensorFlow 中最基本的单位是常量（Constant）、变量（Variable）和占位符（Placeholder）。常量定义后值和维度不可变，变量定义后值可变而维度不可变。在神经网络中，变量一般可作为储存权重和其他信息的矩阵，而常量可作为储存超参数或其他结构信息的变量。在上面的计算图中，结点 1 和结点 2 都是定义的常量 tf.constant()。我们可以分别声明不同的常量（tf.constant()）和变量（tf.Variable()），其中 tf.float 和 tf.int 分别声明了不同的浮点型和整数型数据。


## **二、占位符和feed_dict**

TensorFlow 同样还支持占位符，占位符并没有初始值，它只会分配必要的内存。在会话中，占位符可以使用 feed_dict 馈送数据。

feed_dict 是一个字典，在字典中需要给出每一个用到的占位符的取值。在训练神经网络时需要每次提供一个批量的训练样本，如果每次迭代选取的数据要通过常量表示，那么 TensorFlow 的计算图会非常大。因为每增加一个常量，TensorFlow 都会在计算图中增加一个结点。所以说拥有几百万次迭代的神经网络会拥有极其庞大的计算图，而占位符却可以解决这一点，它只会拥有占位符这一个结点。
```python
a = tf.placeholder(tf.float32)
b = tf.placeholder(tf.float32)
adder_node = a+b #provides a shortcut for tf.add(a,b)

print(sess.run(adder_node,feed_dict={a:3,b:4.5}))
print(sess.run(adder_node,feed_dict={a:[1,3],b:[2,4]}))
```

## **三、张量**

在 TensorFlow 中，张量是计算图执行运算的基本载体，我们需要计算的数据都以张量的形式储存或声明。如下所示，该教程给出了各阶张量的意义。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180224165813908.png?raw=true)

零阶张量就是我们熟悉的标量数字，它仅仅只表达了量的大小或性质而没有其它的描述。一阶张量即我们熟悉的向量，它不仅表达了线段量的大小，同时还表达了方向。一般来说二维向量可以表示平面中线段的量和方向，三维向量和表示空间中线段的量和方向。二阶张量即矩阵，我们可以看作是填满数字的一个表格，矩阵运算即一个表格和另外一个表格进行运算。当然理论上我们可以产生任意阶的张量，但在实际的机器学习算法运算中，我们使用得最多的还是一阶张量（向量）和二阶张量（矩阵）。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180224170014765.png?raw=true)

一般来说，张量中每个元素的数据类型有以上几种，即浮点型和整数型，一般在神经网络中比较常用的是 32 位浮点型。

## **四、TensorFlow 机器**
在整个教程中，下面一张示意图将反复出现，这基本上是所有 TensorFlow 机器学习模型所遵循的构建流程，即构建计算图、馈送输入张量、更新权重并返回输出值。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180224170057185.png?raw=true)

在第一步使用 TensorFlow 构建计算图中，我们需要构建整个模型的架构。例如在神经网络模型中，我们需要从输入层开始构建整个神经网络的架构，包括隐藏层的数量、每一层神经元的数量、层级之间连接的情况与权重、整个网络每个神经元使用的激活函数等内容。此外，我们还需要配置整个训练、验证与测试的过程。例如在神经网络中，定义整个正向传播的过程与参数并设定学习率、正则化率和批量大小等各类训练超参数。第二步需要将训练数据或测试数据等馈送到模型中，TensorFlow 在这一步中一般需要打开一个会话（Session）来执行参数初始化和馈送数据等任务。例如在计算机视觉中，我们需要随机初始化整个模型参数数值，并将图像成批（图像数等于批量大小）地馈送到定义好的卷积神经网络中。第三步即更新权重并获取返回值，这个一般是控制训练过程与获得最终的预测结果。
