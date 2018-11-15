# NNS 详细设计
## 域名存储
32字节散列值，递归hash，所有层级的域名都保存在一个Map<hash256, 解析器>
### NameHash算法
```
static byte[] nameHash(string domain)
{
	return SmartContract.Sha256(domain.AsByteArray());
}
static byte[] nameHashSub(byte[] roothash, string subdomain)
{
	var bs = subdomain.AsByteArray();
	if (bs.Length == 0)
		return roothash;
	var domain = SmartContract.Sha256(bs).Concat(roothash);
	return SmartContract.Sha256(domain);
}
static byte[] nameHashArray(string[] domainarray)
{
	byte[] hash = nameHash(domainarray[0]);
	for (var i = 1; i < domainarray.Length; i++)
	{
		hash = nameHashSub(hash, domainarray[i]);
	}
	return hash;
}
```
## 顶级域名合约详解

三种接口：

 - 通用接口类似utility
 - 所有者接口：
		- 转让所有权
		- 设置注册器
		- 设置解析器
- 注册器接口
## 所有者合约
- 合约采取Appcall的形式调用顶级域名合约的所有者接口
- 实现复杂的所有权模式
## 注册器合约
- 采取Appcall的形式调用顶级域名合约的setsubdomainowner接口
- getsubowner
- requestsubowner
## 解析器合约
- 保存解析信息
- 顶级合约调用接口获取信息
- 设置解析数据调用顶级合约来验证域名所有权
- resolve
- SetResolveData
## 投标
- 开标
- 投标：hash混淆报价
- 揭标：提供hash明文和报价，扣除系统费用
- 发送交易获取域名所有权
- 交易
## NNC
- 无锁定资产
- 租金，拍卖所得等进去奖池
- 账户固定资产存入的块数小于奖池的块数则可以提取奖励
- 类似于持有neo提取gas，但是奖池数目一定，超过奖池上限则投入新奖池中，只能按比例提取新奖池的NNC

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3Mzc2ODk5MzUsMzk0MDA4ODAwXX0=
-->