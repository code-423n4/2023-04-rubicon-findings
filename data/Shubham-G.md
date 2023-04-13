## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-01](#GAS-01) | With assembly, `.call (bool sent)` transfer can be done to optimize gas | 1 |
| [GAS-02](#GAS-02) | Setting the constructor to `payable`| 2 |
| [GAS-03](#GAS-03) | Use solidity version 0.8.19 to gain some gas boost | 6 |
| [GAS-04](#GAS-04) | Split conditions in `if`/`require` statements instead of using `&&` | 21 |


## [GAS-01]  With assembly, `.call (bool sent)` transfer can be done to optimize gas 

"(bool success, bytes memory returnData) = target.call()" automatically copies the return data to memory even if you omit the returnData variable. 
If a relayer executes transactions with such calls it can lead to a gas-griefing attack.

In the given contracts, it is advisable that `return` data `(bool sent,)` which by default is stored due to EVM architecture, is executed with the parameters 
inOffset, inSize, retOffset, retSize values set to (0,0,0,0). In this way, the storage disappears and gas optimization is provided.

For reference, refer to this thread
https://twitter.com/pashovkrum/status/1607024043718316032?t=xs30iD6ORWtE2bTTYsCFIQ&s=19

```solidity
File: main/contracts/utilities/FeeWrapper.sol

- L:118            (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L118

### Recommended Step

```solidity
File: main/contracts/utilities/FeeWrapper.sol

+ L:118             bool OK;
+ L:119             assembly {
+ L:120                    OK := call(gas(), _feeTo, _feeAmount, 0, 0, 0, 0)
+ L:121             }
```

## [GAS-02] Setting the constructor to `payable`

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. The extra opcodes avoided are 
CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2). 
Making the constructor payable eliminates the need for an initial check of msg.value == 0 and saves 13 gas on deployment with no security risks.

There are 2 instances of the topic.

```solidity
File: main/contracts/V2Migrator.sol

- L:30       constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
+ L:30       constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) payable {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30

```solidity
File: main/contracts/utilities/poolsUtility/Position.sol

- L:54       constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
+ L:54        constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) payable {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54

## [G-03] Use solidity version 0.8.19 to gain some gas boost

Upgrade to the latest solidity version 0.8.19 to get additional gas savings.

See latest release for reference: https://blog.soliditylang.org/2023/02/22/solidity-0.8.19-release-announcement/

```solidity
File: main/contracts/BathHouseV2.sol

L:2       pragma solidity 0.8.17;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L2

```solidity
File: main/contracts/RubiconMarket.sol

L:2        pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

```solidity
File: main/contracts/V2Migrator.sol

L:         pragma solidity 0.8.17;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L2

```solidity
File: main/contracts/utilities/poolsUtility/Position.sol

L:2        pragma solidity 0.8.17;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L2

```solidity
File: main/contracts/utilities/FeeWrapper.sol

L:2        pragma solidity 0.8.17;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L2

```solidity
File: main/contracts/periphery/BathBuddy.sol

L:2        pragma solidity ^0.8.0;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

## [GAS-04] Split conditions in `if`/`require` statements instead of using `&&`

Splitting conditions inside `if` statement and `require` statement, saves little amounts of gas.
*There are 21 instances of this issue* in the 6 contracts mentioned in the scope of the audit.

```solidity
File: main/contracts/RubiconMarket.sol

- L:349     if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
+ L:349     if (_offer.owner == address(0) {
+ L:350        if (getRecipient(id) != address(0)) {
                   ....
               }
            }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349
