# 深度学习简单介绍

首先要简单区别几个概念：人工智能，机器学习，深度学习，神经网络。这几个词应该是出现的最为频繁的，但是他们有什么区别呢？

**人工智能**：人类通过直觉可以解决的问题，如：自然语言理解，图像识别，语音识别等，计算机很难解决，而人工智能就是要解决这类问题。

**机器学习**：如果一个任务可以在任务T上，随着经验E的增加，效果P也随之增加，那么就认为这个程序可以从经验中学习。

**深度学习**：其核心就是自动将简单的特征组合成更加复杂的特征，并用这些特征解决问题。

**神经网络**：最初是一个生物学的概念，一般是指大脑神经元，触点，细胞等组成的网络，用于产生意识，帮助生物思考和行动，后来人工智能受神经网络的启发，发展出了人工神经网络。

来一张图就比较清楚了，如下图：

![img](https://images2017.cnblogs.com/blog/36692/201708/36692-20170827094211730-2059009135.png)

## 拟合数据



```python
import tensorflow as tf
import numpy as np

### 设置答案
# 使用 NumPy 生成假数据(phony data), 总共 100 个点.
x_data = np.float32(np.random.rand(2, 100)) # 随机输入
y_data = np.dot([0.100, 0.200], x_data) + 0.300


### 构造数据的容器和流程的模型
# 构造一个线性模型
# 
b = tf.Variable(tf.zeros([1]))
W = tf.Variable(tf.random_uniform([1, 2], -1.0, 1.0))
y = tf.matmul(W, x_data) + b

### 设置误差值和调整方向
# 最小化方差
loss = tf.reduce_mean(tf.square(y - y_data)) ### 
optimizer = tf.train.GradientDescentOptimizer(0.5)
train = optimizer.minimize(loss)

### 变量们打包给到对象init
# 初始化变量
init = tf.initialize_all_variables()

### 创建会话session对象
# 启动图 (graph)
sess = tf.Session()
sess.run(init)

### 循环里跑optimizer
# 拟合平面
for step in xrange(0, 201):
    sess.run(train)
    if step % 20 == 0:
        print step, sess.run(W), sess.run(b)
```

## MNIST

```python
import input_data
mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)

import tensorflow as tf
### 容器们
x = tf.placeholder("float", [None, 784])
W = tf.Variable(tf.zeros([784,10]))
b = tf.Variable(tf.zeros([10]))
y = tf.nn.softmax(tf.matmul(x,W) + b)
y_ = tf.placeholder("float", [None,10])

### 误差值与调整方向
cross_entropy = -tf.reduce_sum(y_*tf.log(y))
train_step = tf.train.GradientDescentOptimizer(0.01).minimize(cross_entropy)

### 打包变量传递给init
init = tf.initialize_all_variables()

### 开始会话
sess = tf.Session()
sess.run(init)

# vars , optimizer--initialize_all_variables--->init--run-->Session

# Session --run-->optimizer,dict_dict{placeholder:param}


#### 到目前为止都是传递的空容器
for i in range(1000):
  batch_xs, batch_ys = mnist.train.next_batch(100)
  sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})
```

# 激励函数

## 定义激励层

```python
import tensorflow as tf

def add_layer(inputs,in_size,out_size,activation_func=None):
    Weights = tf.Variable(tf.random_normal([in_szie,out_size]))
    biaes = tf.Variable(tf.zeros([1,out_size])+0.1)
    Wx_plus_b =tf.matmul(inputs,Weights)+biases
    
    if activation_func is None:
        outpusts = Wx_plus_b
	else:
		outpust=activation_func(Wx_plus_b)
     return output
    
```

