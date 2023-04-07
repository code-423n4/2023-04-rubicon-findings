#L-01
Local variable shadowing
Detection of shadowing using local variables.
The use of owner in sensitive function might be incorrect.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L23-L42
```solidity
RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,address,address).owner (contracts/RubiconMarket.sol#808) shadows: 

- DSAuth.owner (contracts/RubiconMarket.sol#23) (state variable) 

RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).owner (contracts/RubiconMarket.sol#831) shadows: 

- DSAuth.owner (contracts/RubiconMarket.sol#23) (state variable) 

RubiconMarket._matcho(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).owner (contracts/RubiconMarket.sol#1280) shadows: 

- DSAuth.owner (contracts/RubiconMarket.sol#23) (state variable) 
```


