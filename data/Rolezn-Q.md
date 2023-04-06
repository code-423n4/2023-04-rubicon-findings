## Summary<a name="Summary">

### Low Risk Issues
| |Issue|Contexts|
|-|:-|:-:|
| [LOW&#x2011;1](#LOW&#x2011;1) | IERC20 `approve()` Is Deprecated | 5 |
| [LOW&#x2011;2](#LOW&#x2011;2) | Do not allow fees to be set to `100%` | 2 |
| [LOW&#x2011;3](#LOW&#x2011;3) | `decimals()` not part of ERC20 standard | 1 |
| [LOW&#x2011;4](#LOW&#x2011;4) | Keccak Constant values should used to `immutable` rather than `constant` | 1 |
| [LOW&#x2011;5](#LOW&#x2011;5) | The Contract Should `approve(0)` First | 5 |
| [LOW&#x2011;6](#LOW&#x2011;6) | Missing Contract-existence Checks Before Low-level Calls | 3 |
| [LOW&#x2011;7](#LOW&#x2011;7) | Contracts are not using their OZ Upgradeable counterparts | 14 |
| [LOW&#x2011;8](#LOW&#x2011;8) | Prevent division by 0 | 5 |
| [LOW&#x2011;9](#LOW&#x2011;9) | `Safeapprove()` Is Deprecated | 1 |
| [LOW&#x2011;10](#LOW&#x2011;10) | Use `safetransfer` Instead Of `transfer`  | 15 |
| [LOW&#x2011;11](#LOW&#x2011;11) | Upgrade OpenZeppelin Contract Dependency | 1 |

Total: 53 contexts over 11 issues

### Non-critical Issues
| |Issue|Contexts|
|-|:-|:-:|
| [NC&#x2011;1](#NC&#x2011;1) | Add a timelock to critical functions | 6 |
| [NC&#x2011;2](#NC&#x2011;2) | Avoid Floating Pragmas: The Version Should Be Locked | 2 |
| [NC&#x2011;3](#NC&#x2011;3) | Constants in comparisons should appear on the left side | 6 |
| [NC&#x2011;4](#NC&#x2011;4) | Critical Changes Should Use Two-step Procedure | 6 |
| [NC&#x2011;5](#NC&#x2011;5) | Declare interfaces on separate files | 2 |
| [NC&#x2011;6](#NC&#x2011;6) | Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function | 26 |
| [NC&#x2011;7](#NC&#x2011;7) | Function writing that does not comply with the Solidity Style Guide  | 6 |
| [NC&#x2011;8](#NC&#x2011;8) | Large or complicated code bases should implement fuzzing tests | 1 |
| [NC&#x2011;9](#NC&#x2011;9) | Use `delete` to Clear Variables | 1 |
| [NC&#x2011;10](#NC&#x2011;10) | Imports can be grouped together | 3 |
| [NC&#x2011;11](#NC&#x2011;11) | NatSpec return parameters should be included in contracts | 1 |
| [NC&#x2011;12](#NC&#x2011;12) | No need to initialize uints to zero | 1 |
| [NC&#x2011;13](#NC&#x2011;13) | Initial value check is missing in Set Functions | 6 |
| [NC&#x2011;14](#NC&#x2011;14) | Contracts should have full test coverage | 1 |
| [NC&#x2011;15](#NC&#x2011;15) | Lines are too long | 3 |
| [NC&#x2011;16](#NC&#x2011;16) | Missing event for critical parameter change | 3 |
| [NC&#x2011;17](#NC&#x2011;17) | Implementation contract may not be initialized | 2 |
| [NC&#x2011;18](#NC&#x2011;18) | NatSpec comments should be increased in contracts | 1 |
| [NC&#x2011;19](#NC&#x2011;19) | Non-usage of specific imports | 11 |
| [NC&#x2011;20](#NC&#x2011;20) | Use a more recent version of Solidity | 6 |
| [NC&#x2011;21](#NC&#x2011;21) | Omissions in Events | 2 |
| [NC&#x2011;22](#NC&#x2011;22) | Large multiples of ten should use scientific notation | 3 |
| [NC&#x2011;23](#NC&#x2011;23) | Use `bytes.concat()` | 6 |
| [NC&#x2011;24](#NC&#x2011;24) | Use Underscores for Number Literals  | 3 |

Total: 107 contexts over 24 issues

## Low Risk Issues

### <a href="#Summary">[LOW&#x2011;1]</a><a name="LOW&#x2011;1"> IERC20 `approve()` Is Deprecated

`approve()` is subject to a known front-running attack. It is deprecated in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead.

https://docs.openzeppelin.com/contracts/3.x/api/token/erc20#IERC20-approve-address-uint256-

#### <ins>Proof Of Concept</ins>

```solidity
53: underlying.approve(bathTokenV2, amountWithdrawn);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L53

```solidity
105: IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L105

```solidity
297: IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L297

```solidity
465: IERC20(_quote).approve(address(rubiconMarket), _maxFill);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L465

```solidity
500: IERC20(_asset).approve(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L500



#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeIncreaseAllowance()` / `safeDecreaseAllowance()` instead.



### <a href="#Summary">[LOW&#x2011;2]</a><a name="LOW&#x2011;2"> Do not allow fees to be set to `100%`

It is recommended from a risk perspective to disallow setting 100% fees at all. A reasonable fee maximum should be checked for instead.

#### <ins>Proof Of Concept</ins>


```solidity

function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1466

```solidity

function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1471







### <a href="#Summary">[LOW&#x2011;3]</a><a name="LOW&#x2011;3"> `decimals()` not part of ERC20 standard

`decimals()` is not part of the official ERC20 standard and might fail for tokens that do not implement it. While in practice it is very unlikely, as usually most of the tokens implement it, this should still be considered as a potential issue.

#### <ins>Proof Of Concept</ins>


```solidity
148: _decimals = ERC20(_underlying).decimals()
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L148






### <a href="#Summary">[LOW&#x2011;4]</a><a name="LOW&#x2011;4"> Keccak Constant values should used to `immutable` rather than `constant`

Constant variables and immutable variables have distinct characteristics, and each should be employed in suitable situations.
Although using one instead of the other doesn't lead to gas savings, as the compiler recognizes this common developer error, it is always preferable to utilize the correct tool for the given task.

#### <ins>Proof Of Concept</ins>


```solidity
232: bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L232







### <a href="#Summary">[LOW&#x2011;5]</a><a name="LOW&#x2011;5"> The Contract Should `approve(0)` First

Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

#### <ins>Proof Of Concept</ins>


```solidity
53: underlying.approve(bathTokenV2, amountWithdrawn);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L53

```solidity
105: IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L105

```solidity
297: IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L297

```solidity
465: IERC20(_quote).approve(address(rubiconMarket), _maxFill);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L465

```solidity
500: IERC20(_asset).approve(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L500



#### <ins>Recommended Mitigation Steps</ins>

Approve with a zero amount first before setting the actual amount.



### <a href="#Summary">[LOW&#x2011;6]</a><a name="LOW&#x2011;6"> Missing Contract-existence Checks Before Low-level Calls

Low-level calls return success if there is no code present at the specified address. 

#### <ins>Proof Of Concept</ins>


```solidity
66: (bool _OK, bytes memory _data) = _params.target.call(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L66

```solidity
82: (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L82

```solidity
118: (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L118




#### <ins>Recommended Mitigation Steps</ins>

In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`




### <a href="#Summary">[LOW&#x2011;7]</a><a name="LOW&#x2011;7"> Contracts are not using their OZ Upgradeable counterparts

The non-upgradeable standard version of OpenZeppelin’s library are inherited / used by the contracts.
It would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

#### <ins>Proof of Concept</ins>

```solidity
4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L4

```solidity
11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L11-L12

```solidity
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L4-L5

```solidity
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
7: import "@openzeppelin/contracts/security/Pausable.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L4-L7

```solidity
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L4

```solidity
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
7: import "@openzeppelin/contracts/access/Ownable.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L4-L7



#### <ins>Recommended Mitigation Steps</ins>

Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.
See https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/tree/master/contracts for list of available upgradeable contracts



### <a href="#Summary">[LOW&#x2011;8]</a><a name="LOW&#x2011;8"> Prevent division by 0

On several locations in the code precautions are not being taken for not dividing by 0, this will revert the code.

These functions can be called with 0 value in the input, this value is not checked for being bigger than 0, that means in some scenarios this can potentially trigger a division by zero.

#### <ins>Proof Of Concept</ins>


```solidity
55: require(y == 0 || (z = x * y) / y
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L55

```solidity
86: z = add(mul(x, WAD), y / 2) / y
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L86

```solidity
90: z = add(mul(x, RAY), y / 2) / y
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L90

```solidity
1317: t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1317

```solidity
41: fees[i] = (tokenAmounts[i] * feeValue) / feeType
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L41



#### <ins>Recommended Mitigation Steps</ins>

Recommend making sure division by 0 won't occur by checking the variables beforehand and handling this edge case.



### <a href="#Summary">[LOW&#x2011;9]</a><a name="LOW&#x2011;9"> `safeApprove()` Is Deprecated

Deprecated in favor of `safeIncreaseAllowance()` and `safeDecreaseAllowance()`. If only setting the initial allowance to the value that means infinite, `safeIncreaseAllowance()` can be used instead

#### <ins>Proof Of Concept</ins>


```solidity
358: IERC20(_token).safeApprove(_bathToken, _amount);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L358



#### <ins>Recommended Mitigation Steps</ins>

Consider using `safeIncreaseAllowance()` / `safeDecreaseAllowance()` instead.





### <a href="#Summary">[LOW&#x2011;11]</a><a name="LOW&#x2011;11"> Upgrade OpenZeppelin Contract Dependency

An outdated OZ version is used (which has known vulnerabilities, see: https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories).

#### <ins>Proof Of Concept</ins>

Require dependencies to be at least version of 4.8.1 to include critical vulnerability patches.

```solidity
    "@openzeppelin/contracts": "^4.8.0"
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/../package.json#L47



#### <ins>Recommended Mitigation Steps</ins>

Update OpenZeppelin Contracts Usage in package.json and require at least version of 4.8.1 via `>=4.8.1`




## Non Critical Issues

### <a href="#Summary">[NC&#x2011;1]</a><a name="NC&#x2011;1"> Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user). Consider adding a timelock to the following functions:

#### <ins>Proof Of Concept</ins>


```solidity
25: function setOwner(address owner_) external auth {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L25

```solidity
955: function setMinSell(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L955

```solidity
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1466

```solidity
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1471

```solidity
1476: function setFeeTo(address newFeeTo) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1476

```solidity
232: function setRewardsDuration(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L232







### <a href="#Summary">[NC&#x2011;2]</a><a name="NC&#x2011;2"> Avoid Floating Pragmas: The Version Should Be Locked

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

#### <ins>Proof Of Concept</ins>

```solidity
Found usage of floating pragmas ^0.8.9 of Solidity in [RubiconMarket.sol]
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L2

```solidity
Found usage of floating pragmas ^0.8.0 of Solidity in [BathBuddy.sol]
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L2












### <a href="#Summary">[NC&#x2011;3]</a><a name="NC&#x2011;3"> Constants in comparisons should appear on the left side

Doing so will prevent <a href="https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html">typo bugs</a>

#### <ins>Proof Of Concept</ins>

```solidity
327: spend == 0 ||
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L327

```solidity
436: if (offers[id].pay_amt == 0) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L436

```solidity
1233: if (pos == 0) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1233

```solidity
1319: if (t_pay_amt == 0 || t_buy_amt == 0) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1319

```solidity
1413: _rank[id].delb == 0 &&
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1413

```solidity
163: if (vars.initAssetBalance == 0) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L163








### <a href="#Summary">[NC&#x2011;4]</a><a name="NC&#x2011;4"> Critical Changes Should Use Two-step Procedure

The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference:
https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure

#### <ins>Proof Of Concept</ins>

```solidity
25: function setOwner(address owner_) external auth {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L25

```solidity
955: function setMinSell(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L955

```solidity
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1466

```solidity
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1471

```solidity
1476: function setFeeTo(address newFeeTo) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1476

```solidity
232: function setRewardsDuration(
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L232



#### <ins>Recommended Mitigation Steps</ins>

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.



### <a href="#Summary">[NC&#x2011;5]</a><a name="NC&#x2011;5"> Declare interfaces on separate files

Interfaces should be declared on a separate file and not on the same file where the contract resides in.

#### <ins>Proof Of Concept</ins>

```solidity
8: interface IBathToken is IERC20 {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L8

```solidity
9: interface IBathBuddy {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L9






#### <a href="#Summary">[NC&#x2011;6]</a><a name="NC&#x2011;6"> Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function

Saves deployment costs

#### <ins>Proof Of Concept</ins>

```solidity
246: require(isActive(id));
252: require(isActive(id));
604: require(isActive(id));
611: require(isActive(id));
1366: require(isActive(id));
265: require(!locked);
938: require(!locked);
1034: require(!locked);
1075: require(!locked);
488: require(cancel(uint256(id)));
757: require(cancel(uint256(id)));
565: require(buy(uint256(id), maxTakeAmount));
753: require(buy(uint256(id), maxTakeAmount));
598: require(!isClosed());
605: require(!isClosed());
834: require(!locked, "Reentrancy attempt");
859: require(!locked, "Reentrancy attempt");
874: require(!locked, "Reentrancy attempt");
835: require(_dust[address(pay_gem)] <= pay_amt);
1354: require(_dust[address(pay_gem)] <= pay_amt);
1136: require(offerId != 0);
1169: require(offerId != 0);
1209: require(id > 0);
1226: require(id > 0);

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L246

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L252

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L604

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L611

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1366

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L265

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L938

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1034

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1075

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L488

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L757

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L565

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L753

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L598

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L605

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L834

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L859

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L874

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L835

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1354

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1136

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1169

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1209

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1226



```solidity
122: require(friendshipStarted, "I have not started a bathToken friendship");
143: require(friendshipStarted, "I have not started a bathToken friendship");

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L122

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L143










### <a href="#Summary">[NC&#x2011;7]</a><a name="NC&#x2011;7"> Function writing that does not comply with the Solidity Style Guide

Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:
- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private
- within a grouping, place the view and pure functions last

#### <ins>Proof Of Concept</ins>

All in-scope contracts

See `RubiconMarket.sol` for example




### <a href="#Summary">[NC&#x2011;8]</a><a name="NC&#x2011;8"> Large or complicated code bases should implement fuzzing tests

Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement <a href="https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05">fuzzing tests</a>. Fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.

#### <ins>Proof Of Concept</ins>

Various in-scope contract files.




### <a href="#Summary">[NC&#x2011;9]</a><a name="NC&#x2011;9"> Use `delete` to Clear Variables

`delete a` assigns the initial value for the type to `a`. i.e. for integers it is equivalent to `a = 0`, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset. Similarly, it can also be used to set an address to zero address. It has no effect on whole mappings though (as the keys of mappings may be arbitrary and are generally unknown). However, individual keys and what they map to can be deleted: If `a` is a mapping, then `delete a[x]` will delete the value stored at `x`.

The `delete` key better conveys the intention and is also more idiomatic. Consider replacing assignments of zero with `delete` statements.

#### <ins>Proof Of Concept</ins>

```solidity
181: tokenRewards[address(rewardsToken)][holderRecipient] = 0;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L181





### <a href="#Summary">[NC&#x2011;10]</a><a name="NC&#x2011;10"> Imports can be grouped together

Consider importing OZ first, then all interfaces, then all utils.

#### <ins>Proof Of Concept</ins>


```solidity
5: import "hardhat/console.sol";
11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L5

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L11

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L12









### <a href="#Summary">[NC&#x2011;11]</a><a name="NC&#x2011;11"> NatSpec return parameters should be included in contracts

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html
#### <ins>Proof Of Concept</ins>


All in-scope contracts

See `RubiconMarket.sol` for example



#### <ins>Recommended Mitigation Steps</ins>

Include return parameters in NatSpec comments

Recommendation Code Style: (from Uniswap3)

```solidity
    /// @notice Adds liquidity for the given recipient/tickLower/tickUpper position
    /// @dev The caller of this method receives a callback in the form of IUniswapV3MintCallback#uniswapV3MintCallback
    /// in which they must pay any token0 or token1 owed for the liquidity. The amount of token0/token1 due depends
    /// on tickLower, tickUpper, the amount of liquidity, and the current price.
    /// @param recipient The address for which the liquidity will be created
    /// @param tickLower The lower tick of the position in which to add liquidity
    /// @param tickUpper The upper tick of the position in which to add liquidity
    /// @param amount The amount of liquidity to mint
    /// @param data Any data that should be passed through to the callback
    /// @return amount0 The amount of token0 that was paid to mint the given amount of liquidity. Matches the value in the callback
    /// @return amount1 The amount of token1 that was paid to mint the given amount of liquidity. Matches the value in the callback
    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);
```



### <a href="#Summary">[NC&#x2011;12]</a><a name="NC&#x2011;12"> No need to initialize uints to zero

There is no need to initialize `uint` variables to zero as their default value is `0`

#### <ins>Proof Of Concept</ins>

```solidity
1214: uint256 old_top = 0;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1214





### <a href="#Summary">[NC&#x2011;13]</a><a name="NC&#x2011;13"> Initial value check is missing in Set Functions

A check regarding whether the current value and the new value are the same should be added

#### <ins>Proof Of Concept</ins>

```solidity
25: function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L25

```solidity
955: function setMinSell(
        ERC20 pay_gem, 
        uint256 dust 
    ) external auth note returns (bool) {
        _dust[address(pay_gem)] = dust;
        emit LogMinSell(address(pay_gem), dust);
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L955

```solidity
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1466

```solidity
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1471

```solidity
1476: function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1476

```solidity
232: function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );
        rewardsDuration[token] = _rewardsDuration;
        emit RewardsDurationUpdated(rewardsDuration[token]);
    }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L232






### <a href="#Summary">[NC&#x2011;14]</a><a name="NC&#x2011;14"> Contracts should have full test coverage

The test coverage rate of the project is 60%. Testing all functions is best practice in terms of security criteria.
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

```js
- What is the overall line coverage percentage provided by your tests?:  60%
```
Due to its capacity, test coverage is expected to be 100%.



### <a href="#Summary">[NC&#x2011;15]</a><a name="NC&#x2011;15"> Lines are too long

Usually lines in source code are limited to 80 characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length
Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length

#### <ins>Proof Of Concept</ins>

```solidity
9: /// @notice This contract is a derivative work, and spiritual continuation, of the open-source work from Oasis DEX: https://github.com/OasisDEX/oasis
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L9

```solidity
35: /// @dev NOTE: There is an implicit assumption that having bathToken shares means you are staked. This BathBuddy is paired with a SINGLE bathToken
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L35

```solidity
36: /// @dev It only accepts calls from that BathToken, specifically when a user wants to claim funds, base function and exit, withdraw() and get all rewards
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L36





### <a href="#Summary">[NC&#x2011;16]</a><a name="NC&#x2011;16"> Missing event for critical parameter change

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

#### <ins>Proof Of Concept</ins>


```solidity
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1466

```solidity
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1471

```solidity
1476: function setFeeTo(address newFeeTo) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1476





### <a href="#Summary">[NC&#x2011;17]</a><a name="NC&#x2011;17"> Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

#### <ins>Proof Of Concept</ins>


```solidity
30: constructor(address[] memory bathTokensV1, address[] memory bathTokensV2)
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L30

```solidity
54: constructor(address _oracle, address _rubiconMarket, address _bathHouseV2)
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L54





### <a href="#Summary">[NC&#x2011;18]</a><a name="NC&#x2011;18"> NatSpec comments should be increased in contracts

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability. https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

#### <ins>Proof Of Concept</ins>


All in-scope contracts

See `RubiconMarket.sol` for example


#### <ins>Recommended Mitigation Steps</ins>

NatSpec comments should be increased in contracts



### <a href="#Summary">[NC&#x2011;19]</a><a name="NC&#x2011;19"> Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace.
Instead, the Solidity docs recommend specifying imported symbols explicitly.
https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files

A good example:
```solidity
import {OwnableUpgradeable} from "openzeppelin-contracts-upgradeable/contracts/access/OwnableUpgradeable.sol";
import {SafeTransferLib} from "solmate/utils/SafeTransferLib.sol";
import {SafeCastLib} from "solmate/utils/SafeCastLib.sol";
import {ERC20} from "solmate/tokens/ERC20.sol";
import {IProducer} from "src/interfaces/IProducer.sol";
import {GlobalState, UserState} from "src/Common.sol";
```

#### <ins>Proof Of Concept</ins>


```solidity
5: import "./compound-v2-fork/InterestRateModel.sol";
6: import "./compound-v2-fork/CErc20Delegator.sol";
7: import "./compound-v2-fork/Comptroller.sol";
8: import "./compound-v2-fork/Unitroller.sol";
9: import "./periphery/BathBuddy.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L5-L9

```solidity
6: import "./compound-v2-fork/CTokenInterfaces.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L6

```solidity
5: import "./RubiconRouter.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L5

```solidity
8: import "../../compound-v2-fork/Comptroller.sol";
9: import "../../compound-v2-fork/PriceOracle.sol";
10: import "../../BathHouseV2.sol";
11: import "../../RubiconMarket.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L8-L11




#### <ins>Recommended Mitigation Steps</ins>

Use specific imports syntax per solidity docs recommendation.



### <a href="#Summary">[NC&#x2011;20]</a><a name="NC&#x2011;20"> Use a more recent version of Solidity

<a href="https://blog.soliditylang.org/2021/04/21/solidity-0.8.4-release-announcement/">0.8.4</a>:
bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>)

<a href="https://blog.soliditylang.org/2022/02/16/solidity-0.8.12-release-announcement/">0.8.12</a>: 
string.concat() instead of abi.encodePacked(<str>,<str>)

<a href="https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/">0.8.13</a>: 
Ability to use using for with a list of free functions

<a href="https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/">0.8.14</a>:

ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
Override Checker: Allow changing data location for parameters only when overriding external functions.

<a href="https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/">0.8.15</a>:

Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

<a href="https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/">0.8.16</a>:

Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

<a href="https://blog.soliditylang.org/2022/09/08/solidity-0.8.17-release-announcement/">0.8.17</a>:

Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.

<a href="https://blog.soliditylang.org/2023/02/22/solidity-0.8.19-release-announcement/">0.8.19</a>:
SMTChecker: New trusted mode that assumes that any compile-time available code is the actual used code, even in external calls. 
Bug Fixes: 
- Assembler: Avoid duplicating subassembly bytecode where possible.
- Code Generator: Avoid including references to the deployed label of referenced functions if they are called right away.
- ContractLevelChecker: Properly distinguish the case of missing base constructor arguments from having an unimplemented base function.
- SMTChecker: Fix internal error caused by unhandled z3 expressions that come from the solver when bitwise operators are used.
- SMTChecker: Fix internal error when using the custom NatSpec annotation to abstract free functions.
- TypeChecker: Also allow external library functions in using for.

#### <ins>Proof Of Concept</ins>


```solidity
pragma solidity 0.8.17;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L2

```solidity
pragma solidity ^0.8.9;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L2

```solidity
pragma solidity 0.8.17;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L2

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L2

```solidity
pragma solidity 0.8.17;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L2

```solidity
pragma solidity 0.8.17;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L2



#### <ins>Recommended Mitigation Steps</ins>

Consider updating to a more recent solidity version.



### <a href="#Summary">[NC&#x2011;21]</a><a name="NC&#x2011;21"> Omissions in Events

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters
The following events should also add the previous original value in addition to the new value.

#### <ins>Proof Of Concept</ins>


```solidity
97: event LogItemUpdate(uint256 id);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L97

```solidity
265: event RewardsDurationUpdated(uint256 newDuration);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L265




#### <ins>Recommended Mitigation Steps</ins>
The events should include the new value and old value where possible.










### <a href="#Summary">[NC&#x2011;22]</a><a name="NC&#x2011;22"> Large multiples of ten should use scientific notation

Use (e.g. 1e5) rather than decimal literals (e.g. 10000), for better code readability.

#### <ins>Proof Of Concept</ins>


```solidity
459: uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L459

```solidity
481: uint256 _fee = _minFill.mul(_feeBPS).div(10000);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L481

```solidity
490: _fee = _payAmount.mul(_feeBPS).div(10000);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L490





### <a href="#Summary">[NC&#x2011;23]</a><a name="NC&#x2011;23"> Use `bytes.concat()`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

#### <ins>Proof Of Concept</ins>


```solidity
369: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
                _offer.buy_gem,
                mFee
            )
400: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
            msg.sender,
            _offer.owner,
            _offer.pay_gem,
            _offer.buy_gem,
            uint128(quantity),
            uint128(spend)
        )
423: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
            _offer.buy_gem,
            fee
        )
442: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
                _offer.owner
            )
476: keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
            _offer.owner,
            _offer.pay_gem,
            _offer.buy_gem,
            uint128(_offer.pay_amt),
            uint128(_offer.buy_amt)
        )
555: keccak256(abi.encodePacked(pay_gem, buy_gem)),
            msg.sender,
            pay_gem,
            buy_gem,
            uint128(pay_amt),
            uint128(buy_amt)
        )

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L369

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L400

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L423

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L442

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L476

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L555



#### <ins>Recommended Mitigation Steps</ins>

Use `bytes.concat()` and upgrade to at least Solidity version 0.8.4 if required. 





### <a href="#Summary">[NC&#x2011;24]</a><a name="NC&#x2011;24"> Use Underscores for Number Literals

#### <ins>Proof Of Concept</ins>


```solidity
459: uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L459

```solidity
481: uint256 _fee = _minFill.mul(_feeBPS).div(10000);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L481

```solidity
490: _fee = _payAmount.mul(_feeBPS).div(10000);
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L490






