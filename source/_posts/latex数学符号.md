---
title: latex数学符号
date: 2018-05-09 09:13:12
tags: [latex, 数学符号]
categories: latex
---

## 1.公式左对齐

```latex
\begin{align}
& \widehat{q_i} = qual \\\
& q = globalDeltaQEmpirical \\\
& \bar{\widehat{q}} = aggregrateQReported \\\
& q_i = deltaQReportedEmpirical \\\
&q_{i0} = deltaQCovariateEmpirical_0, covariatedName = context \\\
& q_{i1} = deltaQCovariateEmpirical_1, covariatedName = cycle
\end{align}
```

显示效果如下:
$$
\begin{align}
& \widehat{q_i} = qual \\\
& q = globalDeltaQEmpirical \\\
& \bar{\widehat{q}} = aggregrateQReported \\\
& q_i = deltaQReportedEmpirical \\\
&q_{i0} = deltaQCovariateEmpirical_0, covariatedName = context \\\
& q_{i1} = deltaQCovariateEmpirical_1, covariatedName = cycle
\end{align}
$$


   参考链接:

      1. [常用数学符号的 LaTeX 表示方法](http://wdxtub.com/2016/03/26/latex-notation-table/)
      2. [Latex 常用符号表](http://wulc.me/2016/09/18/%E5%B8%B8%E7%94%A8%E6%95%B0%E5%AD%A6%E7%AC%A6%E5%8F%B7%E7%9A%84%20LaTeX%20%E8%A1%A8%E7%A4%BA%E6%96%B9%E6%B3%95/)
      3. [Texlive: latex数学符号表(2) ](http://blog.sina.com.cn/s/blog_4df4d74401014qxb.html)

## 2. 公式编号

- 手动编号

```latex
$$x^n+y^n=z^n \tag{1.1}$$
```

**显示效果**

$$x^n+y^n=z^n \tag{1.1}$$

- 或者使用Typora偏好设置中的自动编号（不方便维护，不推荐）

参考：https://www.zybuluo.com/fyywy520/note/82980

## 3. 矩阵

- 不带括号的矩阵

  **语法**

  ```latex
  \begin{matrix}
     1 & 2 & 3 \\\
     4 & 5 & 6 \\\
     7 & 8 & 9
  \end{matrix} \tag{1}
  ```

  **显示效果**
  $$
  \begin{matrix}
     1 & 2 & 3 \\\
     4 & 5 & 6 \\\
     7 & 8 & 9
    \end{matrix} \tag{1}
  $$

- 带括号的矩阵

  **语法**

  ```latex
  \left{
  \begin{matrix}
     1 & 2 & 3 \\\
     4 & 5 & 6 \\\
     7 & 8 & 9
  \end{matrix}
  \right} \tag{2}
  ```

  **显示效果**
  $$
  \left\{
  \begin{matrix}
     1 & 2 & 3 \\\
     4 & 5 & 6 \\\
     7 & 8 & 9
  \end{matrix}
  \right\} \tag{2}
  $$

- 带方括号的矩阵

  **语法**

  ```latex
  \begin{bmatrix}
     1 & 2 & 3 \\\
     4 & 5 & 6 \\\
     7 & 8 & 9
  \end{bmatrix} \tag{3}
  ```

  **显示效果**
  $$
  \begin{bmatrix}
     1 & 2 & 3 \\\
     4 & 5 & 6 \\\
     7 & 8 & 9
  \end{bmatrix} \tag{3}
  $$

- 带省略符号的矩阵

  **语法**

  ```latex
  \left[
  \begin{matrix}
   1      & 2      & \cdots & 4      \\\
   7      & 6      & \cdots & 5      \\\
   \vdots & \vdots & \ddots & \vdots \\\
   8      & 9      & \cdots & 0      \\\
  \end{matrix}
  \right]
  ```

  **显示效果**
  $$
  \left[
  \begin{matrix}
   1      & 2      & \cdots & 4      \\\
   7      & 6      & \cdots & 5      \\\
   \vdots & \vdots & \ddots & \vdots \\\
   8      & 9      & \cdots & 0      \\\
  \end{matrix}
  \right]
  $$
  

参考：[使用LaTeX写矩阵 ](https://blog.csdn.net/bendanban/article/details/44221279)

## 4. 符号

- 叉乘

  **语法**

  ```latex
  a \times b
  ```

  **显示效果**

  $$a \times b$$

- 除以

  **语法**

  ```latex
  a \div b
  ```

  **显示效果**

  $$a \div b$$

  







