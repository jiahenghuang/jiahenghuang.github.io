---
title: pair Hmm notes
date: 2018-4-29 16:48:52
tags: 
- hmm
- 生物信息
categories: [生物信息,算法]
---

#pair Hmm学习笔记

计划：

1. EM算法复习
2. HMM学习
   - 概率计算：Evaluate $p(Y|\lambda)$
   - 参数学习：$\lambda_{MLE}=arg\max\limits_\lambda(p(Y|\lambda))$
   - 预测|解码：$arg\max\limits_Q(p(Y|Q,\lambda))$
3. HMM python实现
4. pair HMM学习

[TOC]

## 1. EM算法复习

参见：<http://www-staff.it.uts.edu.au/~ydxu/ml_course/em.pdf>

​	当求随机变量$x_1,x_2,...,x_n\;{\scriptsize{\sim} }\;p(x|\theta)$的参数$\theta$的极大似然估计，若似然函数$L(\theta|X=(x_1,...,x_n))$形式十分复杂，这时候可以引进潜在变量$Z$，然后应用EM算法求解$\theta$的局部最优解
$$
\theta^{g+1}=arg\max\limits_{\theta}(\int\limits_Zlog(p(X,Z|\theta))p(Z|X,\theta^g)dZ)\;(1)
$$
(a)证明序列$\theta=\{\theta^1,...,\theta^g,...\}$的收敛性

<=>证明$\;log[p(X|\theta^{g+1})]=L(\theta^{g+1}){\geq}L(\theta^g),\forall{i}$

(b)证明(1)式等价于
$$
\theta^{g+1}=arg\max\limits_{\theta}(\int\limits_Zlog(p(X,Z|\theta))p(Z,X|\theta^g)dZ)\;(1)
$$
(1)证明：
$$
log[p(X|\theta^{g+1})]=log(\frac{p(X,Z|\theta)}{p(Z|X,\theta)})\\\
=log(\frac{\frac{p(X,Z|\theta)}{Q(Z)}}{\frac{p(Z|X,\theta)}{Q(Z)}})\\\
=log(\frac{p(X,Z|\theta)}{Q(Z)})+log(\frac{Q(Z)}{p(Z|X,\theta)})\\\
<=>log[p(X|\theta^{g+1})]=\int\limits_Zlog[p(X|\theta)]Q(Z)dZ\\\
=\int\limits_Zlog(\frac{p(X,Z|\theta)}{Q(Z)})Q(Z)dZ+\int\limits_Zlog(\frac{Q(Z)}{p(Z|X,\theta)})Q(Z)dZ\\\
=\int\limits_Zlog(\frac{p(X,Z|\theta)}{Q(Z)})Q(Z)dZ+\underset{\geq0}{KL(Q(Z)||p(Z|X,\theta))}\\\
\geq{F(\theta,Q)}等式成立条件：Q(Z)=p(Z|X,\theta)
$$

## 2. HMM学习

以下基于离散转移概率的假设。研究HMM，需要搞清楚三个基本问题：

- 概率计算：Evaluate $p(Y|\lambda)$
- 参数学习：$\lambda_{MLE}=arg\max\limits_\lambda(p(Y|\lambda))$
- 预测|解码：$arg\max\limits_Q(p(Y|Q,\lambda))$

符号表示及术语说明：

> transition probability: $p(q_t=j|q_{t-1}=i)=a_{i,j}$，转移矩阵$A=\{a_{i,j}\}$
>
> emission probability(又叫measurement probability): $p(y_t|q_t=j)=b_j(y_t)$，$B=\{b_j(y_t)\}$
>
> start probability:$\pi=(p(q_1=1,p(q_1=2),...,p(q_1=K)),\pi_i=p(q_1=i)$
>
> 参数:$\lambda={A,B,\pi}$
>
> Forward algorithm参数: $\alpha_i(t)=p(y_1,...,y_t,q_t=i|\lambda)$
>
> Backward algorithm参数: $\beta_j(t)=p(y_{t+1},...,y_T|q_t=j,\lambda)$
>
> HMM假设：
>
> 1. $p(q_t|q_1,...,q_{t-1},y_1,...,y_{t-1})=p(q_t|q_{t-1})$
> 2. $p(y_t|q_1,...,q_t,y_1,...,y_{t-1})=p(y_t|q_t)$

### 2.1 概率计算

$$
p(Y|\lambda)=p(y_1,...,y_T|\lambda)\\\
=\sum_{q_1=1}^{K}\sum_{q_2=1}^{K}...\sum_{q_T=1}^{K}p(y_1,...,y_T,q_1,...,q_T|\lambda)\\\
=\sum_{q_1=1}^{K}\sum_{q_2=1}^{K}...\sum_{q_T=1}^{K}\pi(q_1)\prod_{t=2}^Ta_{q_{t-1},q_t}b_{q_t}(y_t)
$$

### 2.2 参数学习

#### 2.2.1 forward algorithm

> $\alpha=p(y_1,y_2,...,y_t,q_t=i|\lambda)$
>
> $\alpha_i(1)=p(y_1,q_1=i)=p(y_1|q_1=i)=p(y_1|q_1=i)p(q_1=i)=b_i(y_1)\pi_i$
>
> $\alpha_j(2)=p(y_1,y_2,q_2=j|\lambda)=\sum_{i=1}^{K}p(y_1,y_2,q_1=i,q_2=j|\lambda)$
>
> $=\sum_{q_1}p(y_2,q_2|q_1,\lambda)p(y_1,q_1|\lambda)=\sum_{q_1}p(y_2|q_2=j,\lambda)p(q_2=j|q_1=i,\lambda)\alpha_i(1)$
>
> $=b_j(y_2)\sum_{q_1}a_{i,j}\alpha_i(1)$
>
> ...
>
> $\alpha_j(t)=b_j(y_t)\sum_{i=1}^{K}a_{i,j}\alpha_i(t-1)$
>
> ...
>
> $\alpha_j(T)=b_j(y_T)\sum_{i=1}^Ka_{i,j}\alpha_i(T-1)=p(y_1,y_2,...,y_T,q_T=j)$
>
> $p(y_1,y_2,...,y_T)=\sum_{i=1}^{K}\alpha_j(T)$

#### 2.2.2 backward algorithm

> $\beta_j(t)=p(y_{t+1},...,y_T|q_t=j,\lambda)$
>
> $\beta_i(T-1)=p(y_T|q_{T-1}=i)=\sum_{l=1}^Kp(y_T,q_T=l|q_{T-1}=i)$
>
> $=\sum_{l=1}^Kp(y_T|q_T=l)p(q_T=l|q_{T-1}=i)=\sum_{i=1}^Kb_l(y_T)a_{i,l}$
>
> $\beta_j(T-2)=p(y_{T-1},y_T|q_{T-2}=j)=\sum_{i=1}^Kp(y_{T-1},y_T,q_{T-1}=i|q_{T-2}=j)$
> $$
> =\sum{i=1}^Kp(y_T|q{T-1}=i)p(y{T-1},q{T-1}=i|q{T-2}=j)\\\
> =\sum{i=1}^Kp(y_T|q{T-1}=i)p(y{T-1}|q{T-1}=i)p(q{T-1}=i|q_{T-2}=j)
> $$
> $=\sum_{i=1}^K\beta_i(T-1)b_i(y_{T-1})a_{j,i}$
>
> ...
>
> $\beta_j(t)=\sum_{i=1}^K\beta_i(t+1)b_i(y_{t+1})a_{j,i}$
>
> ...
>
> $\beta_j(1)=\sum_{i=1}^K\beta_i(2)b_i(y_2)a_{j,i}=p(y_1,y_2,...,y_T|q_1=j)$
>
> $p(y_1,y_2,...,y_T)=\sum_{j=1}^Kp(y_1,y_2,...,y_T,q_1=j)=\sum_{j=1}^K\beta(1)\pi_j$

#### 2.2.3 Baum-welch算法

​	Baum-welch算法其实就是EM算法在HMM算法中的直接应用。

$$
{\lambda ^{g + 1}} = \mathop {\arg \max }\limits_\lambda  (\int\limits_q {\ln (p(Y,q|\lambda ))p(Y,q|{\lambda ^g})} dq)
$$


$$
Q(\lambda ,{\lambda ^g}) = \int\limits_q {\ln (p(Y,q|\lambda ))p(Y,q|{\lambda ^g})} dq\\\
= \sum_{ {q_1} = 1}^K ... \sum_{q_T=1}^K  \lbrace{\ln(\pi_{q1}) + \sum_{t = 2}^T {\ln(a_{q_{t-1}, q_t})} + \sum_{t = 1}^T{\ln(b_{q_t}(y_t))} \rbrace p(q,Y | \lambda^g) }\\\
= {Q_{term1} } + {Q_{term2} } + {Q_{term3} }
$$

$$
LM_{term1} = \sum_{i = 1}^K {\ln (\pi _i)p(q_1 = i,Y|{\lambda ^g})} + \tau(\sum_{i=1}^K\pi_i-1)\\\
=> \pi = \frac{\alpha_i(1)\beta_i(1)}{\sum_{j=1}^K \alpha_j(1)\beta_j(1)}
$$


$$
LM_{term2} = \sum_{i=1}^K \sum_{j=1}^K \sum_{t=1}^T \ln(a_{i,j})p(q_{t-1} = i, q_t = j, Y | \lambda^g) + \sum_{i = 1}^K \tau_i(\sum_{j = 1}^K a_{i,j} - 1)\\\
=> a_{i,j} = \frac{\sum_{t = 1}^T p(q_{t - 1} = i, q_t = j, Y | \lambda^g)}{\sum_{t = 1}^T p(q_{t - 1} = i, Y | \lambda^g)}
$$

$$
LM_{term3} = \sum_{j = 1}^K \sum_{t = 1}^T \ln (b_j(y_t))p(q_t = j, Y | {\lambda ^g})  +  \sum_{t = 1}^T \tau_t(\sum_{j = 1}^K {b_j}({y_t})  - 1)\\\
=> b_j(y_t) = \frac{\sum_{t = 1}^T p({q_t} = j,Y|{\lambda ^g})} {\sum_{t = 1}^T p(Y|{\lambda ^g})} 
$$

### 2.3 解码：Viterbi算法

​	若定义在时刻t状态为i使得概率最大的路径$({q_1},...,{q_t})$
$$
\delta_t(i) = \mathop{\max}\limits_{q_1,...,q_{t - 1} } p(q_t = i, q_{t - 1},...,q_1, y_1,...,y_t|\lambda ), i = 1,...,K
$$
则使得t+1时刻概率最大的路径为
$$
\delta_{t+1}(j) = \mathop{ \max } \limits_{q_1,...,q_t } p(q_{t+1} = i, q_t, ..., q_1, y_1,...,y_{t+1}|\lambda ),i = 1,...,K
$$
记$\psi_t(i) = \mathop {\arg \max} \limits_i [\delta_t(i)a_{i,j}],i = 1,2,...,K$

| 算法：                                                       |      |      |
| :----------------------------------------------------------- | ---- | ---- |
| 输入：模型$\lambda  = (A,B,\pi )$和观测$Y = ({y_1},...,{y_T})$; |      |      |
| 输出：最优路径$q^{\star} = ({q_1}^{\star},{q_2}^{\star},...,{q_T}^{\star})$. |      |      |
| (1)初始化                                                    |      |      |
| ${\delta_1}(i) = \pi_i b_i(y_1),i = 1,2,...,K$               |      |      |
| ${\psi _1}(i) = 0,i = 1,2,...,K$                             |      |      |
| (2)递推，对t=2,3,...,T.                                      |      |      |
| $\delta_t(j) = \mathop {\max }\limits_{1 \le i \le K} [{\delta_{t - 1}}(i)a_{i,j}]{b_j}({y_t}),j = 1,...,K$ |      |      |
| ${\psi_t}(i) = \mathop {\arg \max }\limits_i [{\delta_{t - 1}}(i){a_{i,j}}],i = 1,2,...,K$ |      |      |
| (3)终止                                                      |      |      |
| ${p^*} = \mathop {\max }\limits_{1 \le i \le K} {\delta _T}(i)$ |      |      |
| ${i_T}^* = \mathop {\arg \max }\limits_i [{\delta _T}(i)]$   |      |      |
| (4)最优路径回溯，对t=T-1,T-2,...,1;${i_T}^{\star} = {\psi_{t + 1}}({i_{t + 1}}^{\star})$ |      |      |
| 求得最优路径$q^{\star} = ({q_1}^{\star},{q_2}^{\star},...,{q_T}^{\star})$. |      |      |