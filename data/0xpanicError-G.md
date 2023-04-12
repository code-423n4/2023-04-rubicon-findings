# Gas Report


| |Issue|Instances|
|-|:-|:-:|
| [G-1](#G-1) | Use uint8 = 1/2 instead of bool to save gas | 4 |
| [G-2](#G-2) | Cache storage elements in memory | 2 |
| [G-3](#G-3) | Declare array length in memory | 1 |
### [G-1] Use uint8 = 1/2 instead of bool to save gas 
Setting a state variable from zero to non-zero requires a lot more gas that changing its value from a non-zero 
to a non-zero. Hence, using a uint8 and using 1 as true and 2 as false can save a lot of gas instead of using a bool.

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

*SSTORE requires 20,000 to store from zero to non-zero but only 2900 to change non-zero to non-zero*

*Instances (4)*:
```solidity
File: contracts/RubiconMarket.sol

12:   bool public buyEnabled = true; //buy enabled TODO: review this decision!

13:   bool public matchingEnabled = true; //true: enable matching,

16:   bool public initialized;

19:   bool public AqueductDistributionLive;

```

### [G-2] Cache storage elements in memory
If a storage element is called multiple times in a function, store it and call from memory to save gas. 
`offers[offerId].pay_amt` and `offers[offerId].buy_amt` have been called from storage multiple times.

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

*SLOAD requires 2100 or 100 gas while MLOAD only requires 3*

*Instances (2)*:
```solidity
File: contracts/RubiconMarket.sol

422:   wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

428:   fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator

429:   buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

430:   take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

437:   rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

```

### [G-3] Declare array length in memory
In `getBuyAmount` function, `while` loop runs till `pay_amt > offers[offerId].buy_amt` which is loaded
from storage in every iteration. storing it in memory would save gas. (not declared in automated findings)

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

*SLOAD requires 2100 or 100 gas while MLOAD only requires 3*

*Instances (1)*:
```solidity
File: contracts/RubiconMarket.sol

467:   while (pay_amt > offers[offerId].buy_amt) {

```