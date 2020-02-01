---
date: 2020-02-01 16:15:05
categories:
   - 机器学习
tags:
   - 吴恩达视频笔记
mathjax: true
---
# 模型
神经网络实现and xor or
用Logistic

<!--more-->



# 神经网络

Logistic带正则的损失函数

$$
\begin{equation}
\begin{split}

J(θ)=&-\frac{1}{m}[\sum_{i=1}^{m} y^{(i)}log(h_θ(x^{(i)}))+(1-y^{(i)})log(1-h_θ(x^{(i)}))] \\
&+\frac{\lambda }{2m}\sum_{j=1}^{n}{\theta_j} ^2

\end{split}
\end{equation}
$$
神经网络的损失函数为
$$
\begin{equation}
\begin{split}
h_\Theta(x) \in \mathbb{R}^K    \rightarrow  (h_\Theta(x))_i=i^{th} output
\end{split}
\end{equation}
$$

$$
\begin{equation}
\begin{split}

J(\Theta)=&-\frac{1}{m}[\sum_{i=1}^{m}  \sum_{k=1}^{K} y_k^{(i)}log(h_\Theta(x^{(i)}))_k+(1-y_k^{(i)})log(1-h_\Theta(x^{(i)}))_k] \\
&+
\frac{\lambda }{2m}
\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_{l+1}}{(\Theta_{ji}^{(l)})} ^2

\end{split}
\end{equation}
$$

求极小值
$$
\begin{equation}
\begin{split}

J(\Theta)=&-\frac{1}{m}[\sum_{i=1}^{m}  \sum_{k=1}^{K} y_k^{(i)}log(h_\theta(x^{(i)}))_k+(1-y_k^{(i)})log(1-h_\theta(x^{(i)}))_k]\\
&+\frac{\lambda }{2m}
\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_{l+1}}{(\Theta_{ji}^{(l)})} ^2

\end{split}
\end{equation}
$$
需要计算
$$
\begin{equation}
\begin{split}
J(\Theta)

\frac{\partial }{\partial (\Theta_{ji}^{(l)})}J(\Theta)
\end{split}
\end{equation}
$$



# 前向传播
假设有4层，每层神经元个数分别为3，5，5，4

前向传播计算公式如下：
$$
\begin{equation}
\begin{split}

&a^{(1)}=x \\

&z^{(2)}=\Theta^{(1)}a^{(1)} \\

&a^{(2)}=g(z^{(2)}) \quad (add  \quad a_0^{(2)}) \\

&z^{(3)}=\Theta^{(2)}a^{(2)}\\

&a^{(3)}=g(z^{(3)}) \quad (add  \quad a_0^{(3)})\\

&z^{(4)}=\Theta^{(3)}a^{(3)}\\

&a^{(4)}=h_\Theta(x)=g(z^{(4)})\\

\end{split}
\end{equation}
$$




# 反向传播计算误差

定义符号

$$
\delta_j^{(l)} \quad (error \; of \; layer \; l \;node \;j)
$$

计算公式如下：

$$
\delta_j^{(4)}  = a_j^{(4)} -y_j

\delta_j^{(3)}  = (\Theta^{(3)})^T \delta_j^{(4)}.*{g}'(z^{(3)})


\delta_j^{(2)}  = (\Theta^{(2)})^T \delta_j^{(3)}.*{g}'(z^{(2)})

$$




# 梯度检测
## 概述
实现神经网络的反向传播算法含有许多细节，在编程实现中很容易出现一些微妙的bug，但往往这些bug并不会影响你的程序运行，而且你的损失函数看样子也在不断变小。但最终，你的程序得出的结果误差将会比那些无bug的程序高出一个数量级

当我们对一个较为复杂的模型（例如神经网络）使用梯度下降算法时，可能会存在一些不容易察觉的错误（比如难以发现的bug），虽然在训练过程中，代价函数在变小，但最终的结果可能并不是最优解。

所以我们采用一种叫梯度检测的思想，它可以通过估计梯度（或导数）的近似值来估算我们的梯度下降算法算出的梯度（或导数）是否为正确的。

## 原理
梯度检测会估计梯度值，然后和你程序计算出来的梯度的值进行对比，以判断程序算出的梯度值是否正确。

脑补下，我们关注θ0点的函数的导数，即θ0点切线（蓝线）的斜率，现在我们在θ0−ε和θ0+ε两点连一条线（图中红线），我们发现红线的斜率和蓝线斜率很相似。

红线的斜率可以用以下式子表示

$$
\frac{J(\theta_0 + \epsilon ) -J(\theta_0 - \epsilon ) }{2 \epsilon }
$$

实际上，以上的式子很好地表示了θ0点导数的近似值。

在实际的应用中，θ往往是一个向量，梯度下降算法要求我们对向量中的每一个分量进行偏导数的计算，对于偏导数，我们同样可以用以下式子进行近似计算：
$$
\frac{J(\theta_1 + \epsilon, \theta_2, ... \theta_n) -J(\theta_1-\epsilon,\theta_2, ... \theta_n ) }{2 \epsilon }
$$
上式很好地估计了损失函数对θ1的偏导数


## 使用注意事项
梯度检测方法的开销是非常大的，比反向传播算法的开销都大，所以一旦用梯度检测方法确认了梯度下降算法算出的梯度（或导数）值是正确的，那么就及时关闭它。
一般来说ε的值选10−4，注意并不是越小越好。

# 随机初始化
在一些算法中，不能初始化参数为全零，例如神经网络。
在神经网络中，如果初始化所有的参数（也就是权重）相同，那么所有输入都相同，神经网络就失去了它的作用了。
所以我们需要随机初始化。

随机初始化每一个权重在下述范围之间：
$$
[-\epsilon, \epsilon]
$$

<br/>
<br/>

# 问题
1.初始化梯度全部为0有什么问题
> 答：会使模型相当于是一个线性模型，因为如果将权重初始化为零，那么损失函数对每个 w 的梯度都会是一样的，这样在接下来的迭代中，同一层内所有神经元的梯度相同，梯度更新也相同，所有的权重也都会具有相同的值，这样的神经网络和一个线性模型的效果差不多。（将 biases 设为零不会引起多大的麻烦，即使 bias 为 0，每个神经元的值也是不同的。）<br/><br/>

问题2: 神经网络训练的过程
> 1. 初始化 weights 和 biases
> 2. 前向传播，用 input X, weights W ，biases b, 计算每一层的 Z 和 A，最后一层用 sigmoid, softmax 或 linear function 等作用 A 得到预测值 Y
> 3. 计算损失，衡量预测值与实际值之间的差距
> 4. 反向传播，来计算损失函数对 W, b 的梯度 dW ，db，
> 5. 然后通过随机梯度下降等算法来进行梯度更新，重复第二到第四步直到损失函数收敛到最小。

其中第一步 权重的初始化 对模型的训练速度和准确性起着重要的作用，所以需要正确地进行初始化


问题3: 在训练深度神经网络时可能会造成的两个问题，梯度消失和梯度爆炸
1. 梯度消失：
> 是指在深度神经网络的反向传播过程中，随着越向回传播，权重的梯度变得越来越小，越靠前的层训练的越慢，导致结果收敛的很慢，损失函数的优化很慢，有的甚至会终止网络的训练。<br/>

解决方案：
> 1. Hessian Free Optimizer With Structural Dumping，
> 2. Leaky Integration Units，
> 3. Vanishing Gradient Regularization，
> 4. Long Short-Term Memory，长短期记忆
> 5. Gated Recurrent Unit，（GRU）是长短期的简化版
> 6. Orthogonal initialization 正交初始化


2. 梯度爆炸
> 和梯度消失相反，例如当你有很大的权重，和很小的激活函数值时，这样的权重沿着神经网络一层一层的乘起来，会使损失有很大的改变，梯度也变得很大，也就是 W 的变化（W - * dW）会是很大的一步，这可能导致在最小值周围一直振荡，一次一次地越过最佳值，模型可能一直也学不到最佳。爆炸梯度还有一个影响是可能发生数值溢出，导致计算不正确，出现 NaN，loss 也出现 NaN 的结果<br/>

解决方案:
> 1. Truncated Backpropagation Through Time (TBPTT)，
> 2. L1 and L2 Penalty On The Recurrent Weights，循环权重使用 L1 或 L2 惩罚项
> 3. Teacher Forcing，
> 4. Clipping Gradients， 梯度截断
> 5. Echo State Networks，回声状态网络

