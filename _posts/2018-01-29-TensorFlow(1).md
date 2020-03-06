---
layout: post
title: "TensorFlow 入门第一篇"
date: 2018-01-29
description: ""
tag: TensorFlow
---

## TensorFlow基础
 实际上编写TensorFlow可以总结为两步：
 1. 组装一个graph
 2. 使用session去执行graph中的operation
 因此，我们从graph和session说起。

### 1、graph与session


#### **（1）、计算图**

 TensorFlow是基于计算图的框架，因此理解graph与session显得尤为重要。不过在介绍graph和session之前，先来了解一下什么是计算图。假设我们有这样一个需要计算的表达式，该表达式包括了两个加法与一个乘法，为了更好的讲述引入中间变量c与d。由此该表达式可以表示为

 ![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180129171023477.png?raw=true)

当需要计算e时，就需要计算c与d,而计算c就需要计算a与b,计算d就需要计算b。这样就形成了依赖关系。这种有向无环图就叫做计算图，因为对于图中的每一个节点其微分都很容易得出，因此应用链式法则求得一个复杂的表达式的导数就成为可能，所以它会应用在类似TensorFlow这种需要应用反向传播算法的框架中。


#### **（2）、概念说明**


下面是graph、session、operation和tensor是个概念的简介
- Tensor：类型化的多位数组，图的边；
- Operation：执行计算的单元，图的节点；
- Graph：一张有边与点的图，其表示了需要进行计算的任务；
- Session：称之为会话的上下文，用于执行图。

Graph仅仅定义了所有的operation与tensor流向，没有进行任何计算。而session根据graph的定义分配资源，计算operation，得出结果。既然是图就会有点与边，在图计算中operation就是点而tensor就是边。operation可以是加减乘除等数学运算，也可以是各种各样的优化算法。每个operation都会有零个或多个输出。tensor就是其输入与输出，其可以表示一维二位多维向量或者常量。而且除了Variable指向的tensor外，所有的tensor在流入下一个节点后都不再保存

#### **（3）、举例**

下面首先定义一个图（其实没有必要，tensorflow会默认定义一个），并做一些计算。
```python
import tensorflow as tf
graph = tf.Graph()
with graph.as_default():
    foo = tf.Variable(3,name='foo')
    bar = tf.Variable(2,name='bar')
    result = foo + bar
    initialize = tf.global_variables_initializer()

print(result)  #Tensor("add:0", shape=(), dtype=int32)
```
这段代码，首先会载入tensorflow，定义一个graph类，并在这张图上定义了foo与bar的两个变量，最后对这个值求和，并初始化所有变量。其中，Variable是定义变量并赋予初值。让我们看下result（最后1行代码）。后面是输出，可以看到并没有输出实际的结果，由此可见在定义图的时候其实没有进行任何实际的计算。

下面定义一个session，并进行真正的计算。
```python
with tf.Session(graph=graph) as sess:
    sess.run(initialize)
    res = sess.run(result)
print(res)  # 5
```
这段代码中，定义了session，并在session中执行了真正的初始化，并且求得result的值并打印出来。可以看到，在session中产生了真正的计算，得出值为5。
下图是该graph在tensorboard中的显示。这张图整体是一个graph,其中foo,bar,add这些节点都是operation，而foo和bar与add连接边的就是tensor。当session运行result时，实际就是求得add这个operation流出的tensor值，那么add的所有上游节点都会进行计算，如果图中有非add上游节点（本例中没有）那么该节点将不会进行计算，这也是图计算的优势之一。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180129173040327.png?raw=true)


### 2、数据结构

Tensorflow的数据结构有着rank,shape,data types的概念，下面来分别讲解。
#### **（1）、rank**

Rank一般是指数据的维度，其与线性代数中的rank不是一个概念。其常用rank举例如下。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180129173221363.png?raw=true)

#### **（2）、shape**

Shape指tensor每个维度数据的个数，可以用python的list/tuple表示。下图表示了rank,shape的关系。
![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180129173314523.png?raw=true)

#### **（3）、data type**

Data type，是指单个数据的类型。常用DT_FLOAT，也就是32位的浮点数。下图表示了所有的types。

![图片加载失败](https://github.com/PengSi/pengsi.github.io/blob/master/_posts/assets/markdown-img-paste-20180129173854136.png?raw=true)

### 3、Variables

#### **(1)、介绍**

当训练模型时，需要使用Variables保存与更新参数。Variables会保存在内存当中，所有tensor一旦拥有Variables的指向就不会在session中丢失。其必须明确的初始化而且可以通过Saver保存到磁盘上。Variables可以通过Variables初始化。
```python
weights = tf.Variable(tf.random_normal([784, 200], stddev=0.35),name="weights")
biases = tf.Variable(tf.zeros([200]), name="biases")
```
其中，tf.random_normal是随机生成一个正态分布的tensor，其shape是第一个参数，stddev是其标准差。tf.zeros是生成一个全零的tensor。之后将这个tensor的值赋值给Variable。

#### **(2)、初始化**

实际在其初始化过程中做了很多的操作，比如初始化空间，赋初值（等价于tf.assign），并把Variable添加到graph中等操作。注意在计算前需要初始化所有的Variable。一般会在定义graph时定义global_variables_initializer，其会在session运算时初始化所有变量。

直接调用global_variables_initializer会初始化所有的Variable，如果仅想初始化部分Variable可以调用tf.variables_initializer。
```python
Init_ab = tf.variables_initializer([a,b],name=”init_ab”)
```
Variables可以通过eval显示其值，也可以通过assign进行赋值。Variables支持很多数学运算，具体可以参照官方文档。

#### **(3)、Variables与constant的区别**

值得注意的是Variables与constant的区别。Constant一般是常量，可以被赋值给Variables，constant保存在graph中，如果graph重复载入那么constant也会重复载入，其非常浪费资源，如非必要尽量不使用其保存大量数据。而Variables在每个session中都是单独保存的，甚至可以单独存在一个参数服务器上。可以通过代码观察到constant实际是保存在graph中，具体如下。
```python
const = tf.constant(1.0,name="constant")
print(tf.get_default_graph().as_graph_def())
```
这里第二行是打印出图的定义，其输出如下：
```python
node {
       name: "constant"
       op: "Const"
       attr {
          key: "dtype"
       value {
          type: DT_FLOAT
      }
  }
  attr {
    key: "value"
    value {
      tensor {
        dtype: DT_FLOAT
        tensor_shape {
        }
        float_val: 1.0
      }
    }
  }
}
versions {
  producer: 17
}
```
#### **(4)、命名**

另外一个值得注意的地方是尽量每一个变量都明确的命名，这样易于管理命令空间，而且在导入模型的时候不会造成不同模型之间的命名冲突，这样就可以在一张graph中容纳很多个模型。

### 4、placeholders与feed_dict

当我们定义一张graph时，有时候并不知道需要计算的值，比如模型的输入数据，其只有在训练与预测时才会有值。这时就需要placeholder与feed_dict的帮助。
定义一个placeholder，可以使用tf.placeholder(dtype,shape=None,name=None)函数。
```python
foo = tf.placeholder(tf.int32,shape=[1],name='foo')
bar = tf.constant(2,name='bar')
result = foo + bar
with tf.Session() as sess:
   print(sess.run(result))
```
在上面的代码中，会抛出错误（InvalidArgumentError），因为计算result需要foo的具体值，而在代码中并没有给出。这时候需要将实际值赋给foo。最后一行修改如下（其中最后的dict就是一个feed_dict，一般会使用python读入一些值后传入，当使用minbatch的情况下，每次输入的值都不同）：
```python
print(sess.run(result,{foo:[3]}))
```
