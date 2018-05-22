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

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyNTQ1MzkwM119
-->