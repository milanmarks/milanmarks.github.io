---
title: 'Durbin-levinson algorithm'
date: 2022-04-16
permalink: /posts/2022/durbin-levinson/
tags:
  - Time series
---

在看shumway学习ARMA模型的PACF时，下面两个问题总是困扰着我：

1. 为什么arma模型中，计算pacf的时候，将$X_{t+h}$向$\{X_{t+h-1},X_{t+h-2},\cdots,X_{t+1}\}$ 上面投影的时候，$X_{t+1}$的系数就是pacf？

2. 为什么arma模型进行forecasting的时候，用$\{X_1,X_2,\cdots, X_{n}\}$对$X_{n+1}$进行best linear prediction的时候，$X_1$前面的系数就是pacf？

看了Brockwell and Davis之后，我才终于明白。

在进行forecasting的时候，我们将$X_{n+1}$投影到希尔伯特空间$\mathcal{H}_n=span\\{X_1,\cdots,X_n\\}$（内积定义为协方差）。

$$
\mathcal{P}_{\mathcal{H}_n}
X_{n+1}=\sum_{j=1}^n\phi_{nj}X_{n+1-j}
$$

系数$\phi_{nj}$应当满足平均预测方差最小，即

$$
v_n=\mathbb{E}(X_{n+1}-\hat{X}_{n+1})^2
$$

最小化。注意到，这也意味着在希尔伯特空间上，$X_{n+1}$与$\hat{X}_{n+1}$之间的距离最小。

下面，我们考虑将$\mathcal{H_n}$分解为两个正交子空间$\mathcal{H_1}=span\\{X_2,\cdots,X_n\\}$和$\mathcal{H_2}=span\\{X_1-\mathcal{P}_{\mathcal{H}_1}X_1\\}$。则

$$
\begin{aligned}
\hat{X}_{n+1}&=\mathcal{P}_{\mathcal{H}_1}X_{n+1}+\mathcal{P}_{\mathcal{H}_2}X_{n+1}\\
&=\mathcal{P}_{\mathcal{H}_1}X_{n+1}+a(X_1-\mathcal{P}_{\mathcal{H}_1}X_1)
\end{aligned}
$$

其中

$$
a=\frac{\langle X_{n+1},X_1-\mathcal{P}_{\mathcal{H}_1}X_1\rangle}{\|X_1-\mathcal{P}_{\mathcal{H}_1}X_1 \|^2}
$$

根据稳定性，我们知道$(X_1,\cdots,X_{n})$，$(X_{n},\cdots,X_1)$与$(X_2,\cdots,X_{n+1})$有相同的协方差矩阵，因此$X_1$向$\mathcal{H_1}=span\\{X_2,\cdots,X_n \\}$的投影和$X_{n+1}$向$\mathcal{H_1}$的投影的时候，会存在系数相同的情况。并且$X_1-\mathcal{P_{\mathcal{H_1}}}X_1$与$X_{n+1}-\mathcal{P_{\mathcal{H_1}}}X_1$与$X_n-\mathcal{P_{\mathcal{H_{n-1}}}}X_n$的大小是相同的，即

$$
\mathcal{P}_{\mathcal{H}_1}X_1=\sum_{j=1}^{n-1}\phi_{n-1,j}X_{j+1}\\
\mathcal{P}_{\mathcal{H}_1}X_{n+1}=\sum_{j=1}^{n-1}\phi_{n-1,j}X_{n+1-j}=\sum_{j=1}^{n-1}\phi_{n-1,n-j}X_{j+1}\\
\|X_1-\mathcal{P}_{\mathcal{H}_1}X_1\|^2=\|X_{n+1}-\mathcal{P}_{\mathcal{H}_1}X_{n+1}\|^2\\
=\|X_n-\hat{X}_n\|^2=v_{n-1}
$$

那么，

$$
\begin{aligned}
\hat{X}_{n+1}
&=\mathcal{P}_{\mathcal{H}_1}X_{n+1}+a(X_1-\mathcal{P}_{\mathcal{H}_1}X_1)\\
&=\sum_{j=1}^{n-1}[\phi_{n-1,n-j}-a\phi_{n-1,j}]X_{j+1}+aX_1
\end{aligned}
$$

当$h\to \infty$，$\gamma(h)\to 0$，这条性质保证了

$$
\hat{X}_{n+1}=\sum_{j=1}^n\phi_{nj}X_{n+1-j}\tag{*}
$$

的表示是唯一的。因此

$$
\phi_{nn}=a\\
\phi_{n-1,n-j}-a\phi_{n-1,j}=\phi_{nj}
$$

考虑到$\mathcal{P_{\mathcal{H_1}}}X_{n+1}\perp X_1-\mathcal{P}_{\mathcal{H}_1}X_1 $，我们终于可以得到

$$
\begin{aligned}
\phi_{nn}=a
&=\frac{\langle X_{n+1},X_1-\mathcal{P}_{\mathcal{H}_1}X_1\rangle}{\|X_1-\mathcal{P}_{\mathcal{H}_1}X_1 \|^2}\\
&=\frac{\langle X_{n+1}-\mathcal{P}_{\mathcal{H}_1}X_{n+1},X_1-\mathcal{P}_{\mathcal{H}_1}X_1\rangle}{\|X_1-\mathcal{P}_{\mathcal{H}_1}X_1 \|^2}\\
&=\frac{\langle X_{n+1}-\mathcal{P}_{\mathcal{H}_1}X_{n+1},X_1-\mathcal{P}_{\mathcal{H}_1}X_1\rangle}{\|X_1-\mathcal{P}_{\mathcal{H}_1}X_1 \|\|X_{n+1}-\mathcal{P}_{\mathcal{H}_1}X_{n+1}\|}\\
&=Cor(X_{n+1}-\hat{X}_{n+1},X_1-\hat{X}_1)
\end{aligned}
$$

太好啦！现在我们回答了第二个问题！第一个问题我们瞬间就能得到！因为当$h>p$时，

$$
\hat{X}_{t+h}=\sum_{j=1}^p \phi_jX_{t+h-j}
$$

则令$n=p$可知，

$$
\hat{X}_{n+1}=\sum_{j=1}^n\phi_j X_{n+1-j}
$$

与$(*)$比较，可知$\phi_j=\phi_{nj}$。

更进一步，我们可以得到:

$$
\begin{aligned}
\phi_{nn}=a
&=\frac{\langle X_{n+1},X_1-\sum_{j=1}^{n-1}\phi_{n-1,j}X_{j+1}\rangle}{v_{n-1}}\\
&=\frac{\langle X_{n+1},X_1\rangle-\sum_{j=1}^{n-1}\phi_{n-1,j}\langle X_{n+1},X_{j+1}\rangle}{v_{n-1}}\\
&=\frac{\gamma(n)-\sum_{j=1}^{n-1}\phi_{n-1,j}\gamma(n-j)}{v_{n-1}}\\
\end{aligned}
$$

$$
\begin{aligned}
v_n
&=\|X_{n+1}-\hat{X}_{n+1}\|^2\\
&=\|X_{n+1}-\mathcal{P}_{\mathcal{H}_1}X_{n+1}-a(X_1-\mathcal{P}_{\mathcal{H}_1}X_1)\|^2\\
&=\|X_{n+1}-\mathcal{P}_{\mathcal{H}_1}X_{n+1}\|^2+a^2\|X_1-\mathcal{P}_{\mathcal{H}_1}X_1\|^2-2a\langle X_{n+1},X_1-\mathcal{P}_{\mathcal{H}_1}X_1\rangle\\
&=v_{n-1}+a^2v_{n-1}-2a^2v_{n-1}\\
&=v_{n-1}(1-a^2)
\end{aligned}
$$

这就是

**Durbin-Levison algorithm** If $\\{X_{t}\\}$ is a zero mean stationary process with autocovariance function $\gamma(\cdot)$ such that $\gamma(0)>0$ and $\gamma(h) \rightarrow 0$ as $h \rightarrow \infty$, then mean squared errors 

$$
v_{n}=\mathbb{E}(X_{n+1}-\hat{X}_{n+1})^2
$$

and the coefficients $\phi_{n j}$
of satisfy 

$$
\hat{X}_{n+1}=\sum_{j=1}^n\phi_{nj}X_{n+1-j}
$$

and  $\phi_{11}=\gamma(1) / \gamma(0), v_{0}=\gamma(0)$,

$$
\phi_{n n}=\left[\gamma(n)-\sum_{j=1}^{n-1} \phi_{n-1, j} \gamma(n-j)\right] v_{n-1}^{-1},
$$

$$
\left[\begin{array}{c}
\phi_{n 1} \\
\vdots \\
\phi_{n, n-1}
\end{array}\right]=\left[\begin{array}{c}
\phi_{n-1,1} \\
\vdots \\
\phi_{n-1, n-1}
\end{array}\right]-\phi_{n n}\left[\begin{array}{c}
\phi_{n-1, n-1} \\
\vdots \\
\phi_{n-1,1}
\end{array}\right]
$$

and

$$
v_{n}=v_{n-1}\left[1-\phi_{n n}^{2}\right]
$$
