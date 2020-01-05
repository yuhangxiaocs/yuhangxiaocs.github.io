---
layout: post
title:  "first post"
---

$f(x,\theta)$中的$\theta$是参数，另一类用来定义模型结构或权重的称为超参数hyper-parameter，超参数可以理解为参数的参数，比如

$$L(y, f(x,\theta))+\lambda|L_1|$$

其中的$\lambda$就是一个超参数，这个超参数的选取有很大的经验性，因为它不是自动学到的，而是手工设置的。

------

批量梯度下降和随机梯度下降之间的区别在于每次迭代的优化目标是对**所有样本的平均损失函数**还是**单个样本的损失函数**。随机梯度下降因为实现简单，收敛速度也非常快，因此使用非常广泛。随机梯度下降相当于在批量梯度下降 的梯度上引入了随机噪声。当目标函数非凸时，反而可以使其逃离局部最优点。

随机梯度下降法的一个缺点是无法充分利用计算机的并行 计算能力。**小批量梯度下降法**（Mini-Batch Gradient Descent）是批量梯度下 降和随机梯度下降的折中。每次迭代时，我们随机选取**一小部分训练样本**来计 算梯度并更新参数，这样既可以兼顾随机梯度下降法的优点，也可以提高训练 效率。

------



- 对标量求导

  - 向量对标量求导，相当于每个元素对标量求导，结果仍是向量：

    $$\frac{\partial y}{\partial x} = (\frac{\partial y_1}{\partial x}, \cdots, \frac{\partial y_n}{\partial x})$$

  - 矩阵对标量求导，相当于每个元素对标量求导，结果仍是矩阵

- 对向量求导

  - 标量对向量求导，就是所谓的梯度，对于标量函数 $f(X),X=(x_1, x_2,\dots,x_n)$ 

    $$\frac{\partial f}{\partial X} = \left(\frac{\partial f}{\partial x_1} ,\cdots, \frac{\partial f}{\partial x_n} \right)$$记做：$\nabla f$ 

  - 向量对向量求导，结果是矩阵，先通过转置化成行向量除以列向量，然后行向量每个元素分别对所有列向量求导，写成一列：

    $$\frac{\partial{WX^{T}}}{\partial{W}}=\frac{\partial{(WX^T)^T}}{\partial{W}}=\frac{\partial{X^TW}}{\partial{W}}=X$$



  ​	具体如下：

  ​	$$\nabla f = \frac{\partial f}{\partial x} = \frac{\partial f^T}{\partial x} = \left[\frac{\partial f_1}{\partial x},\cdots,\frac{\partial f_m}{\partial x} \right ] = \left[ \begin{array}{ccc} \frac{\partial f_{1}}{\partial x_1} &amp; \cdots &amp; \frac{\partial f_{m}}{\partial x_1} \\ \vdots&amp; \ddots &amp; \vdots \\ \frac{\partial f_{1}}{\partial x_n} &amp; \cdots &amp; \frac{\partial f_{m}}{\partial x_n} \\ \end{array} \right]$$

  - 矩阵对象量求导



------

生成模型，结果是概率分布密度
判别模型，结果是条件概率



埃尔米特矩阵（英语：Hermitian matrix，又译作厄米特矩阵，厄米矩阵），也称自伴随矩阵，是共轭对称的方阵。埃尔米特矩阵中每一个第i行第j列的元素都与第j行第i列的元素的复共轭。

显然，埃尔米特矩阵主对角线上的元素都是实数的，其特征值也是实数。对于只包含实数元素的矩阵（实矩阵），如果它是对称阵，即所有元素关于主对角线对称，那么它也是埃尔米特矩阵。也就是说，实对称矩阵是埃尔米特矩阵的特例。