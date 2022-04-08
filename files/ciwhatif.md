<script type="text/javascript" src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
<script type="text/javascript" src="outline-for-typora-master/outline/outline.js"></script>

<link rel="stylesheet" type="text/css" href="outline-for-typora-master/outline/outline.css">

## Part I Causal inference without model

### 1. causal effect and association

#### causation

* 某个本应发生的结果（outcome），比如说病重死亡，在某种治疗、干涉（treatment / intervention）下，这个结果改变了，说明产生了因果影响。

* **Individual causal effect** treatment $A$ has a causal effect on outcome $Y$ if $Y^{a=1}\neq Y^{a=0}$. 

* **average causal effect** $P(Y^{a=1}=1)-P(Y^{a=0}=1)$
  * $Y^{a=1}$和$Y^{a=0}$被称为conterfactual theory
  * 在实际情况下，只能得到$Y^{a=1}$或$Y^{a=0}$。（one of the two conterfactual outcome）

#### association

* 在现实中，我们只能得到$P(Y=1|A=1)-P(Y=1|A=0)$

* association vs causation![image-20220317155411437](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220317155411437.png)

  * 研究因果性：我们假设有两个平行宇宙，一个宇宙中所有被检测的对象都接受了治疗，另一个宇宙中所有对象都没有接受治疗，然后我们比较两个宇宙中整个群体结果的差异。

  * 研究相关性：观察的对象中，一部分接受治疗，另一部分不接受治疗，观察两个群体结果的差异。

  * 相关性$\neq$因果性的例子：假设共有10人参与心脏移植手术，10人不参与。然而，参与心脏移植的人病情都非常重，无论是否手术都会死，而不参与手术的人只要手术就能活。那么

    因果性：$P(Y^{a=1}=1)-P(Y^{a=0}=1)=\frac{10}{20}-\frac{20}{20}\neq 0$ 有因果性：病情不重的人接受手术后可以痊愈

    相关性：$P(Y=1|A=1)-P(Y=1|A=0)=\frac{10}{10}-\frac{10}{10}=0$ 没有相关性：看起来无论接不接受手术都会死。

    如果治疗的方案与样本的结果不独立，因果性$\neq$相关性

* 什么时候相关性能模拟因果性？随机实验（randomized experiments）

### 2. randomized experiments

* 随机实施治疗，因此治疗的结果与所受的治疗是独立的，$Y^a\perp\!\!\!\!\perp A, \,\forall a$. （exchangeability）

  $P(Y^a|A=a')=P(Y^a)$

* **conditional randomization**

  * 现实情况下，不得不根据样本的情况采取不同的治疗方案
    * 假设$L=1$代表病重，$L=0$代表轻症，$L$会对治疗的结果产生影响（effect modifier）
    * 在$L$相同的群体内随机采取治疗方案
    * $Y^a\perp\!\!\!\!\perp A|L, \,\forall a$
  * 两种计算方法：standardization / inverse probability weighting (IP weighting)

* **standardization **

  * $P(Y^a=1)=\sum_l P(Y^a=1|L=l)P(L=l)=\sum_l P(Y=1|A=a,L=l)P(L=l)$

* **inverse probability weighting**

  * 给每个样本一个权重$W^A=\frac{1}{f(A|L)}$

  * standardization与IP weighting 等价
    $$
    \begin{aligned}
    \mathbb{E}\left[\frac{I(A=a)}{f[a|L]}Y^a\right]
    &=\mathbb{E}\left\{\mathbb{E}\left[\frac{I(A=a)}{f(a|L)}Y^a\Big |L \right]\right\}\\
    &=\mathbb{E}\left\{\mathbb{E}\left[\frac{I(A=a)}{f(a|L)}\Big |L \right]\mathbb{E}\left[Y^a\Big |L \right]\right\}(\text{conditional exhangeability})\\
    &=\mathbb{E}\left\{\mathbb{E}\left[Y^a|L\right]\right\}\\
    &=\mathbb{E}[Y^a]=P(Y^a=1)
    \end{aligned}
    $$
    

  

* 现实情况下，我们只能让实验尽可能逼近conditional randomized

### 3. obsevational studies

* 研究者根本无法决定对象采取什么样的治疗方案
* 什么时候能够逼近conditional randomized？
  1.  (**consistency**) $Y=Y^a$. 
      * 明确定义$a$
      * 明确$a$是造成$Y$的原因，而不是其他偶然因素导致的$Y$
  2.  (**exchangeability**) 接受不同治疗方案的群体间只有$L$不同
  3. (**positivity**) 在不同$L$群体中，每种治疗方案都有人采用

### 4. effect modification

* 考虑不同群体的差异
* $V$ is a modifier of the effect $A$ on $Y$ if $\mathbb{E}[Y^{a=1}-Y^{a=0}|V=1]\neq\mathbb{E}[Y^{a=1}-Y^{a=0}|V=0]$.
* 为什么考虑？
  * 说明了治疗在不同群体中的差异，实验的结果不能简单外推（transportability），而是要考虑群体的特征
  * 找到从治疗中受益最大的群体
  * 帮助理解治疗起作用的机理
* **stratification** 按照effect modifier不同将群体分成多个，在不同群体中国分别计算causal effevct
* **matching**
  * 构造新的群体：对相同的$L$，构造$A=1$和$A=0$的pair (one-to-one or one-to-many)
  * 在新的群体中：受治疗的和未受治疗的人中$L$的分布相同

### 5. interaction

* 考虑多个治疗方案共同作用的结果
* There is interaction between two treatment $A$ and $E$ if the causal effect of $A$ on $Y$ with $E=0$ differs with $E=0$.

  * $P(Y^{a=1,e=1}=1)-P(Y^{a=0,e=1}=1)\neq P(Y^{a=1,e=0}=1)-P(Y^{a=0,e=0}=1)$

### 6. graphical representation of causal effects

* causal diagrams：用图表示因果关系

* d-separation

  * 查看图上两点是否具有相关性
  * 对于任意两点，如果他们之间存在一条开路，那么两个节点相关
  * 对于任意一条路，如果存在下表右侧三种情况，则是闭路
  
  | open path | closed path |
  | :---: | :---: |
  | <img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220325100741577.png" alt="image-20220325100741577" style="zoom: 33%;" /> | <img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220325100759127.png" alt="image-20220325100759127" style="zoom:33%;" /> |
  | <img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220325100839884.png" alt="image-20220325100839884" style="zoom: 33%;" /> | <img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220325100855128.png" alt="image-20220325100855128" style="zoom:33%;" /> |
  | <img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220325100947138.png" alt="image-20220325100947138" style="zoom:33%;" /> | <img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220325100930855.png" alt="image-20220325100930855" style="zoom:33%;" /> |
  
  如果两个节点间存在open path，那么两个节点是相关的，否则两者独立。

### 7. confounding

* common causes：比如说，患者的身体状况不仅影响治疗方案，也影响治疗的结果

  * 导致治疗方案和结果之间存在相关性，exchangeability不能满足

  * **backdoor path**

    ![image-20220324231643703](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220324231643703.png)

* 如何消除confounding？找到$L$，conditioning on $L$，消除相关性

* **backdoor criterion** A set of covariates $L$ satisfies the backdoor criterion if

  1. $A$ 和$Y$的backdoor path经过$L$

  2. $L$不是$A$的后代

  * 如果backdoor criteion满足，$Y^a\perp\!\!\!\perp A | L$.

* **Single-world intervention graphs** (**SWIG**)

  ![image-20220324232455900](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220324232455900.png)

* adjust for confounding
  * G-methods: standardization, IP weighting, g-estimation
  * stratification-based method: stratification, matching

### 8. selection bias

* conditioning on common effect:

![image-20220324232802016](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220324232802016.png)

### 9. measurement error

* 记录值和真实值有误差

  ![image-20220324234123964](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220324234123964.png)

  * $A$: true treatment
  * $A^\star$: measured treatment
  * $U_A$: measurement error

* Intention-to-treatment effect: 由于被测对象知道自己将要接受的treatment而对最终结果产生影响

* per-protocal effect: 所有对象都严格遵从treatment而得到最终结果

## Part II causal inference with models

### G-estimation

* goal: 预测$\mathbb{E}(Y^a)-\mathbb{E}(Y^{a=0})$
* standardization or IP weighting?
  * standardization: 需要对outcome model的形式进行假设，比如说$\mathbb{E}(Y|A,L)=\beta_0+\beta_1A+\beta_2L+\beta_3L^2+\beta_4AL$，然后$\mathbb{E}(Y^a)=\sum \mathbb{E}(Y|A,L)P(L)$
  * IP weighting：需要对treatment model的形式做假设，比如$\text{logit}(P(A|L))=\beta_0+\beta_1L+\beta_2L^2+\cdots$
  * 能不能不对confounder $L$单独的影响作假设？只对$A$和$L$之间的相关性假设：
* structural nested mean model: $\mathbb{E}(Y^a-Y^{a=0}|L)=\beta_0a+\beta_1aL\Longrightarrow Y^{a=0}=Y^a-\beta_0a-\beta_1aL$

* 另一方面，考虑 $\text{logit}P(A=1|Y^{a=0},L)= \alpha_0+\alpha_1Y^{a=0}+\alpha_2L$
  * 当$L$已知时，由$Y^a\perp\!\!\!\perp A|L$可知$\alpha_1=0$
* G-estimation: 对于可能的$(\beta_0, \beta_1)$
  1. 计算$Y^{a=0}=Y^a-\beta_0a-\beta_1aL$得到$Y^{a=0}$
  2. 代入logistic 模型，拟合数据，得到$\hat{\alpha_1}$，$\hat{\alpha}_1=0$的参数即为所求

### Doubly robust estimator

只要outcome model和treatment model 其中之一正确，doubly robust estimator 就是unbiased

1. estimate $W^A=\frac{1}{f(A|L)}$
2. fit $\mathbb{E}(Y|A=a,L=l,R)$, where $R=W^A$ if $A=1$, $R=-W^A$ if $A=0$.
3. standardize conditioning on $L$, then use $\mathbb{E}(Y|A=1,R)-\mathbb{E}(Y|A=0,R)\to \mathbb{E}(Y^a)-\mathbb{E}(Y^{a=0})$ 

### instrumental variable estimation

* instrument $Z$

  * $Z$ is associated with $A$

  * $Z$ does not affect $Y$

  * $Z$ and $Y$ do not share causes

    ![image-20220331213002887](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220331213002887.png)

* **usual IV estimand**用$\frac{\mathbb{E}(Y|Z=1)-\mathbb{E}(Y|Z=0)}{\mathbb{E}(A|Z=1)-\mathbb{E}(A|Z=0)}$来预测$\mathbb{E}(Y^a)-\mathbb{E}(Y^{a=0})$
* two-stage-least-square estimator
  * fit $\mathbb{E}(A|Z)=\alpha_0+\alpha_1Z$
  * fit $\mathbb{E}(Y|Z)=\beta_0+\beta_1 \hat{E}(A|Z)$, the $\hat{\beta_1}$ will be equal to standard IV estimator

## Part III Causal inference for longitudinal data

### treatment-confounder feedback

![image-20220331214028555](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20220331214028555.png)

* confouder $L_0$ affect treatment $A_0$, $A_0$ affect $L_1$.
* $U_1$ is unmeasured confounder
* 此时，即使condition on confounder，也无法消除相关性
* 需要对之前的方法进行修正：
  * g-formula: $\sum_{l_1}\mathbb{E}(Y|A_k=a_k,\bar{A}_{k-1}=(a_0,a_1,\cdots,a_{k-1}),L_k=l_k)f\Big(l_k\Big|\bar{a}_{k-1}=(a_0,\cdots, a_{k-1}),\bar{l}_{k-1}=(l_0,\cdots,l_{k-1})\Big)$
  * IP weighting: weight $\prod_{k=0}^k\frac{1}{f(A_k|\bar{A}_{k-1},\bar{L}_k)}$.
  * doubly robust: 
    1. fit for $P(A_k=a_k|\bar{A}_{k-1},\bar{L}_k)$, estimate $W^{\bar{A}_k}=\prod\frac{1}{f(A_k=a_k|\bar{A}_{k-1},\bar{L}_k)}$
    2. fit outcone regression at each time k, starting from last time
    3. standardization

## Conclusion

* Goal : $\mathbb{E}(Y^a)-\mathbb{E}(Y^{a=0})$
* problems: how to achieve exchangeablity?
  * confounding, selection bias (censor), measurement error, random variablity
* solution:
  * g-methods
    * standardization (g-formula)
    * IP weighting
    * g-estimation
  * stratification-based methods
    * stratification
    * matching
* special topic: extension for time-varying treatment
