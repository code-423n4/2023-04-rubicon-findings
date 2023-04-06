### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|
| [G-01] |Using double `require` instead of operator `&&` can save more gas| 2 |
| [G-02] |Using `> 0` costs more gas than `!= 0` when used on a `uint` in a `require()` statement| 9 |
| [G-03] |Using `delete` instead of setting state variable/mapping to 	`0` saves gas| 1 |

## [G-01] Use double `require` instead of using `&&`
Using double `require` instead of operator `&&` can save more gas
When having a require statement with 2 or more expressions needed, place the expression that cost less gas first.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L95-L100
```solidiy
File: contracts/periphery/BathBuddy.sol

   require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L595-L598
```solidity
File:contracts/utilities/poolsUtility/Position.sol

 : require(
                _leverage >= _wad && _leverage <= _leverageMax,
                "_leverageCheck{Short}: INVLAID LEVERAGE"
            );
```

## [G-03]Using > 0 costs more gas than != 0 when used on a uint in a require() statement
This change saves 6 gas per instance.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1410
```solidity
File: contracts/RubiconMarket.sol
 require(_span[pay_gem][buy_gem] > 0);
 ```
 https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L521
 ```
 File: contracts/RubiconMarket.sol
  require(pay_amt > 0);
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L523
```
File: contracts/RubiconMarket.sol
require(buy_amt > 0);
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L581
```
File: contracts/RubiconMarket.sol
require(amount > 0);
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1209
```
File: contracts/RubiconMarket.sol
require(id > 0);
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1226
```
File: contracts/RubiconMarket.sol
require(id > 0);
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L41
```
File: contracts/V2Migrator.sol
 require(bathBalance > 0, "migrate: ZERO AMOUNT");

```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L313
```
File: contracts/utilities/poolsUtility/Position.sol
 require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L318
```
File: contracts/utilities/poolsUtility/Position.sol
require(_max > 0, "_maxBorrow: can't borrow 0");
```
## [G-04]Using delete instead of setting state variable/mapping to 0 saves gas.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L251
```
File: contracts/periphery/BathBuddy.sol
 tokenRewards[token][account] = earned(account, token);
```