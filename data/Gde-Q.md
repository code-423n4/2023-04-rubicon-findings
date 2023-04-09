# Report

## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Array length mismatch | 1 |
| [L-2](#L-2) | Unspecific compiler version pragma | 1 |
### <a name="L-1"></a>[L-1] Array length mismatch
When you encounter a function that accepts arrays as arguments, always check if there's a possibility of index out-of-bounds error.  
If the lengths of the arrays are not the same, the transaction will revert due to index out of bounds errors.  
To prevent this vulnerability, it's important to verify that the lengths of the arrays are equal before executing the function.  This is a common practice that can help catch errors before they result in failed transactions.
```solidity
require(array1.length == array2.length, "arrays length is not equal");
```

*Instances (1)*:
```solidity
File: contracts/V2Migrator.sol

30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
            for (uint256 i = 0; i < bathTokensV1.length; ++i) {
                // set v1 to v2 bathTokens
                v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];

```
[Link to code](https://github.com/code-423n4/2023-04-rubiconcontracts/V2Migrator.sol)

### <a name="L-2"></a>[L-2] Unspecific compiler version pragma
Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs.  Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors.  
It is recommended to fix a concrete compiler version (latest without security issues).  
You can see what new versions offer regarding bug fixed [here](https://github.com/ethereum/solidity/blob/develop/Changelog.md)  

*Instances (1)*:
```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```
[Link to code](https://github.com/code-423n4/2023-04-rubiconcontracts/RubiconMarket.sol)



## Refactoring issue

The `isAuthorized()` method could be removed and
replaced directly in the `auth` modifier by: `msg.sender == owner`

Before:
```solidity
File: contracts/RubiconMarket.sol
30:     modifier auth() {
31:         require(isAuthorized(msg.sender), "ds-auth-unauthorized");
32:         _;
33:     }
34: 
35:     function isAuthorized(address src) internal view returns (bool) {
36:         if (src == owner) {
37:             return true;
38:         } else {
39:             return false;
40:         }
41:     }
```

After:
```solidity
    modifier auth() {
        require(owner == msg.sender, "ds-auth-unauthorized");
        _;
    }
```

## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Unused Function Parameters | 1 |
| [NC-2](#NC-2) | Unused Local Variable | 1 |
| [NC-3](#NC-3) | Setter/Update functions should emit an event | 3 |
| [NC-4](#NC-4) | Function state mutability can be restricted to view | 1 |
| [NC-5](#NC-5) |Function state mutability can be restricted to pure | 1 |
| [NC-6](#NC-6) | Contract does not follow the Solidity style guide's suggested layout ordering | 1 |
### <a name="NC-1"></a>[NC-1] Unused Function Parameters
Unused function parameters could be removed or commented out to silence compilation warning.

*Instances (1)*:
```solidity
File: contracts/utilities/poolsUtility/Position.sol

528:         address _asset,

```
[Link to code](https://github.com/code-423n4/2023-04-rubiconcontracts/utilities/poolsUtility/Position.sol)

### <a name="NC-2"></a>[NC-2] Unused Local Variable
Consider removing unused local variables.

*Instances (1)*:
```solidity
File: contracts/RubiconMarket.sol

298:         uint256 id = uint256(id_);

```
[Link to code](https://github.com/code-423n4/2023-04-rubiconcontracts/RubiconMarket.sol)

### <a name="NC-3"></a>[NC-3] Setter/Update functions should emit an event
Consider having events associated with setter/update functions to track important changes.

*Instances (3)*:
```solidity
File: contracts/RubiconMarket.sol

1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {

```
[Link to code](https://github.com/code-423n4/2023-04-rubiconcontracts/RubiconMarket.sol)
  

### <a name="NC-4"></a>[NC-4] Function state mutability can be restricted to view

```solidity
File: contracts/utilities/poolsUtility/Position.sol
526:     function _borrowLimit(
```  

### <a name="NC-5"></a>[NC-5] Function state mutability can be restricted to pure

```solidity
File: contracts/utilities/FeeWrapper.sol
28:     function calculateFee(
```



### <a name="NC-6"></a>[NC-6]Contract does not follow the Solidity style guide's suggested layout ordering  

The style guide says that, within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Modifiers, and 5) Functions, but the contract(s) below do not follow this ordering.
https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout

This modifier should be moved before the initialize method l.700
```solidity
File: contracts/RubiconMarket.sol
719:     modifier can_cancel(uint256 id) override {
720:         require(isActive(id), "Offer was deleted or taken, or never existed.");
721:         require(
722:             isClosed() ||
723:                 msg.sender == getOwner(id) ||
724:                 id == dustId ||
725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
726:             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
727:         );
728:         _;
729:     }
```