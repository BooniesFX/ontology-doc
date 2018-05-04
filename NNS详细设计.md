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


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDQ5NjMwNTRdfQ==
-->