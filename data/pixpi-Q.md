| ID    | Issue                                                                            | Instances |
| ----- | :------------------------------------------------------------------------------- | :-------: |
| QA-1  | Specify a fixed `pragma` version                                                 |     2     |
| QA-2  | Use a more recent version of solidity                                            |     -     |
| QA-3  | Missing `NatSpec` comments                                                       |     -     |
| QA-4  | Add explicit `private` visibility modifier to the variables                      |     3     |
| QA-5  | Use scientific notation (`1e18`) rather than exponentiation (`10**18`)           |     4     |
| QA-6  | Constants should be defined rather than using magic numbers                      |     6     |
| QA-7  | Unusued functions                                                                |     2     |
| QA-8  | Inconsistent use of `uint` / `uint256` type                                      |    10     |
| QA-9  | Missing checks for the input arguments in the `batchRequote()` function          |     1     |
| QA-10 | Consider adding checks for the input arguments of the `calculateFee()` function  |     -     |
| QA-11 | Consider adding a check for the input arguments of the `claimRewards()` function |     1     |
| QA-12 | Refactor `_supply()` in the `Position` contract                                  |     1     |

---

[QA-1] Specify a fixed `pragma` version

By locking the pragma version, developers can ensure that their code will work as expected with a known version of the Solidity compiler. This can make it easier to maintain and upgrade Solidity code over time, as new versions of the language are released.

https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/

2 instances in 2 files:

```solidity
File: contracts/RubiconMarket.sol
2:  pragma solidity ^0.8.9;
```

```solidity
File: contracts/periphery/BathBuddy.sol
2:  pragma solidity ^0.8.0;
```

[RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)

[BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)

---

## [QA-2] Use a more recent version of solidity

Consider switching to Solidity v.0.8.17 for all contracts in scope. Starting from that version the compiler has more efficient overflow checks for multiplication.

Additionally, removing the need for the DSMath library can improve code readability.

---

## [QA-3] Missing `NatSpec` comments

Many functions lack `NatSpec` comments, and it is recommended to enhance their documentation by adding them.

---

## [QA-4] Add explicit `private` visibility modifier to the variables

Explicitly declaring variables as `private` is a good programming practice that can enhance the security, readability, and maintainability of the contract code.

https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/visibility/

3 instances in 1 file:

```solidity
File: contracts/RubiconMarket.sol

74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;

224:    bool locked;
```

[RubiconMarket.sol#L74-75](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75")

[RubiconMarket.sol#L224](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L224)

---

## [QA-5] Use scientific notation (`1e18`) rather than exponentiation (`10**18`)

While the compiler knows to optimize away the exponentiation, it’s still better coding practice to use idioms that do not require compiler optimization, if they exist.

4 instances in 2 files:

```solidity
File: contracts/RubiconMarket.sol

74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;
```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

317:        _max = (_liq.mul(10 ** 18)).div(_price);

331:        ).div(10 ** 18);

```

[RubiconMarket.sol#L74-L75](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75)

[Position.sol#L317](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317)

[Position.sol#L331](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L331)

---

## [QA-6] Constants should be defined rather than using magic numbers

The following items are missing in the automated findings report.

4 instances in 2 file:

```solidity
File: contracts/RubiconMarket.sol
338:        uint256 fee = mul(spend, feeBPS) / 100_000;

583:        _amount -= mul(amount, feeBPS) / 100_000;

586:            _amount -= mul(amount, makerFee()) / 100_000;
```

```solidity
File: contracts/utilities/poolsUtility/Position.sol
587:        uint256 _leverageMax = WAD.mul(3);

```

[RubiconMarket.sol#L338](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338)

[RubiconMarket.sol#L583](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583)

[RubiconMarket.sol#L586](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586)

[Position.sol#L588](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L588)

---

## [QA-7] Unused functions

2 instances in 1 file:

```solidity
File: contracts/RubiconMarket.sol
487:    function kill(bytes32 id) external virtual {
488:        require(cancel(uint256(id)));
489:    }

756:    function kill(bytes32 id) external override {
757:        require(cancel(uint256(id)));
758:    }

```

[RubiconMarket.sol#L487-L489](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L487-L489)

[RubiconMarket.sol#L756-L758](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L756-L758)

---

## [QA-8] Inconsistent use of `uint` / `uint256` type

Although `uint` is alias for `uint256` type, using both of them in the project makes code less readable.

Therefore, it is advisable to stick with just one, either `uint` or `uint256`.

10 instances in 1 file:

```solidity
File: contracts/RubiconMarket.sol

785:        uint pos, //position to insert offer, 0 should be used if unknown

888:        uint[] calldata payAmts,

899:        for (uint i = 0; i < payAmts.length; i++) {

890:        uint[] calldata buyAmts,

910:    function batchCancel(uint[] calldata ids) external {
911:        for (uint i = 0; i < ids.length; i++) {

918:        uint[] calldata ids,
919:        uint[] calldata payAmts,

921:        uint[] calldata buyAmts,

924:        for (uint i = 0; i < ids.length; i++) {

```

[RubiconMarket.sol#L785](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L785)

---

## [QA-9] Missing checks for the input arguments in the `batchRequote()` function

The `batchRequote()` function should implement the checks for array lengths, like was done in the `batchOffer()` function.

[RubiconMarket.sol#L917-L933](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933)

---

## [QA-10] Consider adding checks for the input arguments of the `calculateFee()` function

For instance:

- Check that the length of `tokenAmounts` array is greater than zero.
- Check that `feeType` is not zero.
- Check that `feeValue` is not greater than or equal to `feeType`.

[FeeWrapper.sol#L28-L44](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L28-L44)

---

## [QA-11] Consider adding a check for the input arguments of the `claimRewards()` function

Check that the input arrays have equal length.

```solidity
File: contracts / BathHouseV2.sol +
  require(buddies.length == rewardsTokens.length);
```

[BathHouseV2.sol#L115-L128](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L128)

---

## [QA-12] Refactor `_supply()` in the `Position` contract

The refactored code replaces the `assembly` block with a single line of code that calculates the `_bathTokenAmount` value by subtracting the input value `_initBathTokenAmount` from the current value `_currentBathTokenAmount`. This simplifies the code, makes it more readable, and reduces the gas cost.

### Recommendation code:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
-367:        assembly {
-368:            switch _initBathTokenAmount
-369:            case 0 {
-370:                _bathTokenAmount := _currentBathTokenAmount
-371:            }
-372:            default {
-373:                _bathTokenAmount := sub(
-374:                    _currentBathTokenAmount,
-375:                    _initBathTokenAmount
-376:                )
-377:            }
-378:        }

+     _bathTokenAmount = _currentBathTokenAmount - _initBathTokenAmount
```

[Position.sol#L367-L378](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L367-L378)

---
