## Summary

### Gas Optimizations
| |Issue|Instances|Total Gas Saved|
|-|:-|:-:|:-:|
| [G&#x2011;01] | Uints can be packed into fewer storage slots | 1 |  - |
| [G&#x2011;02] | DSMath can be removed | 3 |  - |
| [G&#x2011;03] | Reentrancy guard synchronized() should use uints instead of bools | 1 |  13866 |
| [G&#x2011;04] | Multiple keccak256 hashes can be saved in a variable | 1 |  - |




Total: 6 instances over 4 issues 


## Gas Optimizations

### [G&#x2011;01]  Uints can be packed into fewer storage slots

Variables are stored in 32 byte slots, if we have a uint256 we have fully packed a storage slot. However, you can pack multiple variables of smaller size so that they fit in a single slot. Because all the uints are mostly used together in the function this will decrease the gas cost for SLOAD and SSTORE because of accessed storage keys which are a set of storage slot keys accessed by a contract address



Uint sizes should be determined by the project, but in some cases uint256 is cast to uint128 so you can use uint128 instead of uint256 in the struct. Example below 



*There are 5 instances of this issue, but it can only be reliably addressed in one of them:*

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L234

```solidity
File: contracts\RubiconMarket.sol

/// @audit Because require(uint128(pay_amt) == pay_amt); and require(uint128(buy_amt) == buy_amt);
/// pay_amt and buy_amt in OfferInfo can be packed into one storage slot using uint128

234 struct OfferInfo {
235      uint256 pay_amt;
236      ERC20 pay_gem;
237      uint256 buy_amt;
238      ERC20 buy_gem;
239      address recipient;
240      uint64 timestamp;
241      address owner;
242 }
```

### [G&#x2011;02]  DSMath and SafeMath can be removed

In Solidity 0.8.0, "safe math" was introduced by default. This means every math operation is checked for over/under flow. Under/overflow is only possible in unchecked blocks so DSMath and SafeMath can be removed

*There are 3 instances of this issue:*

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L45
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L17
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L39



### [G&#x2011;03]  Reentrancy guard synchronized() should use uints instead of bools

When using bools, the expensive 0 to non-zero SSTORE operation is used when changing from false to true. Uints use only use non-zero to non-zero SSTOREs which are much cheaper


*There is 1 instances of this issue:*

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L264

```solidity
File: contracts\RubiconMarket.sol
/// @audit Use a uint(1 & 2) instead of a bool to avoid a zero to non-zero SSTORE
264 modifier synchronized() {
265       require(!locked);
266        locked = true;
267        _;
268        locked = false;
269 }
```

This saves 13866 gas(runtime)


### [G&#x2011;04]  Multiple keccak256 hashes can be saved in a variable

The keccak256 hash of _offer.pay_gem and _offer.buy_gem is being hashed 4 times in the events. This results in redundant computations and unnecessarily high gas costs. To optimize this code, the hash can be computed only once and then saved as a variable

*There is 1 instance of this issue:*
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L314

```solidity
File: contracts\RubiconMarket.sol

///@audit the keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)) should be computed only once and then saved as a variable

365 emit emitFee(
366     bytes32(id),
367     msg.sender,
368     _offer.owner,
369     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
370     _offer.buy_gem,
371     mFee
372 );



398 emit emitTake(
399      bytes32(id),
400      keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
401      msg.sender,
402      _offer.owner,
403      _offer.pay_gem,
404      _offer.buy_gem,
405      uint128(quantity),
406      uint128(spend)
407 );



419 emit emitFee(
420     bytes32(id),
421     msg.sender,
422     feeTo,
423     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
424     _offer.buy_gem,
425     fee
426 );


440 emit emitDelete(
441    bytes32(id),
442    keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
443    _offer.owner
444 );

```


