| ID    | Title |
| -------- | ------- |
| 01  | The call to the function `_hide()` in `RubiconMarket._buys()` should be wrapped in a require statement |
| 02 | `initialized` not enforced |
| 03 | Emit events before external calls |
| 04 | Some tokens might not implement the decimals function |
| 05 | `buyEnabled` is always true and cannot be toggled to false |
| 06 | The market cannot be closed |
| 07 | Can use libraries like OpenZeppelin or solmate for common functionalities like Ownable |
| 08 | Check if arrays length match when passing multiple arrays as function arguments |
| 09 | Add checks for address(0) in V2Migrator |
| 10 | Usage of `whenNotPaused` in `BathBuddy.getReward()` |
| 11 | Pragma float |
| 12 | Lack of lower and upper bound limit when setting the fee |
| 13 | Lack of checks-effects-interactions |
| 14 | Critical parameter changes should use two-step pattern |
| 15 | Missing event for parameter changes |
| 16 | Lack of old value for events related to parameter updates |
| 17 | Check for stale values on setter functions |
| 18 | Fields in a struct can be packed together  |
| 19 | Redundant check |
| 20 | Place require statements for functions arguments above require statements for state variables |
| 21 | Presence of require statement for calling _unsort in `RubiconMarket.cancel()` |
| 22 | Variable `amountsWithFee` doesn't contain the fee |
| 23 | Address should be payable |
| 24 | Lack of error message for require statement |
| 25 | Some error message could have a better description |
| 26 | One require statement could contain more than one validation |
| 27 | Consider using delete rather than assigning zero to clear values |
| 28 | Variable shadow |
| 29 | `feeTo` could be public instead of internal |
| 30 | `synchronized` modifier can be reused |
| 31 | Place each contract in a separate file |
| 32 | Usage of return named variables and explicit values |
| 33 | Can use ternary |
| 34 | Empty modifier, consider commenting why it's necessary |
| 35 | Unused return named variables |
| 36 | Interchangeable usage of snake case and camel case |
| 37 | Use leading underscore for internal functions consistently |
| 38 | Incorrect visibility |
| 39 | Events should be pascal cased instead of camel cased |
| 40 | Large multiples of ten should use scientific notation |
| 41 | Lack of spacing in comment |
| 42 | Fix typo |
| 43 | Increment and return can be done in one statement |
| 44 | Missing NAPSPEC for external functions |
| 45 | NATSPEC could contain description for function arguments and return values |
| 46 | Order of functions |
| 47 | Modify comments to fit into 120 character or less |
| 48 | Leading underscore should be use for private variables instead of public ones |
| 49 | Reuse newly allocated variable |
| 50 | Marking the variable as private will make it more explicit |
| 51 | Interchangeable usage of uint and uint256 |
| 52 | Struct can be initialized in one statement |
| 53 | Use scientific notation rather than exponentiation |
| 54 | Named imports can be used |
| 55 | Large or complicated contracts should implement fuzzing tests |

# [01] The call to the function `_hide()` in `RubiconMarket._buys()` should be wrapped in a require statement

`_hide()` might return false is the offer is not found in the unsorted list. This should result in a revert while calling `RubiconMarket._buys()`. Otherwise, passing an invalid id can result in silent failures/data corruption.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1190

# [02] `initialized` not enforced

Functions like `RubiconMarket.buy()`, `RubiconMarket.cancel()` and `RubiconMarket.offer()` should only accessible after the contract is initialized by the proxy. Currently it's possible to create/buy/cancel before the `RubiconMarket.initialize()` function is executed. 

Impact of calling buy/cancel/offer before initialization is that variables like `feeTo` and `owner` won't have the proper value.

Consider enforcing `initialized` to be set to true when running buy/cancel/offer. This could be done with a function modifier.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700-L716

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314-L484

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L562

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L452-L485

# [03] Emit events before external calls

Multiple functions in the project emit an event as the last statement. Wherever possible, consider emitting events before external calls. In case of reentrancy, funds are not at risk (for external call + event ordering), however emitting events after external calls can damage frontends and monitoring tools in case of reentrancy attacks.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L374-L384

# [04] Some tokens might not implement the decimals function

Consider adding the placeholder 1e18 for underlying tokens that don't implement `.decimals()`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L148

# [05] `buyEnabled` is always true and cannot be toggled to false

The variable `buyEnabled` in `RubiconMarket` will have the default value true and it will not be able to be disabled. Consider applying the default value of false and also adding a setter function so that buy functionalities can be disabled.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1184

# [06] The market cannot be closed

`RubiconMarket.isClose()` will always return false.

Consider adding the implementation to allow setting this value and allow closing the market.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L622

# [07] Can use libraries like OpenZeppelin or solmate for common functionalities like Ownable

Using libraries instead of custom implementation for common functionalities increases security.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L22-L42

# [08] Check if arrays length match when passing multiple arrays as function arguments

Consider adding the following check on `V2Migrator.constructor()`:

```
require(bathTokenV2.length == bathTokensV2.length);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30-L35

# [09] Add checks for address(0) in V2Migrator

Consider adding a check in `V2Migrator.constructor` to prevent the key or the value of the `v1toV2Pools` receiving the address(0), since it won't be possible to be modified after deployment.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30-L35

# [10] Usage of `whenNotPaused` in `BathBuddy.getReward()`

`BathBuddy.getReward()` is using the `whenNotPaused` modifier inherited from OpenZeppelin, but for `BathBuddy.sol` and `BathHouseV2.sol` there's no functionality to actually pause it. Consider adding the `setPause` function for `BathBuddy.sol` and `BathHouseV2.sol`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L175

# [11] Pragma float

Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version.

Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

# [12] Lack of lower and upper bound limit when setting the fee

If the admin sets values too high or too small, users might not be able to interact with the market due to DOS conditions for functions where the fee is calculated.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1469

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L578-L588

# [13] Lack of checks-effects-interactions

Consider updating state variables before executing external calls.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L233-L238

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L426-L434

# [14] Critical parameter changes should use two-step pattern

Lack of two-step procedure for critical operations leaves them error-prone.

Consider adding a two-steps pattern on critical changes to avoid mistakenly transferring ownership of roles or critical functionalities to the wrong address.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28

# [15] Missing event for parameter changes

Adding an event will facilitate offchain monitoring.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1480

# [16] Lack of old value for events related to parameter updates

Events that mark critical parameter changes should contain both the old and the new value

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955-L962

# [17] Check for stale values on setter functions

Add a check ensuring that the new value if different than the current value to avoid emitting unnecessary events.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955-L962

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1480

# [18] Fields in a struct can be packed together 

Fields can be packed together. Also, if pay_amt and buy_amt where refactored to uint128 (they are downcasted to uin128 in some functions), packing it together would also save gas. e.g.

```
struct OfferInfo {
    uint64 timestamp;
    uint128 pay_amt;
    uint128 buy_amt;
    ERC20 pay_gem;
    ERC20 buy_gem;
    address recipient;
    address owner;
}
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L234-L242

# [19] Redundant check

Currently `BathHouseV2._bathify()` is only called by `BathHouseV2.createBathToken()` and `underlying` is already checked against address(0). E.g. the following statement `require(_underlying != address(0))` can be removed inside `BathHouseV2._bathify()`. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L144

# [20] Place require statements for functions arguments above require statements for state variables

Functions arguments should validated on the top of the function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L68-L79

# [21] Presence of require statement for calling _unsort in `RubiconMarket.cancel()`

Wrapping _unsort with require has no effect since `RubiconMarket._unsort()` cannot return false. Consider converting `require(_unsort(id))` to `_unsort(id)`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L877

# [22] Variable `amountsWithFee` doesn't contain the fee

`amountsWithFee` is deducting the fees from the totalAmounts. The comment on [Test3rdPartyProtocol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/Test3rdPartyProtocol.sol#L48) indicates this variable will be the value with the feeds deducted. Consider renaming the variable to `amountsWithFeeDeducted` or similar.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L42

# [23] Address should be payable

`_params.target` should be casted to payable.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L82-L84

# [24] Lack of error message for require statement

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L519-L520

# [25] Some error message could have a better description

The message "not enough ETH sent" could be converted to "ETH sent differs from totalAmount" or similar, because it's possible for someone to send a msg.value bigger than totalAmount.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115

Furthermore, instead of using "spend is not an int", the message could be "spend is not a uint128", since technically the value will be an integer.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L321-L322

# [26] One require statement could contain more than one validation

For example, consider converting.

```
require(pay_amt > 0);
require(pay_gem != ERC20(address(0)));
```

To

```
require(pay_amt > 0 && pay_gem != ERC20(address(0)));
```

This can also save gas.

This approach is recommended when the statements have the same error message.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L521-L522

# [27] Consider using delete rather than assigning zero to clear values

The delete keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462

# [28] Variable shadow

Consider renaming the argument `owner` in `RubiconMarket.offer()` because it's currently being shadowed by `DSAuth.owner`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L808

# [29] `feeTo` could be public instead of internal

Since the function has a getter function, it could be converted from internal to public.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L230

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1482-L1484

# [30] `synchronized` modifier can be reused

Instead of checking the `locked` variable on the top of the function, the `synchronized` modifier can be reused to improve code reusability.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L859

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L834

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

# [31] Place each contract in a separate file

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

# [32] Usage of return named variables and explicit values

Some functions return named variables, others return explicit values.

Following function returns an explicit value.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314-L448

Following function return returns a named variable.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L452-L485

Consider adopting the same approach throughout the codebase to improve the explicitness and readability of the code.

# [33] Can use ternary

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L36-L41

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L51-L55

# [34] Empty modifier, consider commenting why it's necessary

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260-L262

# [35] Unused return named variables

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276-L286

# [36] Interchangeable usage of snake case and camel case

Camel case is generally recommended for solidity.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251-L252

# [37] Use leading underscore for internal functions consistently

Following internal function has leading underscore:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L251-L258

Following functions doesn't have leading underscore:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L125-L130

# [38] Incorrect visibility

`FeeWrapper.calculateFee()` can be modified to pure and `Position._borrowLimit()` can be modified to view. These warnings are generate by hardhat when compiling the project.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28-L44

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526-L583

# [39] Events should be pascal cased instead of camel cased

E.g. consider converting emitTake to EmitTake.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L398-L407

# [40] Large multiples of ten should use scientific notation

Using scientific notation for large multiples of ten will improve code readability, e.g. 100_000 = 1e5.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L338

# [42] Lack of spacing in comment

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L324

# [43] Fix typo

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L886

# [44] Increment and return can be done in one statement

Consider converting to:

```
function _next_id() internal returns (uint256) {
    return ++last_offer_id;
}
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L569-L570

# [45] Missing NAPSPEC for external functions

Consider adding NATSPEC on all external/public functions to improve documentation

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1069

# [46] NATSPEC could contain description for function arguments and return values

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28-L44

# [47] Order of functions

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) recommends writing view functions bellow mutative functions inside a grouping.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28-L56

# [48] Modify comments to fit into 120 character or less

The solidity [documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) recommends a maximum of 120 characters.

Consider adding a limit of 120 characters or less to prevent large lines. For comments, please refer to this [example](https://github.com/ProjectOpenSea/seaport/blob/main/contracts/Seaport.sol#L87-L88).

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L312

# [49] Leading underscore should be use for private variables instead of public ones

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691-L696

# [50] Reuse newly allocated variable

When assigning an existing variable to a new variable, it's a best practice to reuse the new variable from now on, e.g. consider applying the following refactor on the following instance:

```
owner = msg.sender;
emit LogSetOwner(owner);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L707-L708

# [51] Marking the variable as private will make it more explicit

Marking as private will make it more explicit than letting it have the default.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224

# [52] Interchangeable usage of uint and uint256

Consider using only one approach, e.g. only uint256.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L783

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L785

# [53] Struct can be initialized in one statement

Instead of assigning the values in multiple statements, the assignments can be done in a single statement.

```
OfferInfo memory info = OfferInfo({
    pay_amt = pay_amt;
    pay_gem = pay_gem;
    buy_amt = buy_amt;
    buy_gem = buy_gem;
    recipient = recipient;
    owner = owner;
    timestamp = uint64(block.timestamp);
})
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L527-L534

# [54] Use scientific notation rather than exponentiation

Scientific notation can be used for better code readability, e.g. using `10e18` instead of `10**18`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75

# [55] Named imports can be used

It's possible to name the imports to improve code readability. E.g. `import "@openzeppelin/contracts/token/ERC20/ERC20.sol";` can be rewritten as `import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L11

# [56] Large or complicated contracts should implement fuzzing tests

Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement fuzzing tests.

Fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. 

Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.

