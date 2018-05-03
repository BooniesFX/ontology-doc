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
<table>
  <tr>
    <th width=20%, bgcolor=#eeeeee >code</th>
    <th width=20%, bgcolor=#eeeeee>TPS</th>
    <th width="60%", bgcolor=#eeeeee>comment</th>
  </tr>
  <tr>
    <td bgcolor=#eeeeee> Ont(5349f95) </td>
    <td bgcolor=#eeeeee> 18000 </td>
    <td bgcolor=#eeeeee> </td>
  </tr>
   <tr>
    <td bgcolor=#eeeeee> Ont(5349f95) </td>
    <td bgcolor=#eeeeee> 150000 </td>
    <td bgcolor=#eeeeee> without DecodePublicKey</td>
  </tr>
   <tr>
    <td bgcolor=#eeeeee> Ont(5349f95) </td>
    <td bgcolor=#eeeeee> 200000 </td>
    <td bgcolor=#eeeeee> without Deserialize</td>
  </tr>
</table>

<table>
  <tr>
    <th width=20%, bgcolor=#eeeeee >code</th>
    <th width=20%, bgcolor=#eeeeee>TPS</th>
    <th width="60%", bgcolor=#eeeeee>comment</th>
  </tr>
  <tr>
    <td bgcolor=#eeeeee> Ont(3d8c6a0) </td>
    <td bgcolor=#eeeeee> 150215 </td>
    <td bgcolor=#eeeeee> </td>
  </tr>
   <tr>
    <td bgcolor=#eeeeee> Ont(3d8c6a0) </td>
    <td bgcolor=#eeeeee> 203363 </td>
    <td bgcolor=#eeeeee> without Deserialize</td>
  </tr>
</table>

### conclusion
- Both logic have ability to process 200k/s transactions without deserialize
- With deserialize， current code only able to process 18k/s, and without call DecodePublicKey(), will improve to 150k/s, which is the same with legacy code
- DecodePublicKey() used to be called in different module in legacy code. Now move it to the end of network process speed up other modules, but restrict the p2p module performance. 
 

