## 1 . Redundant check of msg.sender != address(0) :-

Address(0) cannot call so msg.sender will never equal to address(0)

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L97

## 2 . Constructor can declare as payable to save gas :-
If constructor declare as payable it prevents Opcode which were used to check "msg.value == 0" 

saves 130 gas.

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54

## 3 . Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.
saves 42 per access


## 4 