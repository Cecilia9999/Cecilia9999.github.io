---
title: 循环神经网络（RNN）
date: 2020-07-05 16:19:11
categories:
  - machine learning
tags:
  - RNN
mathjax: true
---

DNN / CNN 无法处理训练样本输入是连续序列且长短不一，比如基于时间的序列（数据之间有时间相关性）：一段段连续的语音，一段段连续的手写文字。这些序列比较长，且长度不一，比较难直接的拆分成一个个独立的样本来通过 DNN / CNN 进行训练。循环神经网络（Recurrent Neural Networks，RNN）则解决了这样一个问题，广泛应用于语音识别，手写识别及机器翻译等领域。
<!--more-->

# RNN 基本模型

![](markdown-img-paste-20200705162222486.png)
上图是在序列索引号 𝑡 （ 𝑡 时刻）附近 RNN 的模型。其中：

1）𝑥(𝑡) 代表在序列索引号 𝑡 时训练样本的输入。同样的，𝑥(𝑡−1) 和 𝑥(𝑡+1) 代表在序列索引号 𝑡−1 和 𝑡+1 时训练样本的输入。

2）ℎ(𝑡) 代表在序列索引号 𝑡 时模型的隐藏状态，由 𝑥(𝑡) 和 ℎ(𝑡−1) 共同决定。

3）𝑜(𝑡) 代表在序列索引号 𝑡 时模型的输出，只由当前隐藏状态 ℎ(𝑡) 决定。

4）𝐿(𝑡) 代表在序列索引号 𝑡 时模型的损失函数。

5）𝑦(𝑡) 代表在序列索引号𝑡 时训练样本序列的真实输出。

6）𝑈, 𝑊, 𝑉 这三个矩阵是模型的线性关系参数，它在整个 RNN 网络中是共享的，这点和 DNN 很不相同。 也正因为是共享了，它体现了 RNN 的模型的 “循环反馈” 的思想。　　

# RNN 前向传播
对于任意一个序列索引号 𝑡，我们隐藏状态 ℎ(𝑡) 由 𝑥(𝑡) 和 ℎ(𝑡−1) 得到

$$ h^{(t)} = \sigma(z^{(t)}) = \sigma(Ux^{(t)} + Wh^{(t-1)} +b ) $$

其中 𝜎 为 RNN 的激活函数，一般为 𝑡𝑎𝑛ℎ , 𝑏 为偏置。
序列索引号 𝑡 时模型的输出 𝑜(𝑡) 为

$$o^{(t)} = Vh^{(t)} +c$$

在最终在序列索引号 𝑡 时我们的预测输出为:

$$\hat{y}^{(t)} = \sigma(o^{(t)})$$

通常由于 RNN 是识别类的分类模型，所以上面这个激活函数一般是 softmax。另外，通过损失函数 𝐿(𝑡)，比如对数似然损失函数，可以比较 𝑦̂ (𝑡) 和 𝑦(𝑡) 的差距。

# RNN 反向传播
RNN 反向传播算法的思路和 DNN 是一样的，即通过梯度下降法进行迭代，得到合适的 RNN 模型参数 𝑈, 𝑊, 𝑉, 𝑏, 𝑐。RNN 反向传播有时也叫做 **BPTT (back-propagation through time)**，由于基于时间反向传播。注意：BPTT 和 DNN 最大的不同点是：RNN 中**所有的 𝑈, 𝑊, 𝑉, 𝑏, 𝑐 参数在序列中共享**，反向传播时更新的是相同的参数。

这里选择交叉熵损失函数为例，输出的激活函数为 softmax 函数，隐藏层的激活函数为 tanh 函数。对于 RNN，由于我们在序列的每个位置都有损失函数，因此最终的损失 𝐿 为：

$$L = \sum\limits_{t=1}^{\tau}L^{(t)}$$

其中 𝑉, 𝑐 的梯度计算是比较简单的：

$$\frac{\partial L}{\partial c} = \sum\limits_{t=1}^{\tau}\frac{\partial L^{(t)}}{\partial c}  = \sum\limits_{t=1}^{\tau}\hat{y}^{(t)} - y^{(t)}$$

$$\frac{\partial L}{\partial V} =\sum\limits_{t=1}^{\tau}\frac{\partial L^{(t)}}{\partial V}  = \sum\limits_{t=1}^{\tau}(\hat{y}^{(t)} - y^{(t)}) (h^{(t)})^T$$

𝑊, 𝑈, 𝑏 的梯度在反向传播时，在某一序列位置 𝑡 的梯度损失由当前位置的输出对应的梯度损失和序列索引位置 𝑡+1 时的梯度损失（ 𝑡+1 梯度损失包含了 𝑡+1 及以后）两部分共同决定。

对于 𝑊 在某一序列位置 𝑡 的梯度损失需要反向传播一步步的计算。我们定义序列索引 𝑡 位置的隐藏状态的梯度为

$$\delta^{(t)} = \frac{\partial L}{\partial h^{(t)}}$$

从 𝛿(𝑡+1) 递推 𝛿(𝑡)

$$\delta^{(t)} =(\frac{\partial o^{(t)}}{\partial h^{(t)}} )^T\frac{\partial L}{\partial o^{(t)}} + (\frac{\partial h^{(t+1)}}{\partial h^{(t)}})^T\frac{\partial L}{\partial h^{(t+1)}} = V^T(\hat{y}^{(t)} - y^{(t)}) + W^Tdiag(1-(h^{(t+1)})^2)\delta^{(t+1)}$$

对于 𝛿(𝜏)，由于它的后面没有其他的序列索引了，因此

\delta^{(\tau)} =( \frac{\partial o^{(\tau)}}{\partial h^{(\tau)}})^T\frac{\partial L}{\partial o^{(\tau)}} = V^T(\hat{y}^{(\tau)} - y^{(\tau)})

有了 𝛿(𝑡)，就可以计算 𝑊, 𝑈, 𝑏 的梯度了

$$\frac{\partial L}{\partial W} = \sum\limits_{t=1}^{\tau}diag(1-(h^{(t)})^2)\delta^{(t)}(h^{(t-1)})^T$$

$$\frac{\partial L}{\partial b}= \sum\limits_{t=1}^{\tau}diag(1-(h^{(t)})^2)\delta^{(t)}$$

$$\frac{\partial L}{\partial U} =\sum\limits_{t=1}^{\tau}diag(1-(h^{(t)})^2)\delta^{(t)}(x^{(t)})^T$$

## 总结
RNN 虽然理论上可以很漂亮的解决序列数据的训练，但它也有梯度消失时的问题。当序列很长的时候问题尤其严重。因此，上面的 RNN 模型一般不能直接用于应用领域。在语音识别，手写书别以及机器翻译等NLP 领域实际应用比较广泛的是基于 RNN 模型的一个特例 LSTM。

#### Reference
https://www.cnblogs.com/pinard/p/6509630.html#!comments
