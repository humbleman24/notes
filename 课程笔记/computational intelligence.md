## Probability theory

这里主要是记录一点名词，没必要重复概念了

stochastic (random) system, random variables (discrete, continuous), cumulative distribution function (discrete, continuous), expectation, variance and standard deviation

common probability distribution:  uniform distribution, Gaussian distribution

joint probability distributions: x and y are independent variables only $f_{XY}(x,y) = f_X(x)f_Y(y)$

conditional probability: 只需要掌握贝叶斯公式

Covariance: $Cov(X,Y)$ is a measure of joint variability of two random variables X and Y:

$Cov(X,Y) = E[(X - E[X])(Y-E[Y])]$  注意这里中间是相乘的 $E[XY] - E[X]E[Y]$

#### Bayesian inference

probability of a hypothesis is updated using Bayes' theorem

assuming observation x follow a hypothetical distribution $p(x|\theta)$ , where $\theta$ is the parameter of the distribution, we can relate the distribution as: $p(\theta|x) = \frac{p(x|\theta)p(\theta)}{p(x)}$

we may be able to guess or estimate the distribution $\theta - p(\theta|\alpha)$ , where $\alpha$ is a hyperparameter

主要就是对Naive Bayesian的更广泛的定义

## Gaussian Processes

**Formal definition:**

A stochastic process {f(x)} such that for any finite set {x1, .... xn}, the vector $f = [f(x_1),...f(x_n)]$ follows a multivariate normal distribution

**Gaussian Process Regression:**

目标函数是，$p(f_* | X, y, x_*)$

**联合分布**和**条件分布**，以及**后验均值**和**后验方差**的计算方法。

高斯过程中，我们关心的是输出y和在新输入点x出的函数值f之间的联合分布

基于联合分布，我们可以得到在给定观测数据y的情况下，f的条件分布

## Hidden Markov Models

transaction matrix

frequency of state as $N_t(y) = \sum^t_{s=0} Pr(X_s=y)$

Occupancy matrix: 不知道有什么用，但是就是在有initial state的情况下计算可能的y的frequency of state

limiting distribution and equilibrium distribution 极限分布和平稳分布

定义是：$\sum_{x\in S}\pi(x)P(x,y) = \pi(y) \forall y \in S $ 

稳态分布是唯一的，所以可以通过一个方程组来计算出对应的平稳

**hidden markov models**: 进入正题

the states X of the underlying Markov model cannot be observed directly, but only through another "depending" process.

The observable states Y depend on the states of the HMM at certain probabilities.

$Pr(y) = \sum_{x\in A} Pr(y|x)Pr(x)$

隐马尔可夫的假设：

- 给定当前状态，未来的状态与夺取的状态无关
- 给定当前的隐状态，当前的观测值与之前的观测值无关

**Definitions for HMM**

State space of n states for Markov model S;  Transition probabilities (matrix) A;  set of m observed symbols V;  Observed symbol probability matrix B;   Initial state distribution $\pi_i$

problem: compute the probability of observed sequence $O = \{O_0, ... , O_T\}$, given the parameters $\lambda = \{A, B, n, m, \pi\}$ are known.

solution: decompose the problem by summing the probabilities for all the possible hidden state sequences $Q = \{q_0, ... , q_t\}$ :     $Pr(O|\lambda) = \sum_{all Q} Pr(O|Q, \lambda) Pr(Q|\lambda)$ 

<img src="../pic/HMM_observation_calculation.png" style="zoom:50%;" />

we can solve the probability for O (observation) by using forward algorithm

the probability of seeing the sequence of observation O and ending in state $s_i$ is :

$\alpha_t(i) = Pr(O = \{o_0,o_1,...,o_t\}, q_t=s_i|\lambda)$ 

- initialization: $\alpha_0(i) = \pi_i$
- induction: $\alpha_{t+1}(j) = [\sum^N_{i=1}\alpha_t(i)a_{i,j}]b_{j, o_{t+1}}$
- termination: $Pr(O|\lambda) = \sum^N_{i=1}\alpha_T(i)$  











































## Swarm intelligence

swarm intelligence is based on the idea that autonomous agents can solve complex tasks together by following simple rules.

it is decentralized: agent decide their actions independently based on their interaction with the enviornment (no predefined leader)

**Basic principles:**

- the swarm can solve complex problems that a single individual with simple abilities could not solve
- the performance of the swarm is not affected by loss of one individual or a mistake by one individual
- individuals have local sensory information and thy can perform simple actions, but they have little or no memory, and they do not know about the status of the swarm as a whole or its goal

### Particle swarm optimization (PSO)

goal: find the area of the highest concentration of insects

background settings: the birds can signalize their position and the food concentration there to their neighbors; birds can remember their past position of the best food concentration.

##### programming 

**Each particle $i$** is described by its current position $x_i$, best known position $p_i$, velocity $v_i$, and cost function values  $f(x_i)$ and $f(p_i)$. 

**Goal**: we want to find the position that minimizes the cost function 

**Initial settings:** positions and velocities of the particles are initialized randomly, initialize  $p_i$ is set to $x_i$; the swarms best known position $g$ is set to the $x_i$ that minimizes $f(g)$.

