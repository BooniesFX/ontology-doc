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
Dymaxion主网通过P2P连接，组合成的多条网络已经运行3年
### Dymaxion的匿名性

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDMxMDE1OTEyXX0=
-->