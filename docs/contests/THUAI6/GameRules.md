---
title: 游戏规则
sidebar_label: 游戏规则
---

V6.0
- [规则](#规则)
  - [简则](#简则)
    - [地图](#地图)
    - [人物](#人物)
    - [攻击](#攻击)
    - [交互](#交互)
      - [学习与毕业](#学习与毕业)
      - [勉励](#勉励)
      - [沉迷与唤醒](#沉迷与唤醒)
      - [门](#门)
      - [窗](#窗)
      - [箱子](#箱子)
    - [信息相关](#信息相关)
    - [可视范围](#可视范围)
    - [道具](#道具)
  - [得分](#得分)
    - [捣蛋鬼](#捣蛋鬼)
    - [学生](#学生)
  - [职业与技能](#职业与技能)
    - [捣蛋鬼](#捣蛋鬼-1)
      - [Assassin](#assassin)
      - [Klee](#klee)
      - [喧哗者ANoisyPerson](#喧哗者anoisyperson)
      - [Idol](#idol)
    - [学生（\&老师）](#学生老师)
      - [运动员](#运动员)
      - [教师](#教师)
      - [学霸](#学霸)
      - [机器人Robot](#机器人robot)
      - [技术宅TechOtaku](#技术宅techotaku)
      - [开心果](#开心果)
  - [细则](#细则)
    - [特殊说明](#特殊说明)
    - [移动](#移动)
    - [人物](#人物-1)
    - [初始状态](#初始状态)
    - [道具](#道具-1)
    - [交互](#交互-1)
    - [学习与毕业](#学习与毕业-1)
    - [攻击](#攻击-1)
    - [沉迷与唤醒](#沉迷与唤醒-1)
    - [门](#门-1)
    - [窗](#窗-1)
    - [箱子](#箱子-1)
    - [得分](#得分-1)
    - [信息相关](#信息相关-1)
    - [技能](#技能)
    - [职业](#职业)

## 简则

- 每场比赛分为上下两个半场，上下半场双方换边
- 单局比赛分为学生阵营（4 人）和捣蛋鬼阵营（1 人）
- 最终将两场比赛己方所得分数相加，高者获胜

### 地图
- 地图为矩形区域，游戏对象坐标为（x, y），x和y均为整数。
- **x坐标轴正方向竖直向下，y坐标轴正方向水平向右**；
- **极坐标以x坐标轴为极轴，角度逆时针为正方向**。
- 地图由50 * 50个格子构成，每个格子代表1000 * 1000的正方形。每个格子的编号(CellX,CellY)可以计算得到：

$$
CellX=\frac{x}{1000},CellY=\frac{y}{1000}
$$

- 格子有对应区域类型：陆地、墙、草地、教室、校门、隐藏校门、门、窗、箱子
- 任何格子的区域类型(PlaceType)始终不变,所有隐藏校门刷新点的区域类型均为隐藏校门

### 人物
- 人物直径为800
- 人物共有17种不可叠加的状态：
1. （可）移动状态 Idel
2. 学习 Learning
3. 被勉励 Encouraged
4. 在勉励 Encouraging
5. 开或锁门 Locking
6. 翻箱 Rummaging
7. 使用技能 UsingSpecialSkill
8. 开启校门 OpeningAGate
9. 唤醒他人中 Rousing
- 之后八项为不可接受指令状态
1.  被唤醒中 Roused
2.  沉迷 Addicted
3.  退学 Quit
4.  毕业 Graduated
5.  被眩晕 Stunned
6.  前摇 Attacking
7.  后摇 Swinging
8.  翻窗 Climbing

### 攻击
- 攻击距离是指攻击（子弹）的移动距离，也就是说理论上最远被攻击的学生的中心与捣蛋鬼的中心=学生的半径+捣蛋鬼的半径+攻击距离+子弹半径（200）*2
- 蹦蹦炸弹BombBomb与strike攻击未写完的作业，会造成对应攻击力的损坏
- 捣蛋鬼攻击交互状态或前后摇的学生，将使学生眩晕4.3s
- 蹦蹦炸弹BombBomb可以攻击使门被打开

|   攻击（子弹）类型 |搞蛋鬼的一般攻击CommonAttackOfTricker|飞刀FlyingKnife    |      蹦蹦炸弹BombBomb   | 小炸弹JumpyDumpty       |        strike          |
| :--------------------- |  :---------------------|  :--------------------- | :--------------------- | :--------------------- | :--------------------- |
|   子弹爆炸范围          |     0                  |        0                |   2000                 |    1000                |         0              |
|   子弹攻击距离          |     2200               |       78000             |   2200                 |    1800                |        2000            |
|   攻击力                |     1500000            |        1200000         |   1800000               |    900000             |        1600000         |
|   移动速度/s            |     7400               |              18500     |   6000                  |   8600                 |        6250            |
|   前摇（ms）            |     297                |      600               |   366                   |      -                 |        320             |
|未攻击至目标时的后摇（ms）|     800                |      0                  |     1200               |    -                    |        800             |
|攻击至目标时的后摇（ms）  |    3700                |     0                   |         3700           |      -                  |        3700            |
|   CD(ms)               |      800               |      600               |    3000                 |    -                    |        800             |
|   最大子弹容量          |      1                 |     1                  |    1                    |   -                     |         1              |  

### 交互
- 除了翻窗，交互目标与交互者在一个**九宫格**方可交互
- 交互进度每毫秒增加对应交互速度的值
- 作业，门，箱子完成/开启进度达到10000000为完成
- 校门开启进度达到18000为完成


#### 学习与毕业
- 共有10间教室，学生需要完成**7间**教室的作业，才可以开启校门。
- 开启校门需要18秒，开启进度不清空
- **3间**教室的作业完成时，隐藏校门会在刷新点之一随机显现；当只剩1名学生时，隐藏校门自动打开。
- 从开启的校门或隐藏校门毕业是学生终极目标

#### 勉励
- 当被勉励程度达到当前损失的毅力值或1500000时，勉励完成，学生毅力增加对应被勉励程度。
- 勉励中断时，被勉励程度保留；遭到攻击时被勉励程度清空

#### 沉迷与唤醒
- 学习毅力归零时，学生进入沉迷状态，每毫秒增加1沉迷度
- 唤醒需要1秒，之后学习毅力恢复至1/2。沉迷程度不清空。
- 进入沉迷状态时，如果学生原沉迷程度在（0，该玩家最大沉迷度/3）中，沉迷程度直接变为其最大沉迷度/3；原沉迷程度在[其最大沉迷度/3，其最大沉迷度x2/3）中，沉迷程度直接变为其最大沉迷度x2/3；原沉迷程度大于其最大沉迷度x2/3，从游戏中出局；
  - 当学生沉迷程度达到其最大沉迷程度时，从游戏中出局

#### 门
- 门分别属于三个教学区：三教，五教，六教
- 拥有对应教学区的钥匙才能开锁对应的门
  - 锁门过程中，门所在格子内有人会使锁门过程中断

#### 窗
- 翻窗时玩家应当在窗前后一个格子内

#### 箱子
- 开箱后将有2个随机道具掉落在玩家位置。

### 信息相关
- Bgm （在structures.h/.py中的student类或Tricker类中作为其属性）
  1. 不详的感觉(dangerAlert)：如果捣蛋鬼进入（学生的警戒半径/捣蛋鬼的隐蔽度）的距离，则学生的dangerAlert=（警戒半径/二者距离）
  2. 期待搞事的感觉(trickDesire)：如果有学生进入（捣蛋鬼的警戒半径/学生的隐蔽度）的距离，则捣蛋鬼trickDesire=（警戒半径/可被发觉的最近的学生距离）
  3. 学习的声音(classVolume): 警戒半径内有人学习时，捣蛋鬼classVolume=（（警戒半径x学习进度百分比）/最近二者距离）
- 可以向每一个队友发送不超过256字节的信息

### 可视范围
  - 小于视野半径
  - 对于中心在草地中的物体，物体中心与玩家中心连线上均为草地方可见
  - 不在草地的物体，物体中心与玩家中心连线上无墙即可见

### 道具
- 玩家同时最多拥有三个道具
- 可以捡起与自己处于同一个格子（cell）的道具
- 可将道具扔在玩家位置
  
|   道具       |           对学生增益            |       学生得分条件               |                对搞蛋鬼增益        |       搞蛋鬼得分条件               |
| :-------- | :-------------------------------------- | :-----------------| :-------------------------------------- |:-----------------|
|   Key3   |                            能开启3教的门                            |不得分|      能开启3教的门                            |不得分|
|   Key5   |                             能开启5教的门                           |不得分|      能开启5教的门                            |不得分|
|   Key6   |                             能开启6教的门                            |不得分|      能开启6教的门                            |不得分|
|   AddSpeed   |  2倍速，持续10s                |  得分10|  2倍速，持续10s                            |得分10|
|   AddLifeOrClairaudience   |若在10s内Hp归零，该增益消失以使Hp保留100|在10s内Hp归零，得分100      |10秒内得知全场玩家信息|得分10 |
|   AddHpOrAp   |回血750000   | 回血成功，得分10 |   10秒内下一次攻击增伤1800000|不得分 |
|   ShieldOrSpear   | 10秒内能抵挡一次伤害  |   10秒内成功抵挡一次伤害，得分50       |10秒内下一次攻击能破盾，如果对方无盾，则增伤900000|   10秒内破盾，得分50 |
|   RecoveryFromDizziness   | 使用瞬间从眩晕状态中恢复  |    成功从眩晕状态中恢复，得分30|使用瞬间从眩晕状态中恢复  |    成功从眩晕状态中恢复，得分30|

## 得分

### 捣蛋鬼

- 对学生造成伤害时，得伤害*100/基本伤害（1500000）分。
- 对作业造成破坏时，每破坏n%的作业，得n分
- 使学生沉迷时，得50分。
- 使学生眩晕时，得20*眩晕时长（/s）分。
- 每淘汰一个学生，得1000分
- 摧毁一个TechOtaku的机器人，得50分。

### 学生
- 学生每完成n%的作业，得2n分
- 学生与捣蛋鬼处于小于5000的距离认为在牵制状态，得（牵制时长（ms）*0.00246）分
- 使捣蛋鬼进入眩晕状态时，得20*眩晕时长（/s）分。
- 成功唤醒一名同学，得100分
- 成功勉励一名同学，得50*勉励程度/基本勉励程度（1500000）分
- 毕业，得1000分

## 职业与技能

### 捣蛋鬼

|   捣蛋鬼职业   |        Assassin         |        Klee             |      喧哗者ANoisyPerson |    Idol                |
| :------------ |  :--------------------- |  :--------------------- | :--------------------- | :--------------------- |
|   移动速度/s   |     3960                |      3600               |   3852                 |       3600             |
|   隐蔽度       |      1.5                |      1                  |     0.8                |         0.75           |
|   警戒范围     |     22100               |      17000              |  15300                 |        17000           |
|   视野范围     |      15600              |      13000              |    13000               |       14300            |
|   开锁门速度/ms|     5000                |    5000                 |      5000              |         5000           |
|   翻窗速度/s   |   2540                  |       2540              |     2794               |         2540           |  
|   翻箱速度/ms  |          1250           |   1375                  |     1250               |         1250           |

#### Assassin
- 普通攻击为 CommonAttackOfTricker
- 主动技能
  0. 隐身 BecomeInvisible
    - CD：40s 持续时间：10s
    - 在持续时间内玩家隐身，人物状态进入UsingSpecialSkill，进入其他状态会使得隐身状态解除
    - 使用瞬间得分15
  1. 使用飞刀
    - CD：30s 持续时间：1s
    - 在持续时间内，攻击类型变为飞刀
    - 不直接得分

#### Klee
- 普通攻击为 CommonAttackOfTricker
- 主动技能
  0. 蹦蹦炸弹 JumpyBomb
    - CD：15s 持续时间：3s
    - 在持续时间内，攻击类型变为蹦蹦炸弹
    - 当蹦蹦炸弹因为碰撞而爆炸，向子弹方向上加上90°，120°，150°，180°，210°，240°，270° 发出7个小炸弹
    - 小炸弹运动停止前会因为碰撞爆炸，停止运动后学生碰撞会造成眩晕3.07s
    - 不直接得分，通过眩晕等获得对应得分
  1. SparksNSplash：
    - CD：45s, 持续时间：10s
    - 技能使用瞬间，对于输入的额外参数PlayerID代表的角色，距离最近的本已停止运动的小炸弹开始追踪该角色（每100ms向该角色直线移动）（该角色无血量则失败）
- 特性
  - 开局获得随机的一个道具（不会是钥匙）

#### 喧哗者ANoisyPerson
- 普通攻击为 strike
- 主动技能
  0. 嚎叫 Howl
    - CD：25s 
    - 使用瞬间，在视野半径范围内（不是可视区域）的学生被眩晕5500ms，自己进入800ms的后摇
    - 通过眩晕获得对应得分
- 特性
  - 在场所有学生Bgm系统被设为无用的值

#### Idol
- 普通攻击为 CommonAttackOfTricker
- 主动技能
 0. ShowTime
    - CD: 80s 持续时间：10s
    - 持续时间内
      - 使警戒范围外的学生眩晕并每500ms发送向自己移动500ms的指令（速度为学生本应速度*二者距离/警戒范围）
      - 对于视野范围（不是可视区域）内的学生每500ms加1500的沉迷度
      - 捣蛋鬼变为0.8倍速"

### 学生（&老师）

|   学生职业     |        教师Teacher   |        健身狂Athlete  |学霸StraightAStudent |   开心果Sunshine    |   机器人Robot        |      技术宅TechOtaku |
| :------------ |  :------------------ |  :------------------ | :------------------ | :------------------ | :------------------ | :------------------ |
|   移动速度/s   |    2700              |      3150            |   2880              |    3000             |    2700             |        2880         |
|   最大毅力值   |     30000000         |      3000000         |   3300000           |    3200000          |    900000           |    2700000          |
|   最大沉迷度   |     600000           |        54000         |   78000             |    60000            |        0            |     60000           |
|   学习速度/ms  |        50            |      73              |   135               |     123             |     85             |      110            |
|   勉励速度/ms  |     80               |      90              |   100               |      120            |      0              |    100              |
|   隐蔽度       |      0.5             |      0.9             |     0.9             | 0.8                 |        0.8          |    1.1              |
|   警戒范围     |     10000            |      15000           |   13500             |    15000            |        0            |    15000            |
|   视野范围     |      9000            |      11000           |    9000             |    10000            |        0            |    9000             |
|   开锁门速度/ms |    5000             |    5000              |     5000            |         3500        |        0            |     5000            |
|   翻窗速度/ms   |      1000           |       1466           |     1018            |           1222      |        1            |    1100             |
|   翻箱速度/ms   |            1250     |   1250               |     1250            |     1125            |        1000         |    1100             |

#### 运动员
- 主动技能 
  0. 冲撞 CanBeginToCharge
    - CD：60s 持续时间：3s
    - 在持续时间内，速度变为三倍，期间撞到捣蛋鬼，会导致捣蛋鬼眩晕7.22s,学生眩晕2.09s
    - 通过眩晕获得对应得分

#### 教师
- 主动技能
  0. 惩罚 Punish
    - CD：45s 
    - 使用瞬间，在视野距离/3范围内（不是可视范围）的翻窗、开锁门、攻击前后摇、使用技能期间的捣蛋鬼会被眩晕
  
    $$
    (3070+\frac{500×已受伤害}{基本伤害（1500000）×2^{开局老师数量-1}})ms
    $$

    - 其眩晕得分为正常眩晕得分/2^开局老师数量-1^
  1. 喝茶 HaveTea
    - CD：90s
    - 使用瞬间，向额外参数/1000.0的角度瞬移3000（可以穿墙），如果会碰撞则失败。
- 特性
  - 无法获得牵制得分
  - 无法毕业
  - 扣血则得分$\frac{100×受到伤害}{基本伤害（1500000）×2^{开局老师数量-1}}$

#### 学霸
- 特性
  - 冥想
    - 当处于可接受指令状态且不在学习时，会积累“冥想进度”，速度为40/ms
    - 受到攻击（并非伤害）、进入学习状态或进入不可接受指令状态（包括翻窗）冥想进度清零
- 主动技能
  0. 写答案 WriteAnswers
    - CD：30s
    - 使用瞬间，对于可交互范围内的一间教室的作业增加冥想进度，冥想进度清零
    - 通过学习获得对应得分

#### 机器人Robot
- 特性
  - 无技能
  - 不可被眩晕
  - 不可毕业
  - 不可救人
  - 无牵制得分
  - 不可使用道具（可以捡起和扔道具）
  - 不沉迷，击中直接摧毁

#### 技术宅TechOtaku
- 特性
  - 一名TechOtaku最多可以在场上同时最多拥有3个Robot，无法共享视野
- 主动技能
  0. SummonGolem
    - CD：40s，持续时间：6s
    - 在持续时间中，学生进入人物状态进入UsingSpecialSkill(不能移动),进入其他状态会导致制作机器人失败。
    - 在持续时间中，学生面前生成道具CraftingBench；学生进入其他状态或该道具被碰撞后，CraftingBench消失且制作机器人失败。
    - 持续时间结束后，道具CraftingBench所在位置生成一个Robot，CraftingBench消失
    - TechOtaku的Robot的PlayerId = TechOtaku的PlayerId + n×5（一局游戏理论人数），其中1<=n<=3（自己的n为0）
    - 新造的Robot的PlayerId的n总是尽量小
  1. UseRobot
    - CD：2s，持续时间：0s
    - 输入额外参数PlayerID,切换到要使用的角色。
    - 切换到其他角色时，自己进入UsingSkill状态。

#### 开心果
- 主动技能
  0. 唤醒 Rouse 
    - CD：120s 
    - 使用瞬间，唤醒可视范围内一个沉迷中的人
    - 通过唤醒获得对应得分
  1. 勉励 Encourage
    - CD：120s
    - 使用瞬间，为可视范围内随机一个毅力不足的人试图补充750000的毅力值
    - 获得勉励750000的毅力值对应得分
  2. 鼓舞 Inspire
    - CD：120s  
    - 使用瞬间，可视范围内学生（包括自己）获得持续6秒的1.6倍速Buff
    - 每鼓舞一个学生得分10

## 细则

### 特殊说明
  - 不加说明，这里“学生”往往包括职业“教师”

### 移动
- 不鼓励选手面向地图编程，因为移动过程中你可以受到多种干扰使得移动结果不符合你的预期；因此建议小步移动，边移动边考虑之后的行为。

### 人物
- 眩晕状态中的玩家不能再次被眩晕(除非是ShowTime)

### 初始状态
  - 初赛玩家出生点固定且一定为空地

### 道具
  - 使用钥匙相当于销毁
  - 可接受指令状态下能捡起或扔道具，在场上即可使用道具

### 交互
  - 被唤醒或被勉励不属于交互状态，翻窗属于交互状态

### 学习与毕业
- 一个校门同时最多可以由一人开启
  - 校门对于人有碰撞体积
  - 隐藏校门显现后对于人有碰撞体积

### 攻击
- 无论近战远程均产生bullet表示攻击
- 前摇期间攻击被打断时，子弹消失。
  - 每次学生受到攻击后会损失对应子弹的攻击力的学习毅力
  - 此处，前摇指 从播放攻击动作开始 攻击者不能交互 的时间

### 沉迷与唤醒
- 不能两人同时唤醒一个人
  - 在被救时沉迷度不增加

### 门
- 每个教学区都有2把钥匙
- 一扇门只允许同时一个人开锁门
  - 开锁门未完成前，门状态表现为原来的状态
- 开锁门进度中断后清空

### 窗
- 由于窗户被占用导致翻窗失败会使先前行动停止
- 翻越窗户是一种交互行为,翻窗一共有两个过程
  - 跳上窗：从当前位置到窗边缘中点，位置瞬移，时间=距离/爬窗速度。中断时,停留在原位置
  - 爬窗：从窗一侧边缘中点到另一侧格子中心，位置渐移，时间=距离/爬窗速度。中断时,停留在另一侧格子中心
- 攻击可以穿过窗，道具可以在窗上
- 有人正在翻越窗户时，其他玩家均不可以翻越该窗户。
  - 通常情况下捣蛋鬼翻越窗户的速度高于学生。

### 箱子
- 地图上有8个箱子
  - 同一时刻只允许一人进行开启
- 未开启完成的箱子在下一次需要重新开始开启。
  - 箱子开启后其中道具才可以被观测和拿取
- 箱子道具不刷新

### 得分
  - 眩晕或毅力值归零时无牵制得分

### 信息相关
- Bgm在没有符合条件的情况下，值为0。
  - 不能给自己发信息

### 技能
- CD冷却计时是在开始使用技能的瞬间开始的
- Klee的小炸弹不受道具增益影响
- 除了切换攻击类型的技能，在不能执行指令的状态下（包括翻窗）均不能使用技能

### 职业
- 学生职业可以重复选择