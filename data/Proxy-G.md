### Gas Optimizations List

| Number | Optimization Details                                                                              | Instances |
| :----: | :------------------------------------------------------------------------------------------------ | :-------: |
| [G-01] | Use multiple require() statments insted of require(expression && expression && ...)               |     2     |
| [G-02] | Optimal Comparison                                                                                |     4     |
| [G-03] | Don't use SafeMath when using solidity >= 0.8.0                                                   |    19     |
| [G-04] | Use `calldata` instead of `memory` for function arguments that do not get mutated.                |     4     |
| [G-05] | Pack structs                                                                                      |     1     |
| [G-06] | Use Short Revert Strings                                                                          |     1     |
| [G-07] | Mark functions as payable (with discretion)                                                       |     9     |
| [G-08] | Use custom errors instead of string error messages                                                |    11     |
| [G-09] | Mark storage variables as `constant` or `immutable` if they never change or delete if never used. |     6     |
| [G-10] | Use assembly for math (add, sub, mul, div)                                                        |     1     |
| [G-11] | `unchecked{++i}` instead of `++i` / `i++` or use assembly when applicable                         |    10     |

Total 11 issues

### [G-01] Use multiple require() statments insted of require(expression && expression && ...)

You can safe gas by breaking up a require statement with multiple conditions, into multiple require statements with a single condition.

Lines

- [Position.sol:591](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L591-L594)
- [Position.sol:595](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L595-L598)

Code Snippet

```js
function _leverageCheck(uint256 _leverage, bool _long) internal pure {
    uint256 _wad = WAD;
    uint256 _leverageMax = WAD.mul(3);

    _long // long can't be with 1x leverage
        ? require(
            _leverage > _wad && _leverage <= _leverageMax,
            "_leverageCheck{Long}: INVLAID LEVERAGE"
        )
        : require(
            _leverage >= _wad && _leverage <= _leverageMax,
            "_leverageCheck{Short}: INVLAID LEVERAGE"
        );
}
```

### [G-02] Optimal Comparison

When comparing integers, it is cheaper to use strict `>` & `<` operators over `>=` & `<=` operators, even if you must increment or decrement one of the operands. Note: before using this technique, it's important to consider whether incrementing/decrementing one of the operators could result in an over/underflow.
This optimization is applicable when the optimizer is turned off.

Lines

- [Position.sol:289](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L289)
- [Position.sol:541](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L541)
- [Position.sol:592](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L592)
- [Position.sol:596](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L596)

Code Snippet (for one instance)

```js
require(_amountToRepay <= _quoteBalance, "_repay: balance of quote lt. debt");
```

### [G-03] Don't use SafeMath when using solidity >= 0.8.0

Solidity >= 0.8.0 checks for overflow/underflow by default. Using Safemath when using version >= 0.8.0 is redundant and will incur additional gas costs. Instead of safemath, you can simply use Solidity's built in arithmetic. For further gas savings, you can also use assembly and check for overflow/underflow.

Lines

- [Position.sol:166](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L166)
- [Position.sol:170](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L170)
- [Position.sol:176](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L176)
- [Position.sol:195](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L195)
- [Position.sol:317](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L317)
- [Position.sol:330](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L330)
- [Position.sol:332](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L332)
- [Position.sol:459](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L459)
- [Position.sol:463](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L463)
- [Position.sol:481](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L481)
- [Position.sol:485](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L485)
- [Position.sol:490](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L490)
- [Position.sol:496](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L496)
- [Position.sol:548](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L548)
- [Position.sol:567](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L567)
- [Position.sol:568](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L568)
- [Position.sol:570](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L570)
- [Position.sol:588](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L588)

Code Snippet (for one instance)

```js
// check to prevent underflow!
if (vars.initAssetBalance == 0) {
  vars.currentAssetBalance = assetBalance;
} else if (vars.initAssetBalance < assetBalance) {
  vars.currentAssetBalance = assetBalance.sub(vars.initAssetBalance);
} else {
  vars.currentAssetBalance = vars.initAssetBalance.sub(assetBalance);
}
```

### [G-04] Use `calldata` instead of `memory` for function arguments that do not get mutated.

Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage.

Lines

- [FeeWrapper.sol:61](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L61)
- [FeeWrapper.sol:77](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L77)
- [FeeWrapper.sol:93](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L93)
- [FeeWrapper.sol:109](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L109)

Code Snippet

```js
function calculateFee(uint256[] memory tokenAmounts, uint256 feeType, uint256 feeValue)...
```

### [G-05] Pack structs

When creating structs, make sure that the variables are listed in ascending order by data type. The compiler will pack the variables that can fit into one 32 byte slot. If the variables are not listed in ascending order, the compiler may not pack the data into one slot, causing additional `sload` and `sstore` instructions when reading/storing the struct into the contract's storage.

Lines

- [Position.sol:33](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L33-L40)

Code Snippet

```js
struct Position {
    address asset; // supplied as collateral
    address quote; // borrowed token
    uint256 borrowedAmount; // amount of borrowed quote
    uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
    uint256 blockNum; // block number on which position was opened
    bool isActive; // false by default, when active - true
}
```

### [G-06] Use Short Revert Strings

Keeping revert strings under 32-bytes prevents the string from being stored in more than one memory slot.

Lines

- [BathBuddy.sol:238](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L236-L239)

Code Snippet (for one instance)

```js
modifier onlyBathHouse() {
    require(msg.sender == bathHouse, "You are not my beloved bath house!");
    _;
}
```

### [G-07] Mark functions as payable (with discretion)

You can mark public or external functions as payable to save gas. Functions that are not payable have additional logic to check if there was a value sent with a call, however, making a function payable eliminates this check. This optimization should be carefully considered due to potentially unwanted behavior when a function does not need to accept ether.

Lines

- [Position.sol:65](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L65)
- [Position.sol:72](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L72)
- [Position.sol:86](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L86)
- [Position.sol:93](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L93)
- [Position.sol:107](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L107)
- [Position.sol:210](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L210)
- [Position.sol:226](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L226)
- [Position.sol:242](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L242)
- [V2Migrator.sol:38](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/V2Migrator.sol#L38)

Code Snippet (for one instance)

```js
function borrowBalance(address bathToken) public returns (uint256 balance) {
    balance = CTokenInterface(bathToken).borrowBalanceCurrent(
        address(this)
    );
}
```

### [G-08] Use custom errors instead of string error messages

Lines

- [Position.sol:102](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L102)
- [Position.sol:116](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L116)
- [Position.sol:275](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L275)
- [Position.sol:290](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L290)
- [Position.sol:300](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L300)
- [Position.sol:361](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L361)
- [Position.sol:387](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L387)
- [Position.sol:593](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L593)
- [Position.sol:597](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L597)
- [V2Migrator.sol:56](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/V2Migrator.sol#L56)
- [V2Migrator.sol:65](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/V2Migrator.sol#L65)

Code Snippet (for one instance)

```js
require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");
```

### [G-9] Mark storage variables as `constant` or `immutable` if they never change or delete if never used.

State variables can be declared as `constant` or `immutable`. In both cases, the variables cannot be modified after the contract has been constructed. For `constant` variables, the value has to be fixed at compile-time, while for `immutable`, it can still be assigned at construction time.

The compiler does not reserve a storage slot for these variables, and every occurrence is inlined by the respective value.

Compared to regular state variables, the gas costs of `constant` and `immutable` variables are much lower. For a `constant` variable, the expression assigned to it is copied to all the places where it is accessed and also re-evaluated each time. This allows for local optimizations. Immutable variables are evaluated once at construction time and their value is copied to all the places in the code where they are accessed. For these values, 32 bytes are reserved, even if they would fit in fewer bytes. Due to this, `constant` values can sometimes be cheaper than `immutable` values.

Lines to make constant or delete

- [RubiconMarket.sol:682](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L682)
- [RubiconMarket.sol:684](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L684)

Lines to make immutable

- [Position.sol#L44-L47](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L44-L47)

### [G-10] Use assembly for math (add, sub, mul, div)

Use assembly for math instead of Solidity. You can check for overflow/underflow in assembly to ensure safety. If using Solidity versions < 0.8.0 and you are using Safemath, you can gain significant gas savings by using assembly to calculate values and checking for overflow/underflow.

Lines

- [Position.sol:327](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L327)

Code Snippet

```js
uint256 _blockDelta = block.number - _startBlock;
```

### [G-11] `unchecked{++i}` instead of `++i` / `i++` or use assembly when applicable

Use `++i` instead of `i++`. This is especially useful in for loops but this optimization can be used anywhere in your code. You can also use `unchecked{++i;}` for even more gas savings but this will not check to see if `i` overflows. For extra safety if you are worried about this, you can add a require statement after the loop checking if `i` is equal to the final incremented value. For best gas savings, use inline assembly, however this limits the functionality you can achieve. For example you cant use Solidity syntax to internally call your own contract within an assembly block and external calls must be done with the `call()` or `delegatecall()` instruction. However when applicable, inline assembly will save much more gas.

Lines

- [Position.sol:158](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L158)
- [Position.sol:407](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L407)
- [Position.sol:572](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L572)
- [Position.sol:576](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L576)
- [Position.sol:580](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L580)
- [V2Migrator.sol:31](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/V2Migrator.sol#L31)
- [RubiconMarket.sol:569](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L569)
- [RubiconMarket.sol:899](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L899)
- [RubiconMarket.sol:911](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L911)
- [RubiconMarket.sol:924](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L924)

Code Snippet (for one instance)

```js
for (uint256 i = 0; i < vars.limit; ++i) {
    ...
}
```
