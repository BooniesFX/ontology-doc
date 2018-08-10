# 网络模块对比 noise vs cellnet

 - [noise](https://github.com/perlin-network/noise)
 - [cellnet](https://github.com/davyxu/cellnet)

## 项目情况

|  | cellnet |noise|
|--|--|--|
|commit| 392 | 678 |
|star| 2076 | 694 |
|fork| 489 | 47 |
|start| Aug/15 | Jun/18  |

## Feature
noise: 
- [KCP](https://github.com/xtaci/kcp-go)/TCP and [Protobufs](https://developers.google.com/protocol-buffers/).
- NAT-PMP, UPnP
- [Ed25519](https://tweetnacl.cr.yp.to/)
- Kademlia DHT
-   Request/Response and Messaging RPC.
-   Logging via.  [glog](https://github.com/golang/glog).
-   Plugin system.

cellnet:
-  TCP/KCP、UDP，Protobuf
-  HTTP，json，二进制
-  WebSocket


## 部署方式
noise:  glide (修改后)

## 特点
noise: 
	- peer管理逻辑节点
	- 消息签名
	- 分链路通道

## 性能
noise: 10k+ sig  150k+ without sig
cellnet: 110k+ without sig


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MzUwMTc2NTNdfQ==
-->