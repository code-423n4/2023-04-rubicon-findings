#### Redundant initialization and validation of effectively immutable state variables

##### Severity: Gas Optimization

##### Context: 

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L675

##### Description

`RubiconMarket.sol` contains two state vars, `buyEnabled` and `matchingEnabled` which exist only to maintain storage slots during contract upgrade. These are both set as true and there exists no logic to change them so they're effectively immutable. 

All though they're already set, in `initialize` they are set to true once again which is redundant. Additionally, there exists validation logic which checks whether these values are set to true. This logic is completely redundant and increases gas costs without any use.


#### Redundant usage of SafeMath libraries

##### Severity: Gas Optimization

##### Description

The contracts in scope are all >= solidity v0.8.0, which includes SafeMath logic by default. Regardless of this, SafeMath logic is used both with OpenZeppelin's SafeMath library and DSMath. All arithmetic using SafeMath logic is redundant and can be replaced with simple operations to reduce gas cost.


#### Redundant `isClosed()` checks

##### Severity: Gas Optimization

##### Context:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620

##### Description

In `RubiconMarket.sol`, there exist checks which reference the value of `isClosed()`. This however, always returns false, and thus every reference to it is redundant.


#### `DSNote` should not be used in production

##### Severity: Gas Optimization

##### Context:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L674

##### Description

The `RubiconMarket` contract inherits `DSNote`, which is a simple contract for debugging logs. This contract is not intended to be used in production and should be removed.