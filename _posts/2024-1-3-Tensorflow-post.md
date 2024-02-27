---
layout: post
title: TensorFlow
category: 深度学习
thumbnail: /style/image/thumbnail.png
icon: book
---


* content
  {:toc}



[TOC]



## Graph

图描述了计算的过程，可以通过tensorboard图形化流程结构

- 声明
- 保存为pb文件
- 从pb中恢复Graph
- Tensorboard可视化

![image-20240123110214052](style/image/image-20240123110214052.png)

![image-20240123110530061](style/image/image-20240123110530061.png)

![image-20240123111541518](style/image/image-20240123111541518.png)

## Session

Graph是前端，实际误操作，Graph必须在称之为“会话”的上下文中执行，Session将图的op分发到诸如CPU或GPU之类的设备上执行。

```python
#创建和关闭对话
sess = tf.Session()
sess = tf.InteractiveSession()
with tf.Session() as Sess:
	...
sess.close()
```

![image-20240127142255218](style/image/image-20240127142255218.png)

![image-20240127142306909](style/image/image-20240127142306909.png)

![image-20240127142642439](style/image/image-20240127142642439.png)

## Tensor

在TensorFlow中，所有在节点之间传递的数据都为Tensor对象。

N维数组，图像：（batch * height * widhth * channel） 	

- #### tf.constant ()  常量

- #### tf.Variable()  变量

- #### tf.placeholder()  占位符

![image-20240127143725327](style/image/image-20240127143725327.png)

## Operation

![image-20240127144755538](style/image/image-20240127144755538.png)

## Feed

![image-20240127145002891](style/image/image-20240127145002891.png)

## Fetch

![image-20240127145123496](style/image/image-20240127145123496.png)

## 封装API

![image-20240130144506102](style/image/image-20240130144506102.png)

## 数据增强方法

因为数据训练需要大量的数据基础，进行大规模的训练，所以数据增强相当于创建了更多的数据样本。

![image-20240130153705933](style/image/image-20240130153705933.png)

## Tensorboard进行数据可视化

![image-20240130154503200](style/image/image-20240130154503200.png)

1.pip引入模块

2.对导出的log图进行路径填充

localhost:6060 可以看到可视化结果

注：tensorboard要配合tf.summary共同使用