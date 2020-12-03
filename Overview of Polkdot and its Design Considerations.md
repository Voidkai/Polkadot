---
marp: true
theme: gaia
_class: lead
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
---
# Overview of Polkdot and its Design Considerations
---
<!-- paginate: true -->
<!-- _class: lead -->
## 简介

---
一个以中心化实体控制的系统对所有用户会造成一系列隐患，例如，系统控制者可以随时关闭系统，贩卖用户数据给第三方，在没有获得用户同意的情况下操控服务工作状态。因此，对于严重依赖商业或个人目的而使用系统服务的用户造成了巨大影响。

对安全，自由，用户自我控制，以及对去中心化应用需求的增长，导致去中心成为一种趋势

---

区块链技术的提出主要就是针对这类问题，去构建一个去中心化的网络应用。一个重要要求是**分离的应用必须能够交互**，否则分离的应用就会被相互隔离。 以太坊以及比特币为POW型区块链，而POW（Proof of work）的安全依赖于用户的工作能力，而POS（proof of stack）的安全则依赖于激励以及摧毁安全储蓄（security deposits）的能力。另一个对于区块链的要求是**可扩展性**。现存区块链设计具有高延迟以及每秒只能处理几十个交易，而诸如Visa的信用卡公司需每秒处理上千个交易。

---
一个解决扩展性的主流方案是平行的运行多条链，常常称为分片。Polka是一个能够**汇聚多条链上安全能力的多链系统**（gather the security power of all these chains）

中继链负责为平行链提供安全保障，以及平行链之间的trust free 跨链交易

解决的主要问题，**交互操作可能性**，**可扩展性**，以及由于安全能力分开后的**弱安全问题**。、

---
<!-- _class: lead -->
## 梗概（polkadot的主要功能）

可扩展异构多链

---
### 安全模型

假设平行链是作为中继链的一个外部不可信任客户端运行，并且中继链只能通过接口与外部平行链交互，而不能对平行链内部做出任何假设，对于平行链内部，可能是开放的也可能是禁止访问或控制的。安全目标是，如果一些平行链用户破坏了平行链，从波卡系统来看整个平行链都是恶意的。

波卡中继链也有设计用来处理一定程度的内部恶意行为（一个开放去中心网络的需求）。当特定的几个节点无法信任，但是还是有一个包含最低需求个数的节点自己是可信任的，协议就能够在确保中继链作为一个整体是可信任的发挥作用。

---
### 节点与角色

节点是执行波卡软件应用的网络层实体，而角色则是承担一定作用的协议层实体。**一个节点可能扮演多个角色**。

在网络层，中继链是开放的。任何节点都可以运行软件并且可以作为任何轻节点或者全节点参与到系统中。

---
节点类型包括以下两类

1. 轻客户端（节点）：只获取特点用户相关的信息

2. 全节点：获取所有类型数据，并长期保存，并负责数据广播

中继链节点可以扮演如下角色：

1. 验证者（Validator）：承当大量的安全工作，且必须是中继链上的全节点。需要与平行链交互但是不需要以平行链上的全节点参与到平行链中。
2. 提名者（Nominator）：支持并选择验证候选人。

---
平行链可以选择自己内部网络结构， 但是需要通过以下两个角色与波卡系统交互：

1. 收集者（Collator）：收集并且提交平行链上的数据并且提交到中继链上。由平行链的规则选择收集者，但是收集者必须是全节点
2. 渔夫（fisherman）：在正常运作的平行链中承担额外安全校验的作用。这个角色自我任命，并且由奖赏激励。

---
<!-- _class: lead -->
## 共识协议

后续会有详细描述

---
<!-- _class: lead -->
## 概念/原语

---
### 角色

1. Validator
   1. 最高权力角色，并且帮助在波卡网络中封新的区块
2. nominator
3. collator
4. fishmen

### 波卡的敌手模型



---

## Q：理解波卡的跨链概念时，会遇到的问题？

1. 收集人在做什么，它巨额平行链和中继链的关系是什么
2. 跨链到底是什么东西从平行链跨到中继链
3. 平均每个链是个验证者为什么就可以保障安全性
4. 所谓的pooled security到底是什么
5. 与跨链共识相关的各种数据，那些上链了，那些没有，设计依据是什么
6. 波卡基于Substrate而建，那些模块适合放到波卡上 哪些适合放在substrate上
7. 为什么需要纠删码技术VRF技术
8. TPS能达到多少 为什么？


---
## 波卡架构

1. Relay chain 负责网络的安全性 共识机制以及跨链互操作性
2. Parachains 可以拥有自己的代币并且针对特定的场景优化其功能
3. Parathreads， 与Parachains不一样的是，该结构采用的是即用即付的收费方式，对于不需要持续连接网络的区块链来说更加经济
4. Bridges 允许parachain和parathreads 连接Ethereum 和 Bitcoin这类区块链

---
<!-- _class: lead -->
## 共识参与者

1. Nominators
2. Validators
3. Collators
4. Fishermen



## 平行链阶段

### Collator激励

1. 平行链只需要一个诚实的收集人来提交区块
2. 从中继链的角度看，Collator不需要抵押代币
3. collator作为收集人没有收益，但是如果有奖励，需要从parachain上去实现
4. collator也可以扮演fisherman的角色，从而获得收益

### 测试方法

在polkadot的代码仓库中，有一个adder模块，用来模拟一个最简单的parachain。在实际过程中，parachain需要参考cumulus代码库，将其接入relay chain



## 中继链阶段

1. 接受candidate block
2. 使用stf验证状态转移是否正确
3. 广播验证结果 给其他validator
4. 一旦超过一半validator认可 便可准备candidate receipt
5. 使用纠删码保存block数据在validator的本地数据中而不会上链
6. 发送交易到节点交易池
7. 打包节点将candidate receipt打包进入区块

### Slot

- collator生成了候选区块后，通过p2p网络，将其传送给relay chain的validators
- 在relay chain的每个slot中，会对每个parachain分配若干个validators
- validators per parachain = （validator count -1 ）/ parachain count，剩余一个validator不会被分配在任何你一个parachain上
- 他们会负责验证候选区块（parablock）

###  duty_roaster

- 每个parachain分配validators
- 基于链上的随机数
- 按照shuffle算法
- 进行乱序分配的，每个slot分配的validators都是随机的
- 并没有使用VRF

---
### VRF

verifiable random function， babe中确定出块validator的算法

- 一般的hash函数
- result = hash（SK， info）
- result = vrf_hash(SK, info)
- proof = vrf_proof(SK, info)
- vrf_verify(PK, info, result, proof) -> T or F
- SK and PK 是Validator的一个密钥对
- 这样的方式既可以验证info和result的关联关系也可以知道是哪个validator签署的

---
#### VRF在波卡中的应用

在波卡中，如果result小于一个阈值，那么这个validator才算被选中运行出块
- info的值在每个slot中是一个全网固定的值
- 在波卡中 info称为transcript， 是将一个随机数、 slot number、epoch index，三个元素拼在一起
- 因为validator是否选中来出块， 是根据vrf计算而定的，所以一个slot，会有一个、多个、或零个节点在vrf过程中胜出
  - 一个为正常情况 primary slot leaders
  - 多个validator出块，最后需要prandpa来确定哪一个分叉为最终链
  - 零个： 使用round robin确定一个出块节点

---
### STF

state transfer function

---
#### 默克尔树

- parachain的state 保存在一个默克尔树的结构中
- 所有的状态都是该树的叶子
- 相邻两个节点取hash值
- 直到状态根

---
#### 验证过程

- collator 提供以下数据
  - candidate_block——1
  - validity proof，计算出状态根
    - 该区块修改的平行链数据库中的值——2
    - 默克尔树中没有收影响的hash值——3
- relay chain的validator基于2,3计算出状态根S1
- relay chain的validator使用parachain的 STF wasm镜像，基于修改的数据即2， 以及执行区块交易1，保证只修改了2的内容，同时得到新的值， 修改后的值 4
- 基于 3/4 计算当前block的状态根S2
- 如果S1和S2相同则验证成功

---
polkadot保证的是有效地状态转换而不是保证有效状态

trust free 无信任

shared security 共享安全

---
### executor 验证器

---
<!-- _class: lead -->
## 共识算法








