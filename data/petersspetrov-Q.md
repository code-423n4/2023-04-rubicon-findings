|   NC  | Issue | Instances |
| --- | --- | --- |
| NC-01| Using a floating pragma `^0.8.9`,`0.8.0` statement is discouraged |2|
| NC-02| Use a more recent version of solidity |6|
| NC-03| Constant values such as a call to keccak256(), should used to immutable rather than constant |1|
| NC-03| Use of bytes.concat() instead of abi.encodePacked() |6|

## [NC-01]Using a floating pragma `^0.8.9`,``^0.8.0`` statement is discouraged as code can compile to different bytecodes with different compiler versions. Use a stable pragma statement to get a deterministic bytecode.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2
```
File: contracts/RubiconMarket.sol
pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2
```
File: contracts/BathHouseV2.sol
pragma solidity ^0.8.0;
```

## [NC-02]Use a more recent version of solidity
use last version of solidity `0.8.19`.
Use a solidity version of at least `0.8.0` to get overflow protection without SafeMath.
Use a solidity version of at least `0.8.2` to get compiler automatic inlining.
Use a solidity version of at least `0.8.3` to get better struct packing and cheaper multiple storage reads.
Use a solidity version of at least `0.8.4` to get custom errors, which are cheaper at deployment than revert()/require() strings.
Use a solidity version of at least `0.8.10` to have external calls skip contract existence checks if the external call has a return value.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L2
```
File: contracts/BathHouseV2.sol
pragma solidity 0.8.17;
```
```
File: contracts/V2Migrator.sol
pragma solidity 0.8.17;
```
```
File: contracts/utilities/poolsUtility/Position.sol
pragma solidity 0.8.17;
```
```
File: contracts/utilities/FeeWrapper.sol
pragma solidity 0.8.17;
```


## [N-03] Constant values such as a call to keccak256(), should used to immutable rather than constant
There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand.

Constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232
 ```SOLIDITY
 File: `contracts/RubiconMarket.sol`
232: bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
 ```
## [G-04] 	Use of bytes.concat() instead of abi.encodePacked()
Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked(,) Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(,) Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L369
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L400
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L423
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L442
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L476https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L555
```
File:contracts/RubiconMarket.sol
369: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

400:  keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

423: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

442: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

476: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

555: keccak256(abi.encodePacked(pay_gem, buy_gem)),
```
