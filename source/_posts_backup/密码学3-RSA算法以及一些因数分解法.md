# 密码学（三）RSA算法以及一些因数分解法

## **RSA基本架构：**

RSA算法是由三个人的姓氏合在一起组成的，他们分别是Rivest, Shamir, Adleman。这个密码系统是一个极其常见的密码系统。以下就是关于密码系统的基本架构

首先，Alice和Bob选定两个素数 ![p](https://www.zhihu.com/equation?tex=p) 和 ![q](https://www.zhihu.com/equation?tex=q) ，再选择一个数 ![e](https://www.zhihu.com/equation?tex=e) ，使得 ![gcd\(e,\(p-1\)\(q-1\)\)=1](https://www.zhihu.com/equation?tex=gcd%28e%2C%28p-1%29%28q-1%29%29%3D1) 。发布公钥 ![N=pq](https://www.zhihu.com/equation?tex=N%3Dpq) 以及 ![e](https://www.zhihu.com/equation?tex=e) 。

接下来是Alice的加密过程，她将原文 ![m](https://www.zhihu.com/equation?tex=m) 做如下运算： ![c\\equiv m^{e}\(mod \\ N\)](https://www.zhihu.com/equation?tex=c%5Cequiv+m%5E%7Be%7D%28mod+%5C+N%29) 。将密文 ![c](https://www.zhihu.com/equation?tex=c) 发给Bob。

Bob首先计算出 ![d](https://www.zhihu.com/equation?tex=d) ， ![ed\\equiv 1\(mod \\ \(p-1\)\(q-1\)\)](https://www.zhihu.com/equation?tex=ed%5Cequiv+1%28mod+%5C+%28p-1%29%28q-1%29%29) 。再计算 ![m^{'}\\equiv c^{d} \(mod \\ N\)](https://www.zhihu.com/equation?tex=m%5E%7B%27%7D%5Cequiv+c%5E%7Bd%7D+%28mod+%5C+N%29) 。 ![m^{'}](https://www.zhihu.com/equation?tex=m%5E%7B%27%7D) 即为原文 ![m](https://www.zhihu.com/equation?tex=m) 。

原因： ![m^{'}\\equiv c^{d} \\equiv m^{ed} \\equiv m^{k\(p-1\)\(q-1\)+1} \\equiv m \(mod \\ N\)](https://www.zhihu.com/equation?tex=m%5E%7B%27%7D%5Cequiv+c%5E%7Bd%7D+%5Cequiv+m%5E%7Bed%7D+%5Cequiv+m%5E%7Bk%28p-1%29%28q-1%29%2B1%7D+%5Cequiv+m+%28mod+%5C+N%29) 。最后一步是因为 ![\\varphi\(N\)=\\varphi \(pq\)=\(p-1\)\(q-1\)](https://www.zhihu.com/equation?tex=%5Cvarphi%28N%29%3D%5Cvarphi+%28pq%29%3D%28p-1%29%28q-1%29) ，再使用欧拉定理即可。

RSA方法不仅在实数域上适用，在椭圆曲线中也同样适用，目前广泛使用的也是在椭圆曲线上的RSA算法，这是因为实数域上的RSA算法有很多隐患，这些隐患有的时候是难以避免的。接下来就是如何在特殊情况下攻破RSA算法。

### Woman in the middle attack

我们假设Eva只是不知道p和q是什么，但是她完全掌握了Alice和Bob的RSA系统运作机理，而且她还可以在收到Alice传来的密文 ![c](https://www.zhihu.com/equation?tex=c) 之后，给Bob发一个经过修改的假密文 ![c^{'}](https://www.zhihu.com/equation?tex=c%5E%7B%27%7D) 。（当然这种情况需要Alice和Bob至少用同一套系统互相传递一次信息）那么这种情况下RSA系统将会丧失安全性，因为这种情况下Eva根本不需要费九牛二虎之力去分解 ![N](https://www.zhihu.com/equation?tex=N) 。她只需要做如下操作即可：

在她收到Alice传来的密文 ![c](https://www.zhihu.com/equation?tex=c) 之后，她随便选一个 ![N](https://www.zhihu.com/equation?tex=N) 以下的正整数 ![k](https://www.zhihu.com/equation?tex=k) ，并计算 ![c^{'}\\equiv k^{e}c\(mod \\ N\)](https://www.zhihu.com/equation?tex=c%5E%7B%27%7D%5Cequiv+k%5E%7Be%7Dc%28mod+%5C+N%29) 。把这个假密文 ![c^{'}](https://www.zhihu.com/equation?tex=c%5E%7B%27%7D) 送给Bob。那么Bob依然会对这个假密文做出原始操作： ![\(c^{'}\)^{d}\\equiv \(k^{e}c\)^{d} \\equiv \(k^{e}m^{e}\)^{d} \\equiv km \(mod \\ N\)](https://www.zhihu.com/equation?tex=%28c%5E%7B%27%7D%29%5E%7Bd%7D%5Cequiv+%28k%5E%7Be%7Dc%29%5E%7Bd%7D+%5Cequiv+%28k%5E%7Be%7Dm%5E%7Be%7D%29%5E%7Bd%7D+%5Cequiv+km+%28mod+%5C+N%29) 。

这个时候Eva得到的是 ![km](https://www.zhihu.com/equation?tex=km) 。可是 ![k](https://www.zhihu.com/equation?tex=k) 就是Eva自己挑的，算出 ![m](https://www.zhihu.com/equation?tex=m) 也是轻而易举的事。

这种情况被称为Woman in the middle attack。

## **几种素数分解法**

我们之前已经知道素数分解 ![N](https://www.zhihu.com/equation?tex=N) 在一般情况下是 ![o\(\\sqrt{N}\)](https://www.zhihu.com/equation?tex=o%28%5Csqrt%7BN%7D%29) 的算法复杂度。可是在一些特殊情况下，有更快的分解方法。虽然这些分解方法需要依赖运气。

### **Miller-Robin** **素数判别法:**

这个方法可以用来迅速判别某个数是否是素数，可是我们为什么不用费马小定理直接判别呢，如果对于所有的 ![1\\leq a \\leq n](https://www.zhihu.com/equation?tex=1%5Cleq+a+%5Cleq+n) ,都有 ![a^{n}\\equiv a （mod \\ n\)](https://www.zhihu.com/equation?tex=a%5E%7Bn%7D%5Cequiv+a+%EF%BC%88mod+%5C+n%29) 的话， ![n](https://www.zhihu.com/equation?tex=n) 是不是素数呢？

我们只需要考虑 ![n](https://www.zhihu.com/equation?tex=n) 为合数的情况。

答案是错误的，561就是一个例子，这样的数被称作Carmichael Number。这样的数有无穷多个，所以我们需要更强的结论。

先给出重要定义

定义：如果 ![a^{n}\\equiv a \(mod \\ n\)](https://www.zhihu.com/equation?tex=a%5E%7Bn%7D%5Cequiv+a+%28mod+%5C+n%29) 不成立。那么将 ![a](https://www.zhihu.com/equation?tex=a) 称作 ![n](https://www.zhihu.com/equation?tex=n) 的witness。

如果 ![n](https://www.zhihu.com/equation?tex=n) 有witness存在，那么它一定不是素数。

Miller-Robin分解法的依据是下面这个定理：

![p](https://www.zhihu.com/equation?tex=p) 是一个奇素数，那么 ![p-1=2^{k}q](https://www.zhihu.com/equation?tex=p-1%3D2%5E%7Bk%7Dq) 。 ![gcd\(a,p\)=1](https://www.zhihu.com/equation?tex=gcd%28a%2Cp%29%3D1) 。那么以下两种可能必有一种是对的：

（1）： ![a^{q}\\equiv 1 \(mod \\ p\)](https://www.zhihu.com/equation?tex=a%5E%7Bq%7D%5Cequiv+1+%28mod+%5C+p%29)

（2）： ![a^{q},a^{2q},a^{4q}...a^{2^{k-1}q}](https://www.zhihu.com/equation?tex=a%5E%7Bq%7D%2Ca%5E%7B2q%7D%2Ca%5E%7B4q%7D...a%5E%7B2%5E%7Bk-1%7Dq%7D) 至少有一个模 ![p](https://www.zhihu.com/equation?tex=p) 余 ![-1](https://www.zhihu.com/equation?tex=-1) 。

这个结论是费马小定理的直接推论。

现在将奇素数 ![p](https://www.zhihu.com/equation?tex=p) 换成我们想要判定的数 ![n](https://www.zhihu.com/equation?tex=n) 。对于 ![n](https://www.zhihu.com/equation?tex=n) ，如果 ![a](https://www.zhihu.com/equation?tex=a) 不满足以上两个结论，那么称 ![a](https://www.zhihu.com/equation?tex=a) 为 ![n](https://www.zhihu.com/equation?tex=n) 的Miller-Robin witness。

目前仅仅知道如果 ![n](https://www.zhihu.com/equation?tex=n) 是奇素数，那么以上两式必定成立，可是如果 ![n](https://www.zhihu.com/equation?tex=n) 不是奇素数，那么以上两式就一定不成立吗？换句话说，对于 奇合数 ![n](https://www.zhihu.com/equation?tex=n) ，我们一定能找到足够的Miller-Robin witness吗？

答案是肯定的，因为数论中有如下结论：**对于奇合数** ![n](https://www.zhihu.com/equation?tex=n) ，**在** ![1  ](https://www.zhihu.com/equation?tex=1++) **到** ![n-1](https://www.zhihu.com/equation?tex=n-1) **之中，至少有** ![\\frac{3}{4}](https://www.zhihu.com/equation?tex=%5Cfrac%7B3%7D%7B4%7D) **的数是Miller-Robin witness。** 也就是说如果尝试一个数不成功的话，可以多尝试几个数，对于特定的奇合数 ![n](https://www.zhihu.com/equation?tex=n) 。如果用十个数去尝试，最后判定错误的概率就只有 ![\(\\frac{1}{4}\)^{10}](https://www.zhihu.com/equation?tex=%28%5Cfrac%7B1%7D%7B4%7D%29%5E%7B10%7D) ，可以认为这个方法是奏效的了。

接下来是一个基于黎曼猜想之上的，对Miller-Robin witness的上界估计：

如果黎曼猜想成立，那么 ![a\\leq 2\(ln \\ n\)^{2}](https://www.zhihu.com/equation?tex=a%5Cleq+2%28ln+%5C+n%29%5E%7B2%7D) 。

### **Pollard's p-1 分解法：**

这个方法由Pollard提出，是一种基于费马小定理的分解方法，适用于当 ![N](https://www.zhihu.com/equation?tex=N) 为两个素数的乘积时的分解。

思路如下：

找到一个数 ![L](https://www.zhihu.com/equation?tex=L) 是 ![p-1](https://www.zhihu.com/equation?tex=p-1) 的倍数，但不是 ![q-1](https://www.zhihu.com/equation?tex=q-1) 的倍数。 ![p=gcd\(a^{L}-1,\\ N\)](https://www.zhihu.com/equation?tex=p%3Dgcd%28a%5E%7BL%7D-1%2C%5C+N%29) 。

实际上，可以让 ![L=n!](https://www.zhihu.com/equation?tex=L%3Dn%21) 。

设定初始值 ![a=2](https://www.zhihu.com/equation?tex=a%3D2) 。选取初始幂次 ![j=2](https://www.zhihu.com/equation?tex=j%3D2) ，令 ![a=a^{j}](https://www.zhihu.com/equation?tex=a%3Da%5E%7Bj%7D) 。

计算 ![gcd\(a,N\)](https://www.zhihu.com/equation?tex=gcd%28a%2CN%29) ，如果 ![gcd\(a,N\)=1](https://www.zhihu.com/equation?tex=gcd%28a%2CN%29%3D1) ,那么 ![j=j+1](https://www.zhihu.com/equation?tex=j%3Dj%2B1) ，重新计算 ![a=a^{j}](https://www.zhihu.com/equation?tex=a%3Da%5E%7Bj%7D) ，重复循环。

如果 ![gcd\(a,N\)=N](https://www.zhihu.com/equation?tex=gcd%28a%2CN%29%3DN) ，选取一个新 ![a](https://www.zhihu.com/equation?tex=a) ，令 ![j=2](https://www.zhihu.com/equation?tex=j%3D2) ，循环重新开始。

如果 ![1<gcd\(a,N\)<N](https://www.zhihu.com/equation?tex=1%3Cgcd%28a%2CN%29%3CN) 。那么我们得到了 ![N](https://www.zhihu.com/equation?tex=N) 的因数 ![p](https://www.zhihu.com/equation?tex=p) ， ![p=gcd\(a,N\)](https://www.zhihu.com/equation?tex=p%3Dgcd%28a%2CN%29) 。

在这种方法下，如果 ![p-1](https://www.zhihu.com/equation?tex=p-1) 是多个小素数的乘积，分解速度是很快的。

### **平方差分解法：**

**这个方法是目前为止最快的一种分解法！**

基本原理： ![N=X^{2}-Y^{2}](https://www.zhihu.com/equation?tex=N%3DX%5E%7B2%7D-Y%5E%7B2%7D) 。那么 ![N=\(X+Y\)\(X-Y\)](https://www.zhihu.com/equation?tex=N%3D%28X%2BY%29%28X-Y%29) 。那么 ![X+Y](https://www.zhihu.com/equation?tex=X%2BY) 与 ![X-Y](https://www.zhihu.com/equation?tex=X-Y) 其中的一个是 ![N](https://www.zhihu.com/equation?tex=N) 的因数。

**基本操作如下：**

**（1）：我们可以选取一组数![a_{1}^{2}\\equiv c_{1}\(mod \\ N\)，a_{2}^{2}\\equiv c_{2}\(mod \\ N\)...a_{n}^{2}\\equiv c_{n}\(mod \\ N\)](https://www.zhihu.com/equation?tex=a_%7B1%7D%5E%7B2%7D%5Cequiv+c_%7B1%7D%28mod+%5C+N%29%EF%BC%8Ca_%7B2%7D%5E%7B2%7D%5Cequiv+c_%7B2%7D%28mod+%5C+N%29...a_%7Bn%7D%5E%7B2%7D%5Cequiv+c_%7Bn%7D%28mod+%5C+N%29) 。并将每个 ![c_{i}](https://www.zhihu.com/equation?tex=c_%7Bi%7D) 分解成小素数的乘积。**

**（2）：从这个序列中选取一个子列![c_{i_{1}},c_{i_{2}}...c_{i_{n}}](https://www.zhihu.com/equation?tex=c_%7Bi_%7B1%7D%7D%2Cc_%7Bi_%7B2%7D%7D...c_%7Bi_%7Bn%7D%7D) 。 使得这个子列的乘积 ![c_{i_{1}}c_{i_{2}}...c_{i_{n}}= b^{2} ](https://www.zhihu.com/equation?tex=c_%7Bi_%7B1%7D%7Dc_%7Bi_%7B2%7D%7D...c_%7Bi_%7Bn%7D%7D%3D+b%5E%7B2%7D+) 依旧是一个平方数。**

**（3）：根据（1）中的等式，我们有了两个平方数：![a^{2}\\equiv a_{i_{1}}^{2}a_{i_{2}}^{2}...a_{i_{n}}^{2}\\equiv c_{i_{1}}c_{i_{2}}...c_{i_{n}}\\equiv b^{2}\(mod \\ N\)](https://www.zhihu.com/equation?tex=a%5E%7B2%7D%5Cequiv+a_%7Bi_%7B1%7D%7D%5E%7B2%7Da_%7Bi_%7B2%7D%7D%5E%7B2%7D...a_%7Bi_%7Bn%7D%7D%5E%7B2%7D%5Cequiv+c_%7Bi_%7B1%7D%7Dc_%7Bi_%7B2%7D%7D...c_%7Bi_%7Bn%7D%7D%5Cequiv+b%5E%7B2%7D%28mod+%5C+N%29) 。计算 ![d=gcd\(N,a-b\)](https://www.zhihu.com/equation?tex=d%3Dgcd%28N%2Ca-b%29) 。**

在实际应用中，常常将 ![c_{i}](https://www.zhihu.com/equation?tex=c_%7Bi%7D) 表作素数的乘积：

![c_{i}=p_{1}^{e_{i1}}p_{2}^{e_{i2}}...p_{m}^{e_{im}}](https://www.zhihu.com/equation?tex=c_%7Bi%7D%3Dp_%7B1%7D%5E%7Be_%7Bi1%7D%7Dp_%7B2%7D%5E%7Be_%7Bi2%7D%7D...p_%7Bm%7D%5E%7Be_%7Bim%7D%7D) ， ![i \\in \\left\\{ 1，2... \\ r \\right\\}](https://www.zhihu.com/equation?tex=i+%5Cin+%5Cleft%5C%7B+1%EF%BC%8C2...+%5C+r+%5Cright%5C%7D)

那么在序列 ![{c_{n}}](https://www.zhihu.com/equation?tex=%7Bc_%7Bn%7D%7D) 中取子列的操作即为对于 ![u_{1},u_{2}...u_{r}\\in \\left\\{ 0,1 \\right\\}](https://www.zhihu.com/equation?tex=u_%7B1%7D%2Cu_%7B2%7D...u_%7Br%7D%5Cin+%5Cleft%5C%7B+0%2C1+%5Cright%5C%7D) 。 那么![c_{i_{1}}c_{i_{2}}...c_{i_{n}}=c_{1}^{u_{1}}c_{2}^{u_{2}}...c_{r}^{u_{r}}](https://www.zhihu.com/equation?tex=c_%7Bi_%7B1%7D%7Dc_%7Bi_%7B2%7D%7D...c_%7Bi_%7Bn%7D%7D%3Dc_%7B1%7D%5E%7Bu_%7B1%7D%7Dc_%7B2%7D%5E%7Bu_%7B2%7D%7D...c_%7Br%7D%5E%7Bu_%7Br%7D%7D)

将 ![c_{i}=p_{1}^{e_{i1}}p_{2}^{e_{i2}}...p_{m}^{e_{im}}](https://www.zhihu.com/equation?tex=c_%7Bi%7D%3Dp_%7B1%7D%5E%7Be_%7Bi1%7D%7Dp_%7B2%7D%5E%7Be_%7Bi2%7D%7D...p_%7Bm%7D%5E%7Be_%7Bim%7D%7D) 代入可推得如果这个序列的乘积是一个平方数，那么：

![e_{11}u_{1}+e_{21}u_{2}+...+e_{r1}u_{r}\\equiv 0 \(mod \\ 2\)](https://www.zhihu.com/equation?tex=e_%7B11%7Du_%7B1%7D%2Be_%7B21%7Du_%7B2%7D%2B...%2Be_%7Br1%7Du_%7Br%7D%5Cequiv+0+%28mod+%5C+2%29)

![e_{12}u_{1}+e_{22}u_{2}+...+e_{r2}u_{r}\\equiv 0 \(mod \\ 2\)](https://www.zhihu.com/equation?tex=e_%7B12%7Du_%7B1%7D%2Be_%7B22%7Du_%7B2%7D%2B...%2Be_%7Br2%7Du_%7Br%7D%5Cequiv+0+%28mod+%5C+2%29)

![......](https://www.zhihu.com/equation?tex=......)

![e_{1m}u_{1}+e_{2m}u_{2}+...+e_{rm}u_{r}\\equiv 0 \(mod \\ 2\)](https://www.zhihu.com/equation?tex=e_%7B1m%7Du_%7B1%7D%2Be_%7B2m%7Du_%7B2%7D%2B...%2Be_%7Brm%7Du_%7Br%7D%5Cequiv+0+%28mod+%5C+2%29)

当 ![N](https://www.zhihu.com/equation?tex=N) 很大时，这个矩阵会非常大，为了简化计算，我们需要在三个步骤上做一些文章。从步骤（1）开始，我们在选取子列 ![c_{i_{n}}](https://www.zhihu.com/equation?tex=c_%7Bi_%7Bn%7D%7D) 时，需要使得 ![c_{i_{n}}](https://www.zhihu.com/equation?tex=c_%7Bi_%7Bn%7D%7D) 能够被分解为小素数的乘积，以便于之后配平方数的计算。也就是说 ![c_{i_{n}}](https://www.zhihu.com/equation?tex=c_%7Bi_%7Bn%7D%7D) 的所有素因子都不能超过一个上界 ![B](https://www.zhihu.com/equation?tex=B) 。由此导出如下定义：

**如果一个正整数![n](https://www.zhihu.com/equation?tex=n) ，它的所有素因子都小于 ![B](https://www.zhihu.com/equation?tex=B) ，那么我们称它为B光滑数（B smooth number）。**

接下来是一个数论中的定理，它给出了B光滑数的上界：

**对于固定的![\\varepsilon \\in\( 0，1 \)](https://www.zhihu.com/equation?tex=%5Cvarepsilon+%5Cin%28+0%EF%BC%8C1+%29) ， ![X](https://www.zhihu.com/equation?tex=X) 和 ![B](https://www.zhihu.com/equation?tex=B) 满足 ![\(ln\\ X\)^{\\varepsilon}< ln \\ B<\(ln\\ X\)^{1-\\varepsilon}](https://www.zhihu.com/equation?tex=%28ln%5C+X%29%5E%7B%5Cvarepsilon%7D%3C+ln+%5C+B%3C%28ln%5C+X%29%5E%7B1-%5Cvarepsilon%7D) 。**

**记![u=\\frac{ln \\ X}{ln \\ B}](https://www.zhihu.com/equation?tex=u%3D%5Cfrac%7Bln+%5C+X%7D%7Bln+%5C+B%7D) ，用 ![\\psi\(X,B\)](https://www.zhihu.com/equation?tex=%5Cpsi%28X%2CB%29) 表示小于 ![X](https://www.zhihu.com/equation?tex=X) 的B光滑数的个数。**

**![\\psi\(X,B\)=Xu^{-u\(1+o\(1\)\)}](https://www.zhihu.com/equation?tex=%5Cpsi%28X%2CB%29%3DXu%5E%7B-u%281%2Bo%281%29%29%7D)。**

光滑数进一步的内容，跟素数分布有关：

**素数定理（The Prime Number Theorem):**

**我们用![\\pi\(X\)](https://www.zhihu.com/equation?tex=%5Cpi%28X%29) 表示小于 ![X](https://www.zhihu.com/equation?tex=X) 的素数个数：**

**那么：![\\lim_{X \\rightarrow +\\infty}{\\frac{\\pi\(X\)}{X/ln \\ X}}=1](https://www.zhihu.com/equation?tex=%5Clim_%7BX+%5Crightarrow+%2B%5Cinfty%7D%7B%5Cfrac%7B%5Cpi%28X%29%7D%7BX%2Fln+%5C+X%7D%7D%3D1) 。**

**换言之，对于小于![N](https://www.zhihu.com/equation?tex=N) 的正整数，随便挑选其中的一个，是素数的概率为 ![\\frac{1}{ln \\ N}](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7Bln+%5C+N%7D) 。**

**引申一下黎曼ζ函数：**

![\\zeta\(s\)=\\sum_{n=1}^{\\infty}{\\frac{1}{n^{s}}}=\\prod_{p\\ is \\ prime}^{}\(1-\\frac{1}{p^{s}}\)^{-1}](https://www.zhihu.com/equation?tex=%5Czeta%28s%29%3D%5Csum_%7Bn%3D1%7D%5E%7B%5Cinfty%7D%7B%5Cfrac%7B1%7D%7Bn%5E%7Bs%7D%7D%7D%3D%5Cprod_%7Bp%5C+is+%5C+prime%7D%5E%7B%7D%281-%5Cfrac%7B1%7D%7Bp%5E%7Bs%7D%7D%29%5E%7B-1%7D) 。

如果黎曼猜想成立，对素数定理就有更好的估计（实际上是黎曼猜想的等价形式）：

![\\pi\(X\)=\\int_{2}^{X}\\frac{dt}{ln \\ t}+o\(\\sqrt{X}ln \\ X\)](https://www.zhihu.com/equation?tex=%5Cpi%28X%29%3D%5Cint_%7B2%7D%5E%7BX%7D%5Cfrac%7Bdt%7D%7Bln+%5C+t%7D%2Bo%28%5Csqrt%7BX%7Dln+%5C+X%29) 。

实际上，对于B光滑数的分布，跟函数 ![L\(X\)=e^{\\sqrt{\(ln \\ X\)\\ \(ln\\ ln \\ X\)}}](https://www.zhihu.com/equation?tex=L%28X%29%3De%5E%7B%5Csqrt%7B%28ln+%5C+X%29%5C+%28ln%5C+ln+%5C+X%29%7D%7D) 有关。

回到B光滑数的性质：

我们引入一种被称为“**平方筛法”（Pomerance's quadratic sieve)** 的方法。对于 ![N<2^{350}](https://www.zhihu.com/equation?tex=N%3C2%5E%7B350%7D) 。这种方法的分解速度是最快的。对于更大的数，“**数域筛法”（Number field sieve)** 取代了平方筛法，成为最快的因数分解法。这两种方法都建立在平方差分解法的基础之上。我们现在需要的，仅仅是一个快速寻找B光滑数的方法。

**平方筛法（Pomerance's quadratic sieve)：**

我们需要用到函数 ![F\(T\)=T^{2}-N](https://www.zhihu.com/equation?tex=F%28T%29%3DT%5E%7B2%7D-N) ， ![a=](https://www.zhihu.com/equation?tex=a%3D)

对于序列： ![F\(a\),F\(a+1\),...F\(b\)](https://www.zhihu.com/equation?tex=F%28a%29%2CF%28a%2B1%29%2C...F%28b%29) 。我们仿照筛法，将小于B的素数全部除掉，最后对于某些数 ![i](https://www.zhihu.com/equation?tex=i) 。如果最终 ![F\(i\)=1](https://www.zhihu.com/equation?tex=F%28i%29%3D1) 。那么初始的 ![F\(i\)](https://www.zhihu.com/equation?tex=F%28i%29) 是B光滑数。

具体操作如下：

**所有小于B的素数（以及素数的幂次）构成一个集合，这个集合被称作因数基（Factor base)。**

![p](https://www.zhihu.com/equation?tex=p) 是因数基中的一个数，对于 ![t^{2}\\equiv N\(mod \\ p\)](https://www.zhihu.com/equation?tex=t%5E%7B2%7D%5Cequiv+N%28mod+%5C+p%29) ,由两个答案 ![\\alpha_{p},\\beta_{p}](https://www.zhihu.com/equation?tex=%5Calpha_%7Bp%7D%2C%5Cbeta_%7Bp%7D) 。

那么我们只需要用 ![p](https://www.zhihu.com/equation?tex=p) 去除 ![F\(\\alpha_{p}\),F\(\\alpha_{p}+p\),F\(\\alpha_{p}+2p\)...](https://www.zhihu.com/equation?tex=F%28%5Calpha_%7Bp%7D%29%2CF%28%5Calpha_%7Bp%7D%2Bp%29%2CF%28%5Calpha_%7Bp%7D%2B2p%29...) ， ![F\(\\beta_{p}\),F\(\\beta_{p}+p\),F\(\\beta_{p}+2p\)...](https://www.zhihu.com/equation?tex=F%28%5Cbeta_%7Bp%7D%29%2CF%28%5Cbeta_%7Bp%7D%2Bp%29%2CF%28%5Cbeta_%7Bp%7D%2B2p%29...)

当我们把因数基中的所有元素都循环过一回后，那些值为1的函数对应的原来的值就是我们要找的B光滑数。

运算次数大约为 ![L\(N\)](https://www.zhihu.com/equation?tex=L%28N%29) 。

**数域筛法（Number field sieve)**

完整的数域筛法是在环上运行的，整数是环的特殊形式，所以这里的数域筛法只是一个简化的概括版本。

选取一个不可约首一的整多项式 ![f\(m\)\\equiv 0\(mod \\ N\)](https://www.zhihu.com/equation?tex=f%28m%29%5Cequiv+0%28mod+%5C+N%29) 。

![d](https://www.zhihu.com/equation?tex=d) 是 ![f\(x\)](https://www.zhihu.com/equation?tex=f%28x%29) 的度数， ![\\beta](https://www.zhihu.com/equation?tex=%5Cbeta) 是 ![f\(x\)](https://www.zhihu.com/equation?tex=f%28x%29) 的根。

有 ![Z\[\\beta\]=\\left\\{ c_{0}+c_{1}\\beta+...+c_{d-1}\\beta^{d-1}, c_{0},c_{1},c_{d-1} \\in Z\\right\\}](https://www.zhihu.com/equation?tex=Z%5B%5Cbeta%5D%3D%5Cleft%5C%7B+c_%7B0%7D%2Bc_%7B1%7D%5Cbeta%2B...%2Bc_%7Bd-1%7D%5Cbeta%5E%7Bd-1%7D%2C+c_%7B0%7D%2Cc_%7B1%7D%2Cc_%7Bd-1%7D+%5Cin+Z%5Cright%5C%7D)

找到一个数组 ![\(a_{1},b_{1}\),\(a_{2},b_{2}\)...\(a_{k},b_{k}\)](https://www.zhihu.com/equation?tex=%28a_%7B1%7D%2Cb_%7B1%7D%29%2C%28a_%7B2%7D%2Cb_%7B2%7D%29...%28a_%7Bk%7D%2Cb_%7Bk%7D%29) 满足 ![\\prod_{i=1}^{k}\(a_{i}-b_{i}m\)=A^{2}](https://www.zhihu.com/equation?tex=%5Cprod_%7Bi%3D1%7D%5E%7Bk%7D%28a_%7Bi%7D-b_%7Bi%7Dm%29%3DA%5E%7B2%7D) ， ![\\prod_{i=1}^{k}\(a_{i}-b_{i}\\beta\)=\\alpha^{2}](https://www.zhihu.com/equation?tex=%5Cprod_%7Bi%3D1%7D%5E%7Bk%7D%28a_%7Bi%7D-b_%7Bi%7D%5Cbeta%29%3D%5Calpha%5E%7B2%7D) 。

且 ![\\alpha\\equiv c_{0}+c_{1}m+...+c_{d-1}m^{d-1}\(mod \\ N\)](https://www.zhihu.com/equation?tex=%5Calpha%5Cequiv+c_%7B0%7D%2Bc_%7B1%7Dm%2B...%2Bc_%7Bd-1%7Dm%5E%7Bd-1%7D%28mod+%5C+N%29) 。

由于 ![m\\equiv \\beta \(mod \\ N\)](https://www.zhihu.com/equation?tex=m%5Cequiv+%5Cbeta+%28mod+%5C+N%29) 以及 ![A^{2}\\equiv \\alpha^{2} \(mod \\ N\)](https://www.zhihu.com/equation?tex=A%5E%7B2%7D%5Cequiv+%5Calpha%5E%7B2%7D+%28mod+%5C+N%29) 。

推得 ![A^{2}\\equiv \(c_{0}+c_{1}m+...+c_{d-1}m^{d-1}\)^{2}\(mod \\ N\)](https://www.zhihu.com/equation?tex=A%5E%7B2%7D%5Cequiv+%28c_%7B0%7D%2Bc_%7B1%7Dm%2B...%2Bc_%7Bd-1%7Dm%5E%7Bd-1%7D%29%5E%7B2%7D%28mod+%5C+N%29) 。

计算 ![gcd\(A-B,N\)](https://www.zhihu.com/equation?tex=gcd%28A-B%2CN%29) 即可。

实际上这只是算法步骤，很多理论是不完全的，比如 ![Z\[\\beta\]](https://www.zhihu.com/equation?tex=Z%5B%5Cbeta%5D) 没有唯一分解，即使它的理想都没有唯一分解。但是这些理论上的问题最后都被解决了，解决过程是非常繁琐的。具体可参见C.Pomerance的文章 A tale of two sieves。理论上的问题解决后，这个算法的正确性就没问题了。但是由于数域筛法繁琐的步骤，它只有在处理非常大的数的时候才能发挥威力。虽然如此，数域筛法依然是目前为止最快的一种分解素因数的方法。