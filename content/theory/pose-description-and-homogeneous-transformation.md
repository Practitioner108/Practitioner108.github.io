---
title: "《机器人学》学习笔记 | 第3章：位姿描述和齐次变换"
date: 2025-12-13T11:00:31+08:00
draft: false
math: true
---

# 第三章  位姿描述和齐次变换
运动学涉及很多数学论证，本笔记为实战笔记，只记录与应用相关的结论，至于推理什么的，人生苦短，证明就免了吧。
## 3.1 刚体位姿描述
首先是要表示物体在空间中的位置与姿态，对于物体本身的数学表达，我们一般是在其质心或者几何中心等诸如此类的地方设置原点，将其等效成一个质点，以此来描述它的位置和姿态。

首先是位置：
$${}^A\boldsymbol{p} = \begin{bmatrix} p_x \\\\ p_y \\\\ p_z \end{bmatrix}$$
式中：$p_x, p_y, p_z$ 是点 $p$ 在坐标系 $\{A\}$ 中的三个坐标分量。${}^Ap$ 的上标 $A$ 代表选定的参考坐标系 $\{A\}$。总之，空间任一点 $p$ 的位置在直角坐标系中表示为三维矢量，即 $p \in \Re^3$。除了直角坐标系外，还可采用圆柱坐标系或球（极）坐标系来描述点的位置。

其次是姿态：
为了规定空间某刚体 $B$ 的方位，另设一直角坐标系 $\{B\}$ 与此刚体固连。用坐标系 $\{B\}$ 的三个单位主矢量 $\boldsymbol{x}_B, \boldsymbol{y}_B, \boldsymbol{z}_B$ 相对于坐标系 $\{A\}$ 的方向余弦组成的 $3 \times 3$ 的余弦矩阵来表示刚体 $B$ 相对于坐标系 $\{A\}$ 的方位：

$${}_B^A\boldsymbol{R} = \begin{bmatrix} {}^A\boldsymbol{x}_B & {}^A\boldsymbol{y}_B & {}^A\boldsymbol{z}_B \end{bmatrix}$$

或

{{< math >}}
$$
{}_B^A\boldsymbol{R} = \begin{bmatrix} 
r_{11} & r_{12} & r_{13} \\ 
r_{21} & r_{22} & r_{23} \\ 
r_{31} & r_{32} & r_{33} 
\end{bmatrix}
$$
{{< /math >}}


式中：${}_B^A\boldsymbol{R}$ 称为旋转矩阵。上标 $A$ 代表参考坐标系 $\{A\}$，下标 $B$ 代表被描述的坐标系 $\{B\}$。${}_B^A\boldsymbol{R}$ 有 9 个元素，其中只有 3 个是独立的。因为 ${}_B^A\boldsymbol{R}$ 的三个列矢量 ${}^A\boldsymbol{x}_B, {}^A\boldsymbol{y}_B, {}^A\boldsymbol{z}_B$ 都是单位主矢量，且两两相互垂直，属于右手系，所以 ${}_B^A\boldsymbol{R}$ 的 9 个元素满足 6 个约束条件（称为正交条件）：

$${}^A\boldsymbol{x}_B \cdot {}^A\boldsymbol{x}_B = {}^A\boldsymbol{y}_B \cdot {}^A\boldsymbol{y}_B = {}^A\boldsymbol{z}_B \cdot {}^A\boldsymbol{z}_B = 1$$

$${}^A\boldsymbol{x}_B \cdot {}^A\boldsymbol{y}_B = {}^A\boldsymbol{y}_B \cdot {}^A\boldsymbol{z}_B = {}^A\boldsymbol{z}_B \cdot {}^A\boldsymbol{x}_B = 0$$

因此，旋转矩阵 ${}_B^A\boldsymbol{R}$ 是正交的，并且满足条件：

$$\begin{cases} {}_B^A\boldsymbol{R}^{-1} = {}_B^A\boldsymbol{R}^T \\\\ \det({}_B^A\boldsymbol{R}) = 1 \end{cases}$$

式中：上标 $T$ 表示转置；$\det(\cdot)$ 是矩阵的行列式符号。

绕 $x$ 轴、$y$ 轴、$z$ 轴旋转 $\theta$ 角的旋转矩阵分别为：

$$\boldsymbol{R}(x, \theta) = \begin{bmatrix} 1 & 0 & 0 \\\\ 0 & \cos\theta & -\sin\theta \\\\ 0 & \sin\theta & \cos\theta \end{bmatrix}$$

$$\boldsymbol{R}(y, \theta) = \begin{bmatrix} \cos\theta & 0 & \sin\theta \\\\ 0 & 1 & 0 \\\\ -\sin\theta & 0 & \cos\theta \end{bmatrix}$$

$$\boldsymbol{R}(z, \theta) = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\\\ \sin\theta & \cos\theta & 0 \\\\ 0 & 0 & 1 \end{bmatrix}$$

看着眼花缭乱，但实际上就是：
1. 用一个向量来表征刚体本身（也就是坐标系的原点）在$\{A\}$坐标系的位置；
2. 用一个矩阵（另一个坐标系）来表示相对于$\{A\}$坐标系的位置，因为矩阵里的三个列向量实际上就是三个单位向量，是$\{B\}$坐标系的三个基向量，所以用他们来表示方向。

## 3.2 齐次坐标和齐次变换
虽然我们找到了表示位置和姿态的形式，但是一个向量、一个矩阵，太麻烦了，算着也麻烦，统筹更麻烦。按照数学第一准则“能躺着绝不站着”，必然是要寻求一种更简单的方法把他们一起统筹和计算，所以就有了齐次的形式。

{{< math >}}
$$
{}_B^A\boldsymbol{T} = \begin{bmatrix}
{}_B^A\boldsymbol{R} & {}^A\boldsymbol{p}_{Bo} \\
\mathbf{0} & 1
\end{bmatrix} = \begin{bmatrix}
r_{11} & r_{12} & r_{13} & p_x \\
r_{21} & r_{22} & r_{23} & p_y \\
r_{31} & r_{32} & r_{33} & p_z \\
0      & 0      & 0      & 1
\end{bmatrix}
$$
{{< /math >}}

对于这个T矩阵，其实就是前面的R矩阵和p向量的结合体，下面加一行$[0, 0, 0, 1]$，这样做之后，不仅可以把整个矩阵变成一个方阵方便运算，而且能通过每个列向量底下是0还是1来确定其作用，具体来说：

**首先** $[0 \quad 0 \quad 0 \quad 0]^T$ 没有意义。

**我们规定：** 列矢量 $[a \quad b \quad c \quad 0]^T$ （其中 $a^2 + b^2 + c^2 \neq 0$）表示空间的**无穷远点**。把包括无穷远点的空间称为**扩大空间**，而把第 4 个元素为非零的点称为**非无穷远点**。

无穷远点 $[a \quad b \quad c \quad 0]^T$ 的三元素 $a, b, c$ 称为该点的**方向数**。

也就是说，如果列向量最后一个元素是0，即代表方向；如果是1，则代表具体的方位和距离。

## 3.3 运动算子
总的说来，T矩阵有很多妙用，其中一个妙用就是T本身可以作为一个“动作”，比如有一个向量，想要平移和旋转，这个动作本身就可以用T来表示。

具体来说，可以把T拆分成旋转算子和平移算子，原理也很简单——不做什么动作，初始化就行了。

我们通过T的定义式也不难发现，T本身就是R和p组装起来的，所以如果只平移，就把R矩阵置为单位阵；如果只旋转，就把p的三个分量全部置零。如果是都想做，那就都加上去，就这么简单。

## 3.4 变换矩阵运算
我们来总的看一下T矩阵的所有妙用：

{{< math >}}
$$
\text{位姿变换} 
\begin{cases}
\text{1. 坐标系的描述} \\
\text{2. 坐标映射} \\
\text{3. 运动算子}
\end{cases}
$$
{{< /math >}}

其中第一个就是坐标系的相对位置。前面说过，刚体本身固接一个坐标系来存储它的相对位姿信息（位置和姿态）。那么这个T本身就可以表示刚体本身相对于另一个坐标系的位姿信息，在数学上也就是一个坐标系和另一个坐标系的相对位姿关系。

其二就是映射关系。同一个向量通过乘上一个T可以在{A}和{B}之间来回切换，当然不是位姿的改变，而是可以获得在不同坐标系视角中的位姿。

其三就是作为运动算子，本身作为一种动作存在。

### 3.4.1 变换矩阵相乘
变换极有可能不是一次变完的，可能涉及很多动作，所以要有个先后顺序，这在矩阵里表现为矩阵相乘。

但是矩阵相乘是没有交换律的，这在现实里也很好理解：顺序一换，位置极大概率都不一样。

所以在变换之前需要确定参考点：
**如果运动是相对于固定坐标系而言的，就需要按照先后顺序左乘矩阵；**
**如果运动是相对于运动坐标系而言的，就需要按照先后顺序右乘矩阵。**

顺便提一句，还能省一个章节：这种连乘的特性可以用来在多个坐标系之间来回切换。

### 3.4.2 变换矩阵求逆
{{< math >}}
$$
{}_A^B\boldsymbol{T} = {}_B^A\boldsymbol{T}^{-1} = \begin{bmatrix}
{}_B^A\boldsymbol{R}^T & -{}_B^A\boldsymbol{R}^T {}^A\boldsymbol{p}_{Bo} \\
\mathbf{0} & 1
\end{bmatrix}
$$
{{< /math >}}

一个公式完事儿，不多废话。顺便说一句：T左下角是子坐标系，左上角是父坐标系。

## 3.5 RPY角和欧拉角
### 3.5.1 RPY角
![RPY角](/images/pose-description-and-homogeneous-transformation/R-P-Y%20angles.png)

实际上RPY角就是前面3.4.1讲的**固定坐标系**，所以要左乘（按照先后顺序，从右向左乘）。

对于RPY角，实际上矩阵如下：

{{< math >}}
$$
{}_B^A\boldsymbol{R}_{xyz}(\gamma, \beta, \alpha) = \begin{bmatrix}
c\alpha c\beta & c\alpha s\beta s\gamma - s\alpha c\gamma & c\alpha s\beta c\gamma + s\alpha s\gamma \\
s\alpha c\beta & s\alpha s\beta s\gamma + c\alpha c\gamma & s\alpha s\beta c\gamma - c\alpha s\gamma \\
-s\beta & c\beta s\gamma & c\beta c\gamma
\end{bmatrix}
$$
{{< /math >}}

式中：$c\alpha = \cos\alpha$；$s\alpha = \sin\alpha$，$c\beta, s\beta$ 和 $c\gamma, s\gamma$ 依此类推。
$\alpha$、$\beta$、$\gamma$，就是图中所示的方向，是有先后顺序的。

现在来讨论它的**逆问题（RPY 角反解）**：从给定的旋转矩阵求出等价的绕固定轴 $x$-$y$-$z$ 的转角 $\gamma, \beta$ 和 $\alpha$。令

{{< math >}}
$$
{}_B^A\boldsymbol{R}_{xyz}(\gamma, \beta, \alpha) = \begin{bmatrix}
r_{11} & r_{12} & r_{13} \\
r_{21} & r_{22} & r_{23} \\
r_{31} & r_{32} & r_{33}
\end{bmatrix} 
$$
{{< /math >}}

如果 $\beta \neq \pm90^\circ$，则得到各个角的反正切表达式：

{{< math >}}
$$
\begin{cases}
\beta = \operatorname{Atan2}(-r_{31}, \sqrt{r_{11}^2 + r_{21}^2}) \\
\alpha = \operatorname{Atan2}(r_{21}, r_{11}) \\
\gamma = \operatorname{Atan2}(r_{32}, r_{33})
\end{cases}
$$
{{< /math >}}

式中的根式运算有两个解，总是取 $(-90^\circ, 90^\circ]$ 中的一个解。这样做通常可以定义方位元各种描述之间的一一对应关系。当然，有时也要计算出所有的解。

{{< math >}}
如果 $\beta = \pm 90^\circ, \cos\beta = 0$，则各式表示的反解退化。这时只可解出 $\alpha$ 与 $\gamma$ 的和或差，通常选择 $\alpha = 0^\circ$，从而解出结果如下：

假若 $\beta = 90^\circ$，则
$$
\beta = 90^\circ, \quad \alpha = 0, \quad \gamma = \operatorname{Atan2}(r_{12}, r_{22})
$$

假若 $\beta = -90^\circ$，则
$$
\beta = -90^\circ, \quad \alpha = 0, \quad \gamma = -\operatorname{Atan2}(r_{12}, r_{22})
$$
{{< /math >}}


### 3.5.2 欧拉角
欧拉角分为两种：z-y-x和z-y-z，实际上就是绕轴的先后顺序，不过与RPY角不同的是，RPY角是从始至终拿着固定的坐标系在转，而欧拉角本身是拿着在动的那个坐标系本身在看，这就是区别，对于这种，要用到3.4.1所讲的**运动坐标系**的方法，也就是右乘。

跟RPY角一样，欧拉角也有各自的旋转矩阵表示和逆问题，下面简单介绍：
#### z-y-x角

{{< math >}}
$$
{}_B^A\boldsymbol{R}_{zyx}(\alpha, \beta, \gamma) = \begin{bmatrix}
c\alpha c\beta & c\alpha s\beta s\gamma - s\alpha c\gamma & c\alpha s\beta c\gamma + s\alpha s\gamma \\
s\alpha c\beta & s\alpha s\beta s\gamma + c\alpha c\gamma & s\alpha s\beta c\gamma - c\alpha s\gamma \\
-s\beta & c\beta s\gamma & c\beta c\gamma
\end{bmatrix}
$$
{{< /math >}}

该矩阵与RPY角的矩阵完全一样，反解也一样。

#### z-y-z角
{{< math >}}
$$
\begin{aligned}
{}_B^A\boldsymbol{R}_{zyz}(\alpha, \beta, \gamma) &= \boldsymbol{R}(z, \alpha)\boldsymbol{R}(y, \beta)\boldsymbol{R}(z, \gamma) \\
&= \begin{bmatrix}
c\alpha c\beta c\gamma - s\alpha s\gamma & -c\alpha c\beta s\gamma - s\alpha c\gamma & c\alpha s\beta \\
s\alpha c\beta c\gamma + c\alpha s\gamma & -s\alpha c\beta s\gamma + c\alpha c\gamma & s\alpha s\beta \\
-s\beta c\gamma & s\beta s\gamma & c\beta
\end{bmatrix}
\end{aligned}
$$
{{< /math >}}

**反解问题：**

{{< math >}}
$$
{}_B^A\boldsymbol{R}_{zyz}(\alpha, \beta, \gamma) = \begin{bmatrix}
r_{11} & r_{12} & r_{13} \\
r_{21} & r_{22} & r_{23} \\
r_{31} & r_{32} & r_{33}
\end{bmatrix}
$$

$$
\begin{cases}
\beta = \operatorname{Atan2}(\sqrt{r_{31}^2 + r_{32}^2}, r_{33}) \\
\alpha = \operatorname{Atan2}(r_{23}, r_{13}) \\
\gamma = \operatorname{Atan2}(r_{32}, -r_{31})
\end{cases}
$$
{{< /math >}}

虽然 $\sin\beta = \sqrt{r_{31}^2 + r_{32}^2}$ 有两个解存在，但总是取 $[0, 180^\circ)$ 范围内的一个解。

{{< math >}}

如果 $\beta = 0^\circ$，则解为
$$
\beta = 0^\circ, \quad \alpha = 0^\circ, \quad \gamma = \operatorname{Atan2}(-r_{12}, r_{11})
$$

如果 $\beta = 180^\circ$，则解为
$$
\beta = 180^\circ, \quad \alpha = 0^\circ, \quad \gamma = \operatorname{Atan2}(-r_{12}, r_{11})
$$

{{< /math >}}



