# Performance testing of network modules-02

> Updated 5/03/2018 Ont commit:5349f95

 
### Setup
- client: ubuntu 2GHz 4GB RAM
- server: macOS 2 GHz Intel Core i5，8 GB RAM
- Parameter：1,000,000 same transactions
- code:  Ont(5349f95)

`Ont(5349f95)` Base on commit 5349f95,a completed network module include all net protocal logic(ping pong addr version etc).

### Steps
- clone https://github.com/BooniesFX/ontology-stress-test, then ==make==
- Setup ontology-stress-test on server
- Use testcli to send  trasactions from a client to the server.
- record time period spend.
- make sure total transactions number on both sides are same.

### Result(compare with last test result)
code | TPS | comment
---|------|------|------|---
Ont(5349f95) |  18000|
Ont(5349f95) |  150000| without DecodePublicKey
Ont(5349f95) |  200000| without Deserialize

code | TPS | comment
---|------|------|------|---
Ont(3d8c6a0  |  150215|
Ont(3d8c6a0  |  203363| without Deserialize

### conclusion
- Both logic have ability to process 200k/s transactions without deserialize
- With deserialize， current code only able to process 18k/s, and without call DecodePublicKey(), will improve to 150k/s, which is the same with legacy code
- DecodePublicKey() used to be called in different module in legacy code. Now move it to the end of network process speed up other modules, but restrict the p2p module performance. 
 

