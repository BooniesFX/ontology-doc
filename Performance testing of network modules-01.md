# Performance testing of network modules-01

> Updated 3/01/2018 Ont commit:3d8c6a0

 
### Setup
- client: ubuntu 2GHz 4GB RAM
- server: macOS 2 GHz Intel Core i5，8 GB RAM
- Parameter：10,000,000 same transactions
- code:  Ont(3d8c6a0) DNA(NET only)

`DNA(NET only)` Base on DAN code and only net module left. The module only process transactions.

`Ont(3d8c6a0)` Base on commit 3d8c6a0,a completed network module include all net protocal logic(ping pong addr version etc).

### Steps
- Setup a node on client and server, make sure them are connected.
- Use a test cmd to send predefined trasactions payload from client to server.
- Track the cpuload on both client and server and record the transaction number.
- Total transactions number on both sides should be the same.

### Result
code |cpuload(client) |cpuload(server) |time(s) | TPS 
---|------|------|------|---
DNA(NET only) |58%|180%| 49.173 |  203363
Ont(3d8c6a0) |55%|170%| 53.064 |  188451

### Result
- The DNA(NET only) performance is the uplimit of the NET module since it only process transactions.
- Due to the cpuload on both sides, the server side have reach the limit.
- Performance of Ont(3d8c6a0) is close to the DNA(NET only).
- Single Net module`s TPS is amost at 190000. 


