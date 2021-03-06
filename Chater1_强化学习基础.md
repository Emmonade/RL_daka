# 强化学习基础+MDP模型

<u></u>强化学习是除了监督学习和非监督学习之外的第三种基本的机器学习方法。
灵感来源于行为主义，即如何在“惩罚和奖励”的刺激下，逐渐形成对刺激的预期，养成使利益最大化的行为习惯。
大部分研究集中于最优解的存在和特性。 经济学中用来寻找多种限制条件下的平衡

机器学习问题里，环境被抽象为MDP，（markov decision process）,该假设下才能使用动态规划方法

在运筹学里被称为 近似动态规划或神经动态规划。

通常被建模为马尔可夫决策过程：


|  |  |
| --- | --- |
| 环境状态集合 |  S|
| 动作集合 | A | 
| 状态之间转移的概率，转移矩阵 | P |
| 规定转换后“即时奖励”的规则,奖励函数 | R |
|描述主体能够观察到什么的规则。| |





数学化的语言描述这个过程就是，时间是离散的，每个时间t,主体都会收到一个观测$o_t$，包含奖励$r_t$,然后从允许的动作集合中选择一个动作$a_t$. 然后环境状态变为$s_{t+1}$,之前的动作被记录为$(s_t,a_t,s_{t+1})$,相关联的奖励为$r_{t+1}$. 

另外一种更为清晰的解释是，agent和environment一直在交互，agent从环境里取得状态，然后输出动作action, action放入环境后，环境会据此输出下一个状态和对action的奖励。agent的目的在于最大化奖励。

强化学习的目标就是最大化收益。

最有意思的地方在于，该主体的表现会始终与以最优行动的主体表现进行比较，行动的差异则为“悔过”的概念。想接近最优方案，则必须根据长时间行动序列来推理。举例：想最大化收入，最好现在去上学，尽管短期内货币收入为负。

因此强化学习对于长期反馈的问题比短期反馈更好。

连电梯调度都有用到强化学习？？主要用在机器人控制

强化学习的强大来源于两个方面：
1、样本用来优化行为
2、函数用来描述复杂环境

可适用的复杂问题：
1、模型环境已知，解析解不存在
2、仅有环境模型（星网的问题）
3、从环境中获取信息的唯一方法是和环境互动
前两个问题是规划问题，后一个genuine学习的问题

常用算法
蒙特卡洛学习 Monte-Carlo Learning
Temporal-Difference Learning
SARSA
Q-Learning

#### 区分监督学习和强化学习
监督学习有两个假设
1、输入数据是没有关联的(iid)，不然网络无法学习
2、有正确答案的反馈，通过正确答案修正预测

强化学习不满足上述两种条件
1、输入数据可能是有强关联的时间序列数据，并不满足iid
2、大多数情况下并没有及时反馈（天呐好像人生）。
涉及到的terminology:delayed reward(延迟奖励)。比如游戏不玩完，根本不知道输赢

总结下二者的区别：

| 强化学习 |监督学习  |
| --- | --- |
| 输入为序列数据 | 输入是iid的，彼此之间并无关联 |
| 无正确行为的指导，只能靠自己探索发现，不断试错。探索（exploration）和利用(exploitation),在当中取一个权衡。一旦探索发现该行为能获得奖励，就一直利用该行为。 | 有标签直接反馈行为是否正确 |
| 无强监督，只有一个会延迟的奖励信号 | 随时有指引 |

#### 因此强化学习的特点
- 试错探索（trial-and-error exploration）
- agent从环境里获得延迟奖励
- 数据具有时间关联(sequential data)
- agent的表现会影响之后的数据，因此让模型稳定提升非常重要
- 强化学习里一个很重要的课题就在于近期奖励和远期奖励的一个权衡（trade-off）。

#### 强化学习的优势
可超越人的表现。监督学习的标签都是人工标注。者决定了模型能力的上限就是人类的水平。而强化学习是被放在环境里自己去探索。潜力是无穷的。如deepmind所发明的AlphaGO

股票收益其实也是一个强化学习的过程（学会我就去炒股实现财富自由了

轨迹(trajectory)，描述环境和动作的策略的时间序列
$$\tau=(s_0,a_0,s_1,a_1...)$$


#### 深度强化学习
深度学习+强化学习 = 深度强化学习
即神经网络放入强化学习
- standard RL : 设计特征 -> 训练值函数
设计手工特征，然后设计一个分类网络或者训练一个价值估计函数进行分类
-Deep RL: 省去特征工程的过程，变成end-to-end的训练，输入状态然后直接输出动作

#### 算力对于强化学习的重要性
可以快速增加各种试错次数
通过各种尝试获取信息，然后增加获得奖励的机率
将特征工程和价值提取函数一起优化，就可以得到很强的决策网络

#### 真实操作实践
在虚拟环境中模拟得到一个表现优良的agent,然后再将其放进机器人中。


#### 专业名词的解释：
rollout:从当前帧开始去生成很多局游戏

#### 专业名词创造
事件拓扑：
![ccf17cc9168145484d931a95b91e6b47.png](evernotecid://83F18317-A830-4711-99EB-A524290AACDF/appyinxiangcom/22419927/ENResource/p277)


### 序列决策过程（Sequential Decision making）
先定义奖励：标量反馈信号（scalar feedback signal）
强化学习的目的是最大化奖励（expected cumulative reward）

奖励本身的稀疏程度决定了这个游戏的难度。

历史：观测、奖励、行为的序列
$$H_t=O_1,R_1,A_1,...,A_{t-1},O_t,R_t$$
（是否暗含当前时刻，是知道奖励的）

可把状态看作历史的函数
$$S_t=f(H_t)$$

#### 状态和观测的关系
S state 关于世界的全部信息
O observe 关于世界的部分信息
通常用实值向量、矩阵或者更高阶的张量表示

环境更新状态的函数
$$S_t^e=f^e(H_t)$$
agent内部更新状态的函数
$$S_t^a=f^a(H_t)$$
如果二者等价，则该环境被称为是完全可观测的(fully observable)
该请情形下，强化学习会被建模为MDP问题（Markov Decision Process）
此时有
$$O_t=S_t^e=S_t^a$$

如果agent只能看到部分环境信息，则称该环境是部分可观测的(partially observable).此时的强化学习会被建模为POMDP问题（比如自动驾驶）
<u>部分可观测马尔可夫决策过程(Partially Observable Markov Decision Processes, POMDP)</u>，是MDP的一种泛化。

POMDP可用七元组来描述
$$(S,A,T,R,\Omega,O,\gamma)$$
$\gamma$是折扣系数， $T(s'|s,a)$状态转移概率
$\Omega(o|s,a)$观测概率，$O$观测空间

#### Action Space
- 离散动作空间（Go）
- 连续动作空间（$360^0$转）

#### a RL agent的主要成分
以下由一个或者多个组成
- 策略函数(policy function):agent根据该函数选择下一步策略
- 价值函数（value function）: 进入该状态后，估算未来的收益。该函数值越大，表明进入该状态越有利
- 模型(model)：？？表示该agent对这个世界的理解。实际定义的是状态转移概率和奖励，决定下一个状态是怎样

##### policy
分为两种，一种是随机性策略(stochastic policy),一种是确定性策略(deterministic policy)
随机性策略：根据行为的概率分布进行采样输出行为。$\pi(a|s)$
确定性策略：输出一个确定性动作，如$a*=argmax_a(\pi(a|s))$
随机性策略能更加灵活地探索环境，确定性策略在跟智能体博弈的时候容易被预测。

##### Value
量化评估未来的收益，对状态的好坏进行判断
这里量化的价值函数用到了金数里面的时间折现概念。折现因子$\gamma$，即未来t+1时刻后的所有奖励值折线到$t+1$时刻
另外其实价值函数就是一个期望的概念:知道当前状态后的未来收益
$$v_{\pi}(s)=E_{\pi}(G_t|S_t=s)=E_{\pi}(\sum_{k=0}\gamma^{k}R_{t+k+1}|S_t=s)$$

价值函数除了上述外，还有一个Q函数。Q函数包含两个变量：状态和动作
$$q_{\pi}(s,a)=E_{\pi}(G_t|A_t=a,S_t=s)=E_{\pi}(\sum_{k=0}\gamma^{k}R_{t+k+1}|S_t=s,A_t=a)$$
背后的思想是，你未来所收到的奖励的期望，取决于你当前的状态和当前的行为。

##### model
模型决定了下一步状态，而下一步状态取决于你当前的状态和行为
概率转移定义
$$P_{ss'}^a=P(S_{t+1}=s'|S_t=s,A_t=a)$$
奖励定义（期望）
$$R_s^a=E(R_{t+1}|S_t=s,A_t=a)$$

以上三步到齐形成MDP

##### Types of RL Agents
value-based agent
显式学习价值函数，隐式学习策略

policy-based agent
直接学习策略，给到状态，它输出动作概率

Actor-Critic agent
这一类agent会把价值函数和策略函数同时学习，然后交互得到最佳行为

##### 基于策略迭代和价值迭代的区别
加入概率转移矩阵已知，那就是一个动态规划算法的问题。
决策方式：在动作集里选择动作的依据，静态，不随状态变化。


|  |  |  |
| --- | --- | --- |
| 基于策略迭代 | 指定动作策略，即给定状态采取相应动作。算法直接优化策略从而获取最大收益 |策略梯度算法｜
| 基于价值迭代 |维护优化价值函数，每次根据价值函数的最大值选择动作。适用于离散环境，如围棋游戏等，连续性动作不适用（机器人动作）|Sarsa, Q-learning｜
| Actor-Critic | 根据策略出动作，然后价值函数再给出动作价值，同时优化，加快学习速度 |  
###### 根据是否有模型分类

|  |  |
| --- | --- |
| model-based | 根据状态转移来做决策 |
| model-free | 没有状态转移模型，纯粹根据策略和价值做决策 |

MDP建模,若
$$<S,A,P,R>$$
已知，则可模拟环境交互，生成真实的决策过程
在虚拟环境学习就是有模型学习。

然而在真实环境里，状态转移模型和奖励模型事实上较大概率是难以区分的，这种情形下要用免模型学习。


#### planing 与 learning 
planing是最基本的问题。环境模型若已知，则不需要和环境做过多交互，直接建模学习最优策略。
所以一个基本的切入点是先尝试对环境进行建模

##### learning
learning有两个基本操作：exploration , explitation
前者试错，后者利用已知可以获得最大奖励的操作。（只能通过试错了解行为后果是强化学习的特点）
关键点在于权衡，牺牲多少短期奖励，去学习行为后果。
注：动作奖赏可能是确定值，大多数情况下更可能是概率分布




code：https://github.com/cuhkrlcourse/RLexample
深度学习的包：pytorch , Tensorflow,Keras
所以不需要从头造轮子

|  |  |
| --- | --- |
| 离散控制环境 | Atari  |
| 连续控制环境 | mujoco  |




on-policy 
off-policy l

Monte Carlo
TD Temporal-Difference Learning, 
TD target
TD error

$S_T$ -> $R_{T+1}$ -> $S_{T+1}$
what's V(S)

TD can learn it online after every step
TD can learn before MD

On-Policy Temporal-Differnece Control

GYM测试问题环境的集合 library
has a shared interface


# 习题
## keywords
**深度强化学习**：不需要额外做特征工程，通过神经网络来拟合价值函数或者Policy network

**fully observed（部分可观测）**: agent可以观测到环境的全部面貌

**partially observed（完全可观测）**：Agent不能观察到Environment的所有状态

**基于策略的**：agent直接制定一套策略，即何种状态下采用对应动作的概率，强化学习直接对动作进行优化，使之获得累计奖励最大化

**基于价值的**：不需要给出一个显示策略，而是维护一个价值表格或者价值函数，对应状态下，给出价值最大的动作即可

**有模型**：agent 学习或者已知 环境的状态转移函数
**无模型**：未知环境模型，主要是状态转移概率等等，此时agent主要通过学习policy function or value function。

## Questions
**强化学习的基本特征**
数据是时间序列数据，具有强关联性。而普通监督学习的样本数据是iid的
**状态和观测用什么表示** 
实值向量、矩阵、更高阶的张量
**基于价值迭代的强化学习算法有哪些**
Q-learning、Sarse
**基于策略迭代的强化学习算法有哪些**
策略梯度算法

**有模型的学习和无模型的学习有什么区别？**
有模型的学习需要先对环境进行建模，学习状态转移概率函数等；无模型学习直接通过与环境交互进行学习，算法的泛化性更好。但需要更大量的采样工作和数据量。

## interview
my own answer:
**一句话概括强化学习**
强化学习由环境状态、动作、奖励三部分组成。agent通过与环境进行交互，做出序列决策，强化学习的目标是最大化序列动作的累计收益。


