# Burst 白皮书解读
burst 是一种基于DAG和tangles的新型区块链项目。使用了POC共识算法来解决能量消耗，公平性和去中心化。底层使用与比特币的闪电网络的交易通道相似的概率来进行交易的传播和验证，但是技术上使用的是类似于IOTA的tangle（Dymaxion layers）。
## building blocks
### burstcoin 
burstcoin 从NXT演化而来，但是使用了POC的共识算法，这个算法使用存储而不是算力来节省能源消耗。效率和POS类似，但是不用质押，参与挖矿的门槛很低。
### 智能合约
burst使用一种名叫Automated Transaction的智能合约模式
### DAG &Tangle
![一个简单的5节点DAG图](https://github.com/BooniesFX/ontology-doc/blob/master/attach/dag.png?raw=true)
IOTA白皮书描述了一种叫tangle的模式，可以支持微型交易，主要用于IOT领域。如果一个交易的价值越小，那么可以利用这些交易的应用就越多，但是这样就需要交易费更便宜，使得激励问题更突出。

IOTA的做法是
- 节点需要验证两个交易后才能获得费用去发起一个新交易
-  如果发现一笔交易和缠结历史冲突，则这笔交易不会被验证通过
-  如果一个节点发起了一笔冲突的交易，那么周边的节点就不会验证它的新交易

IOTA白皮书中提出了一种类似于POW的算法来验证交易
不发交易的节点的激励
- 从邻接点同步数据可以得到一定的激励
- 如果一个节点被认为“太懒”，那么它的邻接点就会断开和它的连接
### zk-SNARK vs 环签名
![ring signature scheme](https://github.com/BooniesFX/ontology-doc/blob/master/attach/ring_sig.png?raw=true)
zk-SNARK可以划分为一下四个部分：
- 可以表示为一个多项式问题
- 随机采样验证
- 同态加密
- 零知识证明

### 原子跨链交易（ACCT）
分布式交易步骤：
A vs B
1. A 生成一些随机数据秘密x
2. A发起支付交易Tx1，输出中带有跨链交易的智能合约：资产只能通过被A和B一起签名或是x和B的签名。x在合约中以hash的形式存在，该合约不会被广播
3. A发起合约交易Tx2，这个合约花费Tx1，并且将剩余输出返回给A。这个合约拥有一个锁定时间的概念。A对Tx2签名后发给B，B签名后再发回给A
4. A广播Tx1和Tx2。B这时可以看见资产，但是因为B没有这笔资产的输出所以暂时还不能使用，这时交易还没有完成。
5. B将上述过程在自己的链上以相反的方向再做一次，B的锁定时间会比A的锁定时间长。这时两边的交易都达到未完成的状态。
6. 因为A知道x，所以A可以立刻得到他的资产。然后B就可以用A的x来解锁自己的交易
ACCT的缺点是交易双方都得采用相同的策略来实现交易，这需要对双方的代码都进行修改。Burst 和 Qora之间已经有了一个ACCT的原型实现
### 闪电网络
链下的点对点双向支付通道
ACCT->HTLC
闪电网络的建立和中继需要强有力的第三方中介帮助完成
### 染色币
染色在交易中附加一些元数据，让固定的数字资产可以代表不同的现实时间的价值
当一个币被染色后，它就可以在Burst网络里和其他币一起交易了。这是burst资产交易的基础。
## 技术融合
Dymaxion Tangle 由上面提到的多种技术演进而来，组成了一个全新的框架
### Dymaxion Tangle vs. IOTA Tangle 
IOTA原来使用Curl哈希算法来做交易签名，后来被证明该算法可被伪造攻击[IOTA vulnerability report](https://github.com/mit-dci/tangled-curl/blob/master/vuln-iota.md)，交易签名不用curl算法，但是其他的过程中仍然使用这种算法。基于对curl的担心，Dymaxion会使用其他的签名算法。同时Dymaxion会有任意个逻辑上独立的tangle而不只是IOTA一个tangle，IOTA的tangle可以在Dymaxion里存在。

### Dymaxion 链上互动
Dymaxion分为主链（记账）和各种染色的tangle链。在每个主链的块上均可以打开和关闭一个tangle.
主链的出块时间是4分钟，每个tangle可以在同一个块上打开和关闭。同时资产在各种链之间通过主链来流转。
每条tangle都可以看做是Burst的闪电网络。
![top-view](https://github.com/BooniesFX/ontology-doc/blob/master/attach/Dymaxion.jpg?raw=true)
### P2P for ad-hoc DL
Dymaxion主网通过P2P连接，组合成的多跳网络已经运行3年
### Dymaxion的匿名性
在新建每个DL的时候都可以选择环签名或是zk-SNARKs 。在主链上不会使用匿名算法。主要是考虑zk-SNARKs 的算法风险，如果有超发的风险，那么在子链上实现匿名签名的话风险会小很多，DL层的ttl可以让风险只停留在子链上。
由于主链不会才有匿名算法，同时主链上的许多地址会重复使用，因此主链与DL层的交易可能破坏子链的匿名性。为了解决这个问题，可以使用一种匿名的ACCT模式。即主链的secret和子链的secret不用保持一致。
## DL实现
-  初始化
	1. 发起者设置
	2. 创建DL（节点发现）
	3. 订阅阶段
- 运行
- 关闭
	1. 节点DL关闭广播
	2. 结算
### 创建一个DL
1. DL初始化（ACTT）
	- 共识机制，POC,POW,POS
	- TTL，2^17块
	- 订阅时间，发起者对他们在tangle上抵押签名时限，max 360 blk
	- 抵押，对tangle上的units的担保 utxo
	- 特性，比如匿名性
 	- units，类似发行IPO
 	- 订阅者，参与tangle的人
 	- 花费，分为固定费用和可变费用，固定费用是创建一个tangle的费用。可变费用按照订阅者的数量来算。可变费用会锁定到tangle结算
一般来说发起者是tangle的第一个订阅者，开始需要将burst地址上的资产进行锁定，后来的订阅者也都需要这样操作。如果一个tangle成功的被矿工记录下来，那么它就会获得一个唯一ID，它的颜色，同时unit都将被确定。不同的tangle之间无法直接交易，需要通过主链来转换。
### 节点发现
首先DL初始要求被广播到需要的peers，peers继续广播这些需求并检查自己是否符合这些要求，如果符合的话则称为DL的一部分
### DL 订阅
如果发起者广播的需求里没有订阅者列表的话，那么所有的节点都可以参与到这个tangle里，如果有订阅者列表，只有列表中的节点可以参与到私有tangle中。订阅者需要在订阅时间内对自己的地址进行签名来声明自己已订阅该tangle，因此整个tangle 的ttl是要求大于订阅时限的。整个DL的TTL可以是一个块（4 min)
## 交易
tangle上的交易是由其他的节点来验证的。现在使用的是POC的算法，这个算法需要大概1-5G的存储，而且不会有某个矿工因为有几百TB的空间而比5G的矿工获得更多的算力支持。
## 关闭DL

- DL结束的条件
	1. TTL到期
	2. 除了发起者没人在订阅时限里参与进来

### tangle 记录
tangle的参与者可以自己选择保留tangle 的记录，这个记录不会留在主链上，主链上只会有创建和关闭tangle的hash记录。这个记录可以证明留在节点那的记录的真实性。
## 节点共谋攻击
部分节点共同构造双花交易并互相验证产生寄生链
IOTA: 马尔科夫链模特卡洛算法
BURST：bft-smart ，f作恶节点，2f-1个诚实节点
## DDOS攻击
tangle上的交易时不需要费用的。burst引进了POC算法来保证不会被DDOS攻击。同时由于在交易中作恶的点会被记录被列入黑名单中。
## 性能指标
从网络性能上来说，可以达到
- 1683 tx/s ordinary payments
- 168 tx/s RS-anonymous transactions 
- 112 tx/s zk-SNARK-anonymous transactions
从延时来说，只有 3756 tx/块

## POC
![enter image description here](https://github.com/BooniesFX/ontology-doc/blob/master/attach/poc.png?raw=true)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYwMjk1NDg3NV19
-->