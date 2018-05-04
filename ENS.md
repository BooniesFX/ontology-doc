# ENS 简介
ENS 是以太坊上的一个DNS服务，它以合约的形式提供了对以太坊上的账户地址，合约地址，内容hash，ABI文件等常用的难以记忆的路径和字符串进行名字转换。提供了对域名的拥有者的信息注册，解析方法存储，权益转移，交易，以及域名拍卖等dapp。
## ENS 基本框架
###  注册器
	 - 域名所有者
	 - 域名解析器
	 - 域名有效期
- 所有者
域名所有者可以是一个账户地址，也可以是一个智能合约。
所有者的权限包括：
	- 设置域名解析器和有效期
	- 转移域名的所有权
	- 改变子域名的所有权
 ### 解析器
 解析器存储了真实地址和自定义短字符之间的map关系。ENS提供了一个解析器标准，只要按照标准编写的解析器合约均可被注册器认为是可用的解析器而不用去更改注册器合约地址。
 ### hash算法
 任何的自定义域名在ENS里被转换为32字节的hash字符，这样可以简化处理和存储流程。同时提供一定的匿名性。
 域名内部以“.”作为分割符，同时利用递归hash计算每个分隔开的标签直到算出整个域名的hash。
 ### 接口实现
 ENS定义了各种类型的解析器协议，通过实现对应的接口就可以被注册器合约调用而不用修改注册器合约。

| Record type | Function(s) | Interface ID | Defined in|
|--|--|--|--|
| Ethereum address | addr | 0x3b3b57de | [EIP137](https://github.com/ethereum/EIPs/issues/137) |
|ENS Name|name|0x691f3431|[EIP181](https://github.com/ethereum/EIPs/issues/181)
|ABI specification|ABI|0x2203ab56|[EIP205](https://github.com/ethereum/EIPs/pull/205)
|Public key|pubkey|0xc8690233|[EIP619](https://github.com/ethereum/EIPs/pull/619)
 
 ### ENS合约接口
 ```
 contract ENS {
    function owner(bytes32 node) constant returns (address);
    function resolver(bytes32 node) constant returns (Resolver);
    function ttl(bytes32 node) constant returns (uint64);
    function setOwner(bytes32 node, address owner);
    function setSubnodeOwner(bytes32 node, bytes32 label, address owner);
    function setResolver(bytes32 node, address resolver);
    function setTTL(bytes32 node, uint64 ttl);
}

contract Resolver {
    function addr(bytes32 node) constant returns (address);
}
 ```

