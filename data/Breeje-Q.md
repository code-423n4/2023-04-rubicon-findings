# QA Report

## Non-Critical Issues
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [N-01] | Function state mutability can be restricted to pure | 1 |
| [N-02] | Function state mutability can be restricted to view | 1 |
| [N-03] | `Position` declaration shadows an existing declaration | 1 |

| Total Non-Critical Issues | 3 |
|:--:|:--:|

### [N-01] Function state mutability can be restricted to pure

Given that the `calculateFee` doesn't read or modify the variables of the state, it's mutability should be changed to pure.

*Instance (1):*
```solidity
File: FeeWrapper.sol

28:   function calculateFee(

```

### [N-02] Function state mutability can be restricted to view

Given `_borrowLimit` is not modifying any state variable, it's mutability should be restricted to view.

*Instance (1):*
```solidity
File: Position.sol

526:    function _borrowLimit(

```

### [N-03] `Position` declaration shadows an existing declaration

Struct Name is shadowing the Contract name Position. Consider changing it's name.

*Instance (1):*
```solidity
File: Position.sol

33:    struct Position {

```