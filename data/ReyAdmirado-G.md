| | issue |
| ----------- | ----------- |
| 1 | [State variables only set in the constructor should be declared immutable.](#1-state-variables-only-set-in-the-constructor-should-be-declared-immutable) |
| 2 | [Structs can be packed into fewer storage slots](#2-structs-can-be-packed-into-fewer-storage-slots) |
| 3 | [state variables can be packed into fewer storage slots](#3-state-variables-can-be-packed-into-fewer-storage-slots) |
| 4 | [expressions for constant values such as a call to keccak256(), should use immutable rather than constant](#4-expressions-for-constant-values-such-as-a-call-to-keccak256-should-use-immutable-rather-than-constant) |
| 5 | [Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate](#5-multiple-addressid-mappings-can-be-combined-into-a-single-mapping-of-an-addressid-to-a-struct-where-appropriate) |
| 6 | [state variables should be cached in stack variables rather than re-reading them from storage](#6-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage-ones-not-caught-by-c4udit) |
| 7 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#7-x--y-costs-more-gas-than-x--x--y-for-state-variables) |
| 8 | [not using the named return variables when a function returns, wastes deployment gas](#8-not-using-the-named-return-variables-when-a-function-returns-wastes-deployment-gas) |
| 9 | [can make the variable outside the loop to save gas](#9-can-make-the-variable-outside-the-loop-to-save-gas) |
| 10 | [splitting require() statements that use `&&` saves gas](#10-splitting-require-statements-that-use--saves-gas) |
| 11 | [require() or revert() statements that check input arguments should be at the top of the function](#11-require-or-revert-statements-that-check-input-arguments-should-be-at-the-top-of-the-function) |
| 12 | [internal functions only called once can be inlined to save gas](#12-internal-functions-only-called-once-can-be-inlined-to-save-gas) |
| 13 | [usage of uint/int smaller than 32 bytes (256 bits) incurs overhead](#13-usage-of-uintint-smaller-than-32-bytes-256-bits-incurs-overhead) |
| 14 | [ Ternary over if ... else](#14-ternary-over-if--else) |
| 15 | [public functions not called by the contract should be declared external instead](#15-public-functions-not-called-by-the-contract-should-be-declared-external-instead) |
| 16 | [should use arguments instead of state variable](#16-should-use-arguments-instead-of-state-variable) |
| 17 | [Avoid contract existence checks by using solidity version 0.8.10 or later](#17-avoid-contract-existence-checks-by-using-solidity-version-0810-or-later) |
| 18 | [state var is defined but not used anywhere but](#18-state-var-is-defined-but-not-used-anywhere-but) |
| 19 | [state variable should be declared as constant which saves lots of gas](#19-state-variable-should-be-declared-as-constant-which-saves-lots-of-gas) |
| 20 | [instead of calculating a statevar with keccak256() every time the contract is made pre calculate them before and only give the result to a constant](#20-instead-of-calculating-keccak256-every-time-the-contract-is-made-pre-calculate-them-before-and-only-give-the-result-to-a-constant) |
| 21 | [Functions guaranteed to revert when called by normal users can be marked `payable`](#21-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable) |
| 22 | [Optimize names to save gas](#22-optimize-names-to-save-gas) |
| 23 | [Non-strict inequalities are cheaper than strict ones](#23-non-strict-inequalities-are-cheaper-than-strict-ones) |
| 24 | [Setting the constructor to payable](#24-setting-the-constructor-to-payable) |
| 25 | [instead of assigning to zero use `delete`](#25-instead-of-assigning-to-zero-use-delete) |
| 26 | [part of the code can be pre calculated](#26-part-of-the-code-can-be-pre-calculated) |


## 1. State variables only set in the constructor should be declared immutable.

Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmaccess (100 gas) with a PUSH32 (3 gas).

- [Position.sol#L44](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44)
- [Position.sol#L45](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L45)
- [Position.sol#L46](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L46)
- [Position.sol#L47](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L47)


## 2. Structs can be packed into fewer storage slots

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

with putting `isActive` which is a bool near one of the variables that is address type, this struct will use one less storage slot
- [Position.sol#L39](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L39)

put `selector` and `target` together so they can be in one slot instead of two. saves one slot for this struct
- [FeeWrapper.sol#L](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L)


## 3. state variables can be packed into fewer storage slots

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper.

`locked`(#L224) and `feeTo`(#L230) can be put together into one slot if they are declared near each other
- [RubiconMarket.sol#L224](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224)


## 4. expressions for constant values such as a call to keccak256(), should use immutable rather than constant

- [RubiconMarket.sol#L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232)


## 5. Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. 

3 mappings can be turned here. wont save slot but the accesses gonna be cheaper
- [RubiconMarket.sol#L692-L694](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L692-L694)

7 mapping can be turned here. wont save slot but the accesses gonna be way cheaper and save huge amounts of gas because these mappings are accessed in the same function lots of times
- [BathBuddy.sol#L52-L61](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52-L61)


## 6. state variables should be cached in stack variables rather than re-reading them from storage (ones not caught by c4udit)

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 


`_rank[id].delb` can be cached before the require(#L940) because it is read twice inside it. caching it will reduce one complex SLOAD.
- [RubiconMarket.sol#L942-L943](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L942-L943)

if the provided reward isnt too high, `rewardsDuration[address(rewardsToken)]` will be read twice so we can cache it before the require(#L217) because we know its usaully gonna pass. 
- [BathBuddy.sol#L217-L226](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L217-L226)

`rewardPerToken(token)` answer can be cached and instead of giving it to `rewardsPerTokensStored[token]` at first give to a stack variable because we need to give the same answer to `userRewardsPerTokenPaid[token][account]` as well. so give the answer to a stack variable first and give that two these two storage variables to use one less complex storage read.
- [BathBuddy.sol#L248-L253](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L248-L253)

`lastPositionId` can be cache before #L416 because its being read twice after. reduces one SLOAD
- [Position.sol#L416-L418](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L416-L418)

    ## the ones down here are inside different functions they just look the same. 

`offers[offerId].buy_amt` is being read from storage 3 times if the if statement(#L1043) doesnt succeed, so there is high chance that 2 less complex SLOADs gonna be used every iteration of the while loop if we cache it beforehands
- [RubiconMarket.sol#L1043-L1063](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1043-L1063)

`offers[offerId].pay_amt` is being read from storage at least twice if the if statement(#L1043) doesnt succeed, so there is a good chance that at least one less complex SLOADs gonna be used every iteration of the while loop if we cache it beforehands
- [RubiconMarket.sol#L1043-L1063](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1043-L1063)

`offers[offerId].pay_amt` is being read from storage 3 times if the if statement(#L1083) doesnt succeed, so there is high chance that 2 less complex SLOADs gonna be used every iteration of the while loop if we cache it beforehands
- [RubiconMarket.sol#L1083-L1105](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1083-L1105)

cache `offers[offerId].buy_amt` before the while loop(#L1130) to stop (2*iter) times complex SLOADs. instead cache it before the loop and use the cached version
- [RubiconMarket.sol#L1130-L1144](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1130-L1144)

cache `offers[offerId].pay_amt` before the while loop(#L1130) to stop (iter - 1) times complex SLOADs. instead cache it before the loop and use the cached version
- [RubiconMarket.sol#L1130-L1144](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1130-L1144)

cache `offers[offerId].pay_amt` before the while loop(#L1163) to stop (2*iter) times complex SLOADs. instead cache it before the loop and use the cached version
- [RubiconMarket.sol#L1163-L1177](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163-L1177)

cache `offers[offerId].buy_amt` before the while loop(#L1163) to stop (iter - 1) times complex SLOADs. instead cache it before the loop and use the cached version
- [RubiconMarket.sol#L1163-L1177](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163-L1177)


## 7. `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables
Using the addition operator instead of plus-equals saves gas (13 or 113 each dependant on the usage see [here](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8))

- [Position.sol#L431](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431)


## 8. not using the named return variables when a function returns, wastes deployment gas

- [RubiconMarket.sol#L58](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58)
- [RubiconMarket.sol#L62](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62)
- [RubiconMarket.sol#L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66)
- [RubiconMarket.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70)
- [RubiconMarket.sol#L276](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276)
- [RubiconMarket.sol#L280](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L280)
- [RubiconMarket.sol#L284](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L284)
- [RubiconMarket.sol#L491](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L491)
- [RubiconMarket.sol#L620](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620)
- [RubiconMarket.sol#L873](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L873)


## 9. can make the variable outside the loop to save gas

make the variable outside the loop and only give the value to variable inside

`assetBalance`
- [Position.sol#L160](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L160)


## 10. splitting require() statements that use `&&` saves gas

Instead of using operator && on a single require. Using a two require can save more gas.

- [RubiconMarket.sol#L1413](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1413)
- [Position.sol#L592](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L592)
- [Position.sol#L596](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L596)

2 instances in these
- [RubiconMarket.sol#L893](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893)
- [RubiconMarket.sol#L940](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L940)
- [BathBuddy.sol#L96](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L96) because this one is a modifier it will have more impact than others


## 11. require() or revert() statements that check input arguments should be at the top of the function

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting a Gcoldsload (2100 gas*) in a function that may ultimately revert in the unhappy case.

swap these lines
- [RubiconMarket.sol#L701-L702](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L701-L702)

first require should be last
- [BathHouseV2.sol#L68-L79](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L68-L79)


## 12. internal functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

- [RubiconMarket.sol#L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66)
- [RubiconMarket.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70)
- [RubiconMarket.sol#L568](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568)
- [RubiconMarket.sol#L1225](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1225)
- [RubiconMarket.sol#L1273](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1273)
- [RubiconMarket.sol#L1362](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1362)

- [BathHouseV2.sol#L137](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L137)

- [Position.sol#L251](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L251)
- [Position.sol#L272](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L272)
- [Position.sol#L280](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L280)
- [Position.sol#L322](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L322)
- [Position.sol#L336](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L336)
- [Position.sol#L382](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L382)
- [Position.sol#L401](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L401)
- [Position.sol#L421](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L421)
- [Position.sol#L426](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L426)
- [Position.sol#L454](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L454)
- [Position.sol#L475](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L475)
- [Position.sol#L526](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526)

- [FeeWrapper.sol#L60](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L60)
- [FeeWrapper.sol#L76](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76)
- [FeeWrapper.sol#L93](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93)
- [FeeWrapper.sol#L108](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L108)


## 13. usage of uint/int smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.
Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html Use a larger size then downcast where needed


## 14. Ternary over if ... else

Using ternary operator instead of the if else statement saves gas.

- [RubiconMarket.sol#L36-L40](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L36-L40)
- [RubiconMarket.sol#L876-L880](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L876-L880)
- [RubiconMarket.sol#L1186-L1191](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1186-L1191)

- [Position.sol#L176-L181](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L176-L181)

- [FeeWrapper.sol#L51-L55](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L51-L55)


## 15. public functions not called by the contract should be declared external instead

Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so. 

- [RubiconMarket.sol#L288](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288)
- [RubiconMarket.sol#L624](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624)
- [RubiconMarket.sol#L733](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733)
- [RubiconMarket.sol#L993](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L993)
- [RubiconMarket.sol#L998](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L998)
- [RubiconMarket.sol#L1010](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010)
- [RubiconMarket.sol#L1016](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016)
- [RubiconMarket.sol#L1049](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1049)


## 16. should use arguments instead of state variable

This will save 100 gas because it gets rid of a storage read and instead uses a argument that we already have which gives the same result

use `owner_` instead
- [RubiconMarket.sol#L27](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L27)

use `_rewardsDuration` instead 
- [BathBuddy.sol#L241](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L241)


## 17. Avoid contract existence checks by using solidity version 0.8.10 or later
Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

decimals()
- [BathHouseV2.sol#L148](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L148)


## 18. state var is defined but not used anywhere but 

even if they are for readability, consider making them comments instead

- [RubiconMarket.sol#L682](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682)
- [RubiconMarket.sol#L684](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L684)


## 19. state variable should be declared as constant which saves lots of gas

`buyEnabled` and `matchingEnabled` are always true so they can be changed to a constant or immutable. this will have 40000 deplyment cost save and accessing them will be 3 gas cost instead of 100
- [RubiconMarket.sol#L675](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675)
- [RubiconMarket.sol#L676](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676)


## 20. instead of calculating keccak256() every time the contract is made pre calculate them before and only give the result to a constant

- [RubiconMarket.sol#L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232)


## 21. Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier or require such as onlyOwner-admin is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

- [BathHouseV2.sol#L60](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60)

- [BathBuddy.sol#L168](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L168)
- [BathBuddy.sol#L191](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191)
- [BathBuddy.sol#L235](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L235)

- [Position.sol#L98](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L98)
- [Position.sol#L112](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L112)
- [Position.sol#L210](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210)
- [Position.sol#L226](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L226)
- [Position.sol#L242](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L242)


## 22. Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions.

See more [here](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92).
you can use this [tool](https://emn178.github.io/solidity-optimize-name/) to get the optimized version for function and properties signitures 


## 23. Non-strict inequalities are cheaper than strict ones

In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =).
consider replacing >= with the strict counterpart > and add `- 1` to the second side

- [RubiconMarket.sol#L51](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L51)
- [RubiconMarket.sol#L59](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L59)
- [RubiconMarket.sol#L67](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L67)
- [RubiconMarket.sol#L71](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L71)
- [RubiconMarket.sol#L63](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L63)
- [RubiconMarket.sol#L47](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L47)
- [RubiconMarket.sol#L835](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L835)
- [RubiconMarket.sol#L1108](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1108)
- [RubiconMarket.sol#L1049](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1049)
- [RubiconMarket.sol#L1065](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1065)
- [RubiconMarket.sol#L1089](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1089)
- [RubiconMarket.sol#L1266](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1266)
- [RubiconMarket.sol#L1327](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1327)

- [BathBuddy.sol#L195](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L195)
- [BathBuddy.sol#L218](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L218)

- [Position.sol#L289](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L289)
- [Position.sol#L541](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L541)higher impact because its checked every iter
- [Position.sol#L592](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L592)
- [Position.sol#L596](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L596) 2 instances


## 24. Setting the constructor to payable

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of msg.value == 0 and saves 13 gas on deployment with no security risks.

- [V2Migrator.sol#L30](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30)

- [Position.sol#L54](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54)


## 25. instead of assigning to zero use `delete` 

assigning to zero uses more gas than using `delete` , and they both assign variable to default value. so it is encouraged if the data is no longer needed use delete instead.

- [RubiconMarket.sol#L1449](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449)
- [RubiconMarket.sol#L1462](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462)

- [BathBuddy.sol#L181](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L181)


## 26. part of the code can be pre calculated

this part of the code can be pre calculated and given to the contract as a constant. this will result in less operations used while using a function which will result in less gas used.

`WAD / 2` can be pre calculated
- [RubiconMarket.sol#L78](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L78)

`RAY / 2` can be pre calculated
- [RubiconMarket.sol#L82](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L82)

