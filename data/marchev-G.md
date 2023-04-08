# Gas Report

### Summary

|     | Issue | Instances | Total Gas Saved |
| --- | --- | --- | --- |
| [G-01] | `ds-math` usage is unnecessary | 2   | 4840 |
| [G-02] | Use a more recent version of solidity | 1   | -   |
| [G-03] | For loop post condition increment can be made unchecked | 6   | 408 |
| [G-04] | Use custom errors instead of `revert`/`require` | 111 | 5550 |
| [G-05] | Division by two should use bit shifting | 15  | 300 |

The reported issues constitute **135 instances** and **11 098 gas** saved.

## Details

### [G-01] `ds-math` usage is unnecessary

The `RubiconMarket.sol` contract uses the `ds-math` library which aims to prevent integer under/overflows. But this is unnecessary since the contract uses Solidity 0.8.9 which automatically takes care of integer under-/overflows.

**Extra gas cost:**

| Operation | Additional gas usage | Instances | Total extra gas used |
| --- | --- | --- | --- |
| `add` | 70  | 13  | 910 |
| `sub` | 47  | 7   | 705 |
| `mul` | 129 | 16  | 3225 |
| **Total:** |     |     | **4840** |

*There are 2 instances of this issue:*

- `constracts/RubiconMarket.sol`
  
- `contracts/utilities/poolsUtility/Position.sol`
  

### [G-02] Use a more recent version of solidity

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value.

*There is 1 instances of this issue:*

- `contracts/RubiconMarket.sol`
  
  - ```solidity
    pragma  solidity ^0.8.9;
    ```
    

### [G-03] For loop post condition increment can be made unchecked

The for loop post condition (`i++`) involves checked arithmetic, which is not required. `i` is always less than `type(uint256).max`. Overflow/underflow checks are by default added by `solc`. Thus, some gas optimizations could be achieved by using unchecked arithmetic:

```solidity
for (uint256 i = 0; i < length;) {
    // for loop logic goes here
    unchecked {
        return i + 1;
    }
}
```

For Solidity versions `>= 0.8.2` an inline function could be used for improved readability:

````solidity
```solidity
for (uint256 i = 0; i < length; i = unchecked_increment(i)) {
    // for loop logic goes here
}

function unchecked_increment(uint256 i) pure returns (uint256) {
    unchecked {
        return i + 1;
    }
}

```
````

**Extra gas cost:** ~68 per each iteration.

*There are 6 instances of this issue:*

| File | Instances | Links |
| --- | --- | --- |
| `contracts/RubiconMarket.sol` | 3   | [899](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899), [911](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911), [924](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924) |
| `contracts/BathHouseV2.sol` | 1   | [122](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L122) |
| `contracts/V2Migrator.sol` | 1   | [31](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L31) |
| `contracts/utilities/poolsUtility/Position.sol` | 1   | [158](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L158) |

### [G-04] Use custom errors instead of `revert`/`require`

Starting from Solidity v0.8.4, there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. This affects both deploy and runtime gas costs.

Using custom errors saves gas (~50 gas) every time a condition is met comapred to revert string or require. More details could be found [on the Solidity blog here](https://blog.soliditylang.org/2021/04/21/custom-errors/#example).

This also allows to provide parameters to the custom errors.

*Example:*

```solidity
// Before
revert("Insufficient funds.");
require(someCondition, "Not permissioned.");

// After
if (someCondition) {
    revert InsufficientBalance();
}
```

**Extra gas cost:** ~50 per hit (during runtime) instance.

*There are 111 instances of this issue:*

| File | Instances |
| --- | --- |
| `contracts/RubiconMarket.sol` | 72  |
| `contracts/V2Migrator.sol` | 3   |
| `contracts/BathHouseV2.sol` | 6   |
| `contracts/periphery/BathBuddy.sol` | 8   |
| `contracts/utilities/poolsUtility/Position.sol` | 18  |
| `contracts/utilities/FeeWrapper.sol` | 4   |

### [G-05] Division by two should use bit shifting

`<x> / 2` is the same as `<x> >> 1`. While the compiler uses the `SHR` opcode to accomplish both, the version that uses division incurs an overhead of [**20 gas**](https://gist.github.com/IllIllI000/ec0e4e6c4f52a6bca158f137a3afd4ff) due to `JUMP`s to and from a compiler utility function that introduces checks which can be avoided by using `unchecked {}` around the division by two.

The `DSMath` library in `contracts/RubiconMarket.sol` contains 4 functions which contain division by two:

- `wmul(uint256 x, uint256 y)`
  
- `rmul(uint256 x, uint256 y)`
  
- `wdiv(uint256 x, uint256 y)`
  
- `rdiv(uint256 x, uint256 y)`
  

Those functions are called in secveral places throughout the codebase.

*There are 15 instances of this issue:*

| File | Instances | Links |
| --- | --- | --- |
| `contracts/RubiconMarket.sol` | 10  | [1045](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1045), [1056](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1056), [1058](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1058), [1085](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1085), [1098](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1098), [1100](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1100), [1141](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1141), [1143](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1143), [1174](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1174), [1176](https://github.com/code-423n4/2023-04-rubicon/bob/main/contracts/RubiconMarket.so#1176) |
| `contracts/utilities/poolsUtility/Position.sol` | 5   | [264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L264), [534](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L534), [548](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L548), [552](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L552), [555](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L555) |