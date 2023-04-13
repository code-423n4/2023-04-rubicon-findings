1)owner of the RubiconMarket won't be used in the contract


2)owner setted on "initialize" on the RubinconMarket contract comes from the msg.sender,and by doing so the owner will become the proxy, which could be involuntary, I recommend passing an address in the parameters such as _newOwner

3)require success on "cancel" in different part of RubiconMarket(doesnt check cancel return)
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L912
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L925
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1202

same problem in this situation:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1314

4)should lock the contract before changing fee/setmakerfee (RubiconMarket)

5)in "initialize",feeBPS should be 1000 (rapresenting 1%) instead of 1,cuz then will be devided by 100k

6)fee calculation could be rounded towards 0,and not paying any fees in RubiconMarket
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338
or
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L346

7)no need to check return of transferFrom(if works will go throught if desnt revert,without the need to manage bool return)
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L350-L357
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L339-L342
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L359-L362

8) no check on return in V2Migrator "migrate" function
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59-L62

9)should start using custom errors in every contract

10)it's useless to check for balance == 0,after success transfer of balance in "migrate" function V2Migrator
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L63-L66

11)check that v1v2Pools[bathTokenV1] != addr(0)
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L38

12)consider allowing the owner to be transfered (RubiconMarket)
13)consider adding a blacklist system on the market(RubiconMarket)
