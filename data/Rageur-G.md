## GAS-1: <X> += <Y> costs more gas than <X> = <X> + <Y> for state variables

### Description

Using the addition operator instead of plus-equals saves gas.

### Affected file

* Position.sol (Line: 431)

## GAS-2: <X> <= <Y> costs more gas than <X> < <Y> + 1

### Description

In Solididy, the opcode 'less or equal' doesn't exist. So the EVM will translate this by two distinct operation, first the inferior, and then the equal which cost more gas then a strict less.

### Affected file

* BathBuddy.sol (Line: 195)
* Position.sol (Line: 541, 592, 596)
* RubiconMarket.sol (Line: 47, 51, 59, 63, 67, 71, 835, 1049, 1065, 1089, 1266, 1327)

## GAS-3: Functions guaranteed to revert when called by normal users can be marked payable

### Description

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

### Affected file

* BathBuddy.sol (Line: 168, 191, 232)
* BathHouseV2.sol (Line: 60)
* Position.sol (Line: 93, 107, 210, 226, 242)

## GAS-4: Internal functions only called once can be inlined to save gas

### Description

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

### Affected file

* BathHouseV2.sol (Line: 137)
* FeeWrapper.sol (Line: 60, 76, 93, 108)
* Position.sol (Line: 251, 272, 280, 322, 336, 343, 382, 401, 421, 426, 454, 475, 526)
* RubiconMarket.sol (Line: 35, 568, 1225, 1273, 1362)

## GAS-5: Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

### Description

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

### Affected file

* BathBuddy.sol (Line: 52, 53, 54, 55, 56, 59, 61)
* BathHouseV2.sol (Line: 20, 21)
* RubiconMarket.sol (Line: 692, 693, 694)

## GAS-6: Not using the named return variables when a function returns, wastes deployment gas

### Description

It is not necessary to have both a named return and a return statement.

### Affected file

* RubiconMarket.sol (Line: 58, 62, 66, 70, 276, 280, 284, 496, 620, 873)

## GAS-7: Replace modifier with function

### Description

Modifiers make code more elegant, but cost more than normal functions.

### Affected file

* BathBuddy.sol (Line: 87, 94, 104, 247)
* BathHouseV2.sol (Line: 26)
* RubiconMarket.sol (Line: 30, 245, 251, 260, 264, 597, 603, 610, 643, 719)

## GAS-8: Should use arguments instead of state variable

### Description

Using function's parameter cost less gas then a state variable.

### Affected file

* RubiconMarket.sol (Line: 27)

## GAS-16: Splitting require() statements that use && saves gas

### Description

Saves 16 gas per instance. If you’re using the Optimizer at 200, instead of using the && operator in a single require statement to check multiple conditions, multiple require statements with 1 condition per require statement should be used to save gas.

### Affected file

* BathBuddy.sol (Line: 95)
* RubiconMarket.sol (Line: 253, 612, 721, 893, 940, 1412)

## GAS-9: The result of a function call should be cached rather than re-calling the function

### Description

External calls are expensive. Consider caching.

### Affected file

* Position.sol (Line: 138, 195, 548, 552)
* RubiconMarket.sol (Line: 254, 255, 613, 615, 723, 725, 1239, 1239)

## GAS-10: Usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

### Description

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.

### Affected file

* BathHouseV2.sol (Line: 82, 137)
* RubiconMarket.sol (Line: 321, 322, 491, 519, 520, 534, 564, 624, 625, 733, 752, 1053, 1061, 1093, 1103)

## GAS-11: Using Solidity version 0.8.17 will provide an overall gas optimization

### Description

Using at least 0.8.10 will save gas due to skipped extcodesize check if there is a return value.

### Affected file

* BathBuddy.sol (Line: 2)
* RubiconMarket.sol (Line: 2)

## GAS-12: Using immutable on variables that are only set in the constructor and never after (2.1k gas per var)

### Description

Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving SLOAD.
Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around 20 000 gas per variable) and replace the expensive storage-reading operations (around 2100 gas per reading) to a less expensive value reading (3 gas).

### Affected file

* Position.sol (Line: 44, 45, 46, 47)

## GAS-13: Using private rather than public for constants saves gas

### Description

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table.

### Affected file

* RubiconMarket.sol (Line: 74, 75, 232)