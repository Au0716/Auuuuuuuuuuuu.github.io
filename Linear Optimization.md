# Linear Optimization

##  The Simplex Method

### Basic Idea

只要存在optimal solution就一定存在basic feasible solution是optimal的。那么只要一直寻找，一定能从这些basic feasible solution中找到optimal的。

Simplex Method的基本思路就是从一个basic feasible solution沿着edge出发（沿着cost减小的方向）找到下一个basic feasible solution，直到我们无法让cost更小了，那么这个solution就是locally optimal solution。幸运的是由于convex set的一些性质，locally optimal就等同于globally optimal。

### Optimality Condition

#### Finding Direction

对于simplex method，考虑standard form polyhedron$\{x\in R|Ax = b , x\ge0 \}$。 B(1),B(2),...B(m)为basic indices, $B = [A_{B(1)},...,A_{B(2)}]$为basis，$x_{B(1)},...,x_{B(2)}$为basic variables.

在寻找新的solution过程中，需要保证一直在polyhedron内部，要对方向进行限制，因此给出feasible direction的定义。

<u>***Def {Feasible Direction}***</u> 考虑x是polyhedron P中的一个点，vector $d \in R^n$是**feasible direction**，若存在$\theta \ge 0 $满足$x+\theta d \in P$

此前提到，要沿着polyhedron edge的方向寻找solution（特别地，是沿着保持部分non-basic variables不变的方向，因为好控制）。

那构建这个direction d的方式就是改变一个non-basic variable $x_j$使其为正，保持其他的non-basic variables仍为零。就有$d_j = 1$和其他non-basic variabels 的方向$d_i = 0$。

对于basic variables ，它们改变的方向basic directions记作$d_B = (d_{B(1)},...d_{B(m)})$。

##### Making the direction feasible 

不想要找到的solution在polyhedron外,所以d是feasible direction $x+\theta d \in P$

- 通过等式条件得到满足的$d_B$

$$
A(x+\theta d) - Ax = \theta A d=0
$$

$$
Ad = \sum_{i=1}^{m}A_{B(i)}d_{B(i)} + A_j =0 \\
d_B = -B^{-1}A_j
$$

- 还需要满足variables的非负性。这里通过一张图来说明，只要出发的点是non-degenerate solution就一定可以保持非负，但如果是degenerate solution就不一定。对于A点无论是沿着$x_1= 0$还是$x_2 = 0$的方向移动，始终是在P内部的。但是对于B如果沿着$x_4 = 0 $的方向移动那么一定会出P.![IMG_11F1DA8CE6B3-1](/Users/jinzhiyuan/Desktop/笔记/IMG_11F1DA8CE6B3-1.jpeg)

#### Reduced Cost

现在来看看在这个方向上移动对cost function有什么影响。

在进行移动后cost的改变为 $cx-c(x+\theta d) = \theta cd$，我们现在只关心这个移动能否带来cost的减小所以可以先不考虑$\theta$目前只考虑变化率cd.
$$
cd = \sum_{i ==1}^{m}d_{B(i)}c_i + c_j = c_j - c_{B}'B^{-1}A_j
$$
***<u>Def{Reduced Cost}</u>***x是basic vairable, B是Basis,$c_B$是basic variable对应的cost.对于每一个j，我们定义$x_j$的reduced cost $\overline c_j = c_j - c_B'B^{-1}A_j$

值得一题的是对于basic variable的reduced cost $\hat c_{B(i)} = 0$，对于任意的i = 1,...,m。不难证明，$B^{-1}A_j = e_j$，其中$e_j$是第j项为1，其余项为0的向量。这是因为$B = (A_{B(1)},...,A_{B(m)})$。因此$\overline c_{B(i)} = c_{B(i)}- c_B'e_{B(i)} = 0$

#### Optimality Condition

在定义了reduced cost后我们现在可以正式判断我们找到的x是不是optimal的了。

<u>***Thereom3.1{Optimality Condition}***</u>对于basic feasible solution x，以及与其相对应的basis B，和与其相对应的reduced cost $\overline c$，有如下的结果。
(a)如果$\overline{c} \ge0$，那么x是optimal的
(b)如果x是optimal且non-degenerate的，那么$\overline{c} \ge 0$
***Proof:*** 
(a): **思路：证明在任意的点中x的cost最小即可**。任意选取一个basic feasible solution $y$. 有$Ax = Ay = b$,令$\theta d = y -x$.容易得到：
$$
Ad = Bd_B + \sum_{i \in N}A_id_i = 0 \\
d_B = -\sum_{i\in N}B^{-1}A_id_i
$$
其中N代表所有nonbasic indices. 由于$\theta$是任意正数，那么如果$c'd\ge0$那么就一定有$c'y\ge c'x$。
$$
c'd = c_B'd_B +\sum_{i\in N}c_id_i &=\sum_{i\in N}c_id_i - \sum_{i\in N}c_B'B^{-1}A_id_i \\	
&= \sum_{i \in N}(c_i - c_B'B^{-1}A_i)d_i
$$
*由于reduced cost$\overline c \ge 0 $故$c'd\ge 0$.*
(b)***思路：反证法，如果reduced cost的某些项小于零，那么x就不可能是optimal的。***假设x是non degenerate basic solution且有nonbasic indice j使得$\overline c_j < 0 $.由于x non-degenerate，沿着$x_j$增加的方向一定是feasible direction(见图)， $\overline {c_j}<0$因此沿着这个方向移动cost会变小存在比x cost更小的点，x不是optimal的。与条件矛盾因此$\overline c \ge 0 $

对于theorem3.1值得一提的是，如果x是degenerate solution且部分$\overline c_j<0$，x仍有可能是optimal的，对于这种情况不能用reduced cost来判断了。

​	
