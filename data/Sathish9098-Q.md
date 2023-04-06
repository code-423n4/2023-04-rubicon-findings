## LOW FINDINGS 

##

## [L-1] MIXING AND OUTDATED COMPILER

### Impact 
Mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues

The minimum required version must be 0.8.19 otherwise, contracts will be affected by the following important bug fixes

- 0.8.14:
ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
Override Checker: Allow changing data location for parameters only when overriding external functions.
- 0.8.15
Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
Yul Optimizer: Keep all memory side-effects of inline assembly blocks.
- 0.8.16
Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.
- 0.8.17
Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.
Apart from these, there are several minor bug fixes and improvements

FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

```solidity
2: pragma solidity ^0.8.9;
```

FILE: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

```solidity
2: pragma solidity 0.8.17;
```
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

```solidity
2: pragma solidity ^0.8.0;
```
## Recommended Mitigation:
Use More recent version of solidity 



##

## NON CRITICAL FINDINGS

##

## [NC-1] Named imports can be used

#### CONTEXT

ALL CONTRACTS 

It’s possible to name the imports to improve code readability. 

E.g. import "@openzeppelin/contracts/token/ERC20/IERC20.sol"; can be rewritten as import {IERC20} from “import “@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol”;

> Example 

FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

```solidity 
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "./compound-v2-fork/InterestRateModel.sol";
import "./compound-v2-fork/CErc20Delegator.sol";
import "./compound-v2-fork/Comptroller.sol";
import "./compound-v2-fork/Unitroller.sol";
import "./periphery/BathBuddy.sol";
```
### Recommended Mitigation

Name the imports to all contract scopes to improve code readability

##

## [NC-2] Remove commented out code

CONTEXT
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

it's generally considered good practice to remove commented out code from a codebase once it's determined that it's no longer needed or relevant. This can help keep the codebase clean, readable, and maintainable, and can help prevent confusion or errors in the future

##

## [NC-3] Test environment comments and codes should not be in the main version

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

5: // import "hardhat/console.sol";

```














NC-1	Missing checks for address(0) when assigning values to address state variables	5
NC-2	require() / revert() statements should have descriptive reason strings	45
NC-3	Return values of approve() not checked	4
NC-4	TODO Left in the code	14
NC-5	Event is missing indexed fields	17
NC-6	Constants should be defined rather than using magic numbers	1
NC-7	Functions not used internally could be marked external	10

L-1	abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()	6
L-2	Do not use deprecated library functions	1
L-3	Initializers could be front-run	2
L-4	Unsafe ERC20 operation(s)	20