---
date: 2020-02-01 16:15:04
categories:
   - 机器学习
tags:
   - 吴恩达视频笔记
mathjax: true
---
# 过拟合问题
过拟合（over-fitting），就是所建的机器学习模型或者是深度学习模型在训练样本中表现得十分优越，但是在验证数据集以及测试数据集中表现不佳。
在性能的角度上讲就是协方差过大(variance is large)，同样在测试集上的损失函数(cost function)会表现得很大
<!--more-->

造成过拟合的原因有可以归结为：参数过多

解决方法：
- 减少特征：
<br/> 1）人工判断需要减少的特征
<br/> 2）模型判断可以减少的特征
- 正则化：
<br/> 1）保留所有的特征，但是减小参数的大小 
<br/> 2）当我们有许多用得上的特征时，正则化很有效

# Logistic回归的正则化
 Logistic的损失函数是：
$$
J(θ)=-\frac{1}{m}[\sum_{i=1}^{m} y^{(i)}log(h_θ(x^{(i)}))+(1-y^{(i)})log(1-h_θ(x^{(i)}))]
$$

在末尾增加一个项进行正则化
$$
J(θ)=-\frac{1}{m}[\sum_{i=1}^{m} y^{(i)}log(h_θ(x^{(i)}))+(1-y^{(i)})log(1-h_θ(x^{(i)}))]+
\frac{\lambda }{2m}\sum_{j=1}^{n}{\theta_j} ^2
$$

利用梯度下降不断更新
$$
θ_0:
=θ_0-\alpha \frac{1}{m} \sum_{i=1}^{m}(h_θ(x^{(i)})-y^{(i)})x_0^{(i)}
$$

$$
θ_j:
=θ_j-\alpha [\frac{1}{m} \sum_{i=1}^{m} (h_θ(x^{(i)})-y^{(i)})x_j^{(i)}+ \frac{\lambda }{m} \theta_j]
$$


# 线性回归的正则化
利用梯度下降不断更新
$$
θ_0:
=θ_0-\alpha \frac{1}{m} \sum_{i=1}^{m}(h_θ(x^{(i)})-y^{(i)})x_0^{(i)}
$$
$$
θ_j:
=θ_j-\alpha [\frac{1}{m} \sum_{i=1}^{m} (h_θ(x^{(i)})-y^{(i)})x_j^{(i)}+ \frac{\lambda }{m} \theta_j]   \,\,\,  and \,   j\subset  (1,2,3...n)
$$

对公式整理后得到：

$$
θ_j:
=θ_j(1-\alpha \frac{\lambda }{m})-\alpha \frac{1 }{m}\sum_{i=1}^{m} (h_θ(x^{(i)})-y^{(i)})x_j^{(i)}
$$
第一个系数肯定小于1，可以看出来是将变量在原来的基础上减小了一点点的。


# 正规方程：Normal Equation
对正规方程添加正则化。

 $$
θ=(X^TX+ \lambda L)^{-1}X^Ty
$$
其中 L为n+1 * n+1 的矩阵，包括n*n的单位矩阵
$$
\begin{bmatrix}
0 &  &  &  & \\ 
 & 1 &  &  & \\ 
 &  &1  &  & \\ 
 &  &  &...  & \\ 
 &  &  &  &1 
\end{bmatrix}

$$

如果m 小于n，则 X^TX 是不可逆的，但是
$$
X^TX+ \lambda L
$$
是可逆的