# QA Report
## Finding Summary

[**Low Severity**](#Low-Severity)
1. [**Unchecked Cast May Overflow**](#1-Unchecked-Cast-May-Overflow)
2. [**Incorrect Error Messages**](#2-Incorrect-Error-Messages)
3. [**Gas Grief If Relayed**](#3-Gas-Grief-If-Relayed)
4. [**Reentrancy Modifiers Should Appear First**](#4-Reentrancy-Modifiers-Should-Appear-First)
5. [**Faulty Reentrancy Guard Logic**](#5-Faulty-Reentrancy-Guard-Logic)
6. [**Unnecessary Use of SafeMath**](#6-Unnecessary-Use-of-SafeMath)
7. [**Low Test Coverage**](#7-Low-Test-Coverage)
8. [**Single Step Owner Transfer**](#8-Single-Step-Owner-Transfer)

[**Non-Critical**](#Non-Critical)
1. [**Contracts Missing @title Tag**](#1-Contracts-Missing-title-Tag)
2. [**Inconsistent Comment Initial Spacing**](#2-Inconsistent-Comment-Initial-Spacing)
3. [**Inconsistent Named Returns**](#3-Inconsistent-Named-Returns)
4. [**Spelling Mistakes**](#4-Spelling-Mistakes)
5. [**Power of Ten Literal Not In Scientific Notation**](#5-Power-of-Ten-Literal-Not-In-Scientific-Notation)
6. [**Explicit Variable Not Used**](#6-Explicit-Variable-Not-Used)
7. [**bytes.concat() Can Be Used Over abi.encodePacked()**](#7-bytesconcat-can-be-used-over-abiencodepacked)
8. [**ERC20 Token Recovery**](#8-ERC20-Token-Recovery)
9. [**Unbounded Compiler Version**](#9-Unbounded-Compiler-Version)
10. [**Duplicate Require Statements**](#10-Duplicate-Require-Statements)
11. [**Assembly Should Be Documented**](#11-Assembly-Should-Be-Documented)
12. [**Inconsistent Headers**](#12-Inconsistent-Headers)
13. [**Unnecessary Headers**](#13-Unnecessary-Headers)
14. [**Use of Reserved NatSpec Syntax**](#14-Use-of-Reserved-NatSpec-Syntax)
15. [**Scientific Notation Can Be Used**](#15-Scientific-Notation-Can-Be-Used)
16. [**Contracts Can Be Divided**](#16-Contracts-Can-Be-Divided)
17. [**Commented Code**](#17-Commented-Code)
18. [**Reentrancy Guard Naming**](#18-Reentrancy-Guard-Naming)
19. [**Extra Comment Dashes**](#19-Extra-Comment-Dashes)
20. [**Contradictory Function**](#20-Contradictory-Function)
21. [**Parameter Documentation Should Be In NatSpec**](#21-Parameter-Documentation-Should-Be-In-NatSpec)
22. [**Inconsistent Trailing Period**](#22-inconsistent-trailing-period)
23. [**Inconsistent Comment Capitalization**](#23-Inconsistent-Comment-Capitalization)
24. [**Extra Space**](#24-Extra-Space)
25. [**Inconsistant require Messages**](#25-Inconsistant-require-Messages)
26. [**Inconsistant require Message Punctuation**](#26-Inconsistant-require-Message-Punctuation)
27. [**Statement Can Be Simplified**](#27-Statement-Can-Be-Simplified)

[**Style Guide Violations**](#Style-Guide-Violations)
1. [**Code Layout**](#1-Code-Layout)
2. [**Maximum Line Length**](#2-Maximum-Line-Length)
3. [**Order of Functions**](#3-Order-of-Functions)
4. [**Function Declaration Style**](#4-Function-Declaration-Style)
5. [**Order of Layout**](#5-Order-of-Layout)

# Low Severity

## 1. Unchecked Cast May Overflow

As of Solidity 0.8 integer overflows are handled automatically; however, not for casting. For example `uint32(4294967300)` will result in `4` without reversion. Consider using OpenZepplin's SafeCast library. Even if it seems as though a value cannot overflow, it is best to be safe.

*/contracts/RubiconMarket.sol*

```solidity
297:	function bump(bytes32 id_) external can_buy(uint256(id_)) {
298:	uint256 id = uint256(id_);
321:	require(uint128(spend) == spend, "spend is not an int");
322:	require(uint128(quantity) == quantity, "quantity is not an int");
405:	uint128(quantity),
406:	uint128(spend)
480:	uint128(_offer.pay_amt),
481:	uint128(_offer.buy_amt)
519:	require(uint128(pay_amt) == pay_amt);
520:	require(uint128(buy_amt) == buy_amt);
534:	info.timestamp = uint64(block.timestamp);
559:	uint128(pay_amt),
560:	uint128(buy_amt)
625:	return uint64(block.timestamp);
1053:	take(bytes32(offerId), uint128(offers[offerId].pay_amt));
1061:	take(bytes32(offerId), uint128(baux));
1093:	take(bytes32(offerId), uint128(offers[offerId].pay_amt));
1103:	take(bytes32(offerId), uint128(buy_amt));
```

## 2. Incorrect Error Messages

There are error messages that will not hold true:

*/contracts/RubiconMarket.sol*

```solidity
321:    require(uint128(spend) == spend, "spend is not an int");
322:    require(uint128(quantity) == quantity, "quantity is not an int");
```

Let `x` be a `uint256`.

By doing a check that `uint128(x) == x` one is checking if `x` fits into a `uint128`. `x` is still an `int` regardless. If a user gives the [`buy`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314) function a `quantity` value such that `type(uint128).max < quantity <= type(uint256).max`, the user will get an error claiming: 
> "quantity is not an int"

This is not true.

## 3. Gas Grief If Relayed

When using `(bool sent, ) = address(msg.sender).call`, leaving out the `returnData` does not prevent the `returnData` from being copied to memory. If transactions were relayed, gas griefing is possible. Consider using assembly to prevent such a grief.

*/contracts/utilities/FeeWrapper.sol*

```Solidity
118:    (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
```

For more information please see [this](https://twitter.com/pashovkrum/status/1607024043718316032?t=xs30iD6ORWtE2bTTYsCFIQ&s=19) post.

## 4. Reentrancy Modifiers Should Appear First

The `synchronized` modifier acts as a reentrancy guard and as a result should appear before any other modifier in a function. Putting the reentrancy guard before other modifiers ensures that other modifiers cannot themselves get re-entered.

The following functions have a `synchronized` modifier after other modifiers:

*/contracts/RubiconMarket.sol*

* [`buy`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L317), [`cancel`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L454), [`offer`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L518).

## 5. Faulty Reentrancy Guard Logic

The [`synchronized`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269) modifier acts as a reentrancy guard: 

```Solidity
264:    modifier synchronized() {
265:        require(!locked);
266:        locked = true;
267:        _;
268:        locked = false;
269:    }
```

There are a few functions that utilize a check on the [`locked`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224) state variable to ensure non-reentrance. The [`locked`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224) checks look as follows: `require(!locked, "Reentrancy attempt")`. The functions that utilize the "Reentrancy" check do not implement [`synchronized`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269). All a check on [`locked`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224) will do is prevent a [`synchronized`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269) function from calling the checking function. The [`locked`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224) variable will remain `false` in an actual reentrancy scenario when a function is called multiple times without a [`synchronized`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269) modifier.

The following functions are affected:
* [`offer`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L824-L852), [`buy`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L855-L868), [`cancel`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871-L883), [`del_rank`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L937-L948), [`sellAllAmount`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028-L1067), [`buyAllAmount`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1069-L1112).

## 6. Unnecessary Use of SafeMath

The codebase uses [`SafeMath`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45-L92); however, all contracts in scope use a Solidity version equal to or greater than `0.8.0`. Solidity `0.8.0` introduced automatic overflow / underflow protection, making `SafeMath` redundant. Consider removing  `SafeMath` where possible to avoid unneeded complexity.

## 7. Low Test Coverage

The code coverage of the files in scope is not 100%. Consider a test coverage of 100%.

```
| File                                                       | % Lines        | % Statements   | % Branches     | % Funcs       |
|------------------------------------------------------------|----------------|----------------|----------------|---------------|
| contracts/BathHouseV2.sol                                  | 0.00% (0/26)   | 0.00% (0/31)   | 0.00% (0/10)   | 0.00% (0/7)   |
| contracts/RubiconMarket.sol                                | 1.01% (3/298)  | 0.95% (3/315)  | 0.00% (0/162)  | 4.17% (3/72)  |
| contracts/V2Migrator.sol                                   | 0.00% (0/11)   | 0.00% (0/14)   | 0.00% (0/6)    | 0.00% (0/1)   |
| contracts/compound-v2-fork/BaseJumpRateModelV2.sol         | 0.00% (0/20)   | 0.00% (0/26)   | 0.00% (0/6)    | 0.00% (0/5)   |
| contracts/compound-v2-fork/CDaiDelegate.sol                | 0.00% (0/49)   | 0.00% (0/71)   | 0.00% (0/12)   | 0.00% (0/9)   |
| contracts/compound-v2-fork/CErc20.sol                      | 0.00% (0/38)   | 0.00% (0/44)   | 0.00% (0/10)   | 0.00% (0/14)  |
| contracts/compound-v2-fork/CErc20Delegate.sol              | 0.00% (0/6)    | 0.00% (0/4)    | 0.00% (0/8)    | 0.00% (0/2)   |
| contracts/compound-v2-fork/CErc20Delegator.sol             | 0.00% (0/79)   | 0.00% (0/110)  | 0.00% (0/8)    | 0.00% (0/37)  |
| contracts/compound-v2-fork/CEther.sol                      | 0.00% (0/16)   | 0.00% (0/16)   | 0.00% (0/4)    | 0.00% (0/11)  |
| contracts/compound-v2-fork/CToken.sol                      | 0.00% (0/284)  | 0.00% (0/329)  | 0.00% (0/106)  | 0.00% (0/46)  |
| contracts/compound-v2-fork/Comptroller.sol                 | 0.00% (0/419)  | 0.00% (0/487)  | 0.00% (0/210)  | 0.00% (0/60)  |
| contracts/compound-v2-fork/DAIInterestRateModelV3.sol      | 0.00% (0/18)   | 0.00% (0/23)   | 0.00% (0/6)    | 0.00% (0/4)   |
| contracts/compound-v2-fork/ErrorReporter.sol               | 0.00% (0/4)    | 0.00% (0/4)    | 100.00% (0/0)  | 0.00% (0/2)   |
| contracts/compound-v2-fork/ExponentialNoError.sol          | 0.00% (0/34)   | 0.00% (0/36)   | 0.00% (0/4)    | 0.00% (0/30)  |
| contracts/compound-v2-fork/JumpRateModel.sol               | 0.00% (0/13)   | 0.00% (0/19)   | 0.00% (0/4)    | 0.00% (0/3)   |
| contracts/compound-v2-fork/JumpRateModelV2.sol             | 0.00% (0/1)    | 0.00% (0/1)    | 100.00% (0/0)  | 0.00% (0/1)   |
| contracts/compound-v2-fork/Lens/CompoundLens.sol           | 0.00% (0/93)   | 0.00% (0/117)  | 0.00% (0/20)   | 0.00% (0/13)  |
| contracts/compound-v2-fork/Maximillion.sol                 | 0.00% (0/7)    | 0.00% (0/8)    | 0.00% (0/2)    | 0.00% (0/2)   |
| contracts/compound-v2-fork/Reservoir.sol                   | 0.00% (0/30)   | 0.00% (0/36)   | 0.00% (0/10)   | 0.00% (0/5)   |
| contracts/compound-v2-fork/SafeMath.sol                    | 0.00% (0/31)   | 0.00% (0/33)   | 0.00% (0/18)   | 0.00% (0/10)  |
| contracts/compound-v2-fork/SimplePriceOracle.sol           | 0.00% (0/13)   | 0.00% (0/14)   | 0.00% (0/2)    | 0.00% (0/6)   |
| contracts/compound-v2-fork/Timelock.sol                    | 0.00% (0/37)   | 0.00% (0/41)   | 0.00% (0/28)   | 0.00% (0/8)   |
| contracts/compound-v2-fork/Unitroller.sol                  | 0.00% (0/31)   | 0.00% (0/32)   | 0.00% (0/8)    | 0.00% (0/5)   |
| contracts/compound-v2-fork/WhitePaperInterestRateModel.sol | 0.00% (0/9)    | 0.00% (0/13)   | 0.00% (0/2)    | 0.00% (0/3)   |
| contracts/periphery/BathBuddy.sol                          | 0.00% (0/31)   | 0.00% (0/34)   | 0.00% (0/16)   | 0.00% (0/8)   |
| contracts/periphery/BathTokenV1.sol                        | 0.00% (0/110)  | 0.00% (0/128)  | 0.00% (0/30)   | 0.00% (0/34)  |
| contracts/periphery/DummyPriceOracle.sol                   | 0.00% (0/2)    | 0.00% (0/2)    | 100.00% (0/0)  | 0.00% (0/2)   |
| contracts/periphery/RubiconMarketV1.sol                    | 0.00% (0/299)  | 0.00% (0/309)  | 0.00% (0/168)  | 0.00% (0/67)  |
| contracts/periphery/Test3rdPartyProtocol.sol               | 0.00% (0/39)   | 0.00% (0/55)   | 0.00% (0/6)    | 0.00% (0/4)   |
| contracts/periphery/TokenWithFaucet.sol                    | 0.00% (0/14)   | 0.00% (0/14)   | 0.00% (0/10)   | 0.00% (0/8)   |
| contracts/periphery/WETH9.sol                              | 0.00% (0/20)   | 0.00% (0/20)   | 0.00% (0/8)    | 0.00% (0/7)   |
| contracts/utilities/FeeWrapper.sol                         | 0.00% (0/30)   | 0.00% (0/36)   | 0.00% (0/10)   | 0.00% (0/6)   |
| contracts/utilities/MarketAid.sol                          | 0.00% (0/180)  | 0.00% (0/225)  | 0.00% (0/70)   | 0.00% (0/37)  |
| contracts/utilities/MarketAidFactory.sol                   | 0.00% (0/11)   | 0.00% (0/13)   | 0.00% (0/4)    | 0.00% (0/3)   |
| contracts/utilities/RubiconRouter.sol                      | 0.00% (0/194)  | 0.00% (0/262)  | 0.00% (0/84)   | 0.00% (0/29)  |
| contracts/utilities/poolsUtility/PoolsUtility.sol          | 0.00% (0/12)   | 0.00% (0/13)   | 0.00% (0/4)    | 0.00% (0/3)   |
| contracts/utilities/poolsUtility/Position.sol              | 0.00% (0/134)  | 0.00% (0/168)  | 0.00% (0/52)   | 0.00% (0/26)  |
| Total                                                      | 0.11% (3/2643) | 0.10% (3/3103) | 0.00% (0/1108) | 0.51% (3/590) |
```

## 8. Single Step Owner Transfer

The codebase uses a single step owner transfer seen [here](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28): 

```Solidity
25:    function setOwner(address owner_) external auth {
26:        owner = owner_;
27:        emit LogSetOwner(owner);
28:    }
```

Single step owner transfers are prone to admin error. 

Consider a 2 step ownership transfer like in [Ownable2Step.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol) to avoid possible transfer errors.

# Non-Critical

## 1. Contracts Missing @title Tag

3 out of 6 of the contracts in scope are missing a `@title` tag. Given that 3 contracts all have a `@title` tag, consider adding one per the 3 remaining contracts.

[BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol), [V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol), and [FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol) are missing a `@title` tag.

## 2. Inconsistent Comment Initial Spacing

Some comments have an initial space after `//` or `///` while others do not. It is best for code clearity to keep a consistent style.

1. The following contracts only have initial space comments (EX. `// foo`): [V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol).
2. No contracts have only no initial space comments (EX. `//foo`).
3. The following contracts have both: [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol), [BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol), [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol), [Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol), and [FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol).

## 3. Inconsistent Named Returns

Some functions use named returns and others do not. It is best for code clearity to keep a consistent style.

1. The following contracts only have named returns (EX. `returns(uint256 foo)`): [Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol).
2. The following contracts only have non-named returns (EX. `returns(uint256)`): [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol).
3. The following contracts have both: [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol), [BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol), and [FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol).

## 4. Spelling Mistakes

There are some spelling mistakes throughout the codebase. Consider fixing all spelling mistakes.

*contracts/RubiconMarket.sol*

* The word `multiple` is misspelled as [`multuple`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L886).

*contracts/utilities/FeeWrapper.sol*

* The word `interactions` is misspelled as [`interatcions`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L7).
* The word `default` is misspelled as [`defaull`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L59).
* The word `router` is misspelled as [`rotuer`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L75).

## 5. Power of Ten Literal Not In Scientific Notation

Power of ten literals > `1e2` are easier to read when expressed in scientific notation. Consider expressing large powers of ten in scientific notation.

*/contracts/utilities/poolsUtility/Position.sol*

```Solidity
459:	uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
481:	uint256 _fee = _minFill.mul(_feeBPS).div(10000);
490:	_fee = _payAmount.mul(_feeBPS).div(10000);
```

## 6. Explicit Variable Not Used

As described in the [Solidity documentation](https://docs.soliditylang.org/en/v0.4.21/types.html#integers):
> "`uint` and `int` are aliases for `uint256` and `int256`, respectively". 

There are moments in the codebase where `uint` is used instead of the explicit `uint256`. It is best to be explicit with variable names to lessen confusion. Consider replacing instances of `uint` with `uint256`.

*/contracts/RubiconMarket.sol*

```Solidity
785:	uint pos,
888:	uint[] calldata payAmts,
890:	uint[] calldata buyAmts,
918:	uint[] calldata ids,
919:	uint[] calldata payAmts,
921:	uint[] calldata buyAmts,
```

## 7. bytes.concat() Can Be Used Over abi.encodePacked()

Consider using `bytes.concat()` instead of `abi.encodePacked()` in contracts with Solidity version >= `0.8.4`.

*/contracts/RubiconMarket.sol*

```Solidity
369:	keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
400:	keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
423:	keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
442:	keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
476:	keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
555:	keccak256(abi.encodePacked(pay_gem, buy_gem)),
```

## 8. ERC20 Token Recovery

Consider adding a recovery function for Tokens that are accidently sent to the core contracts of the protocol. 

## 9. Unbounded Compiler Version

[This](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2) pragma and [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2) pragma are unbounded. No one knows what will come in future compiler versions less than `0.9.0`, thus it is best to put an upper limit on the compiler version.

## 10. Duplicate Require Statements

Duplicate require statements should be made into a single modifier or function. 

*/contracts/RubiconMarket.sol*

```Solidity
859:    require(!locked, "Reentrancy attempt");
874:    require(!locked, "Reentrancy attempt");
938:    require(!locked);
1034:   require(!locked);
1075:   require(!locked);
```

```Solidity
1209:   require(id > 0);
1226:   require(id > 0);
```

## 11. Assembly Should Be Documented

It is best practice to heavily comment all lines of assembly. The following lines of code are not documented:

*/contracts/RubiconMarket.sol*

```Solidity
648:        assembly {
649:            foo := calldataload(4)
650:            bar := calldataload(36)
651:            wad := callvalue()
652:        }
```

*/contracts/utilities/poolsUtility/Position.sol*

```Solidity
367:        assembly {
368:            switch _initBathTokenAmount
369:            case 0 {
370:                _bathTokenAmount := _currentBathTokenAmount
371:            }
372:            default {
373:                _bathTokenAmount := sub(
374:                    _currentBathTokenAmount,
375:                    _initBathTokenAmount
376:                )
377:            }
378:        }
```

## 12. Inconsistent Headers

[Some headers](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L42) use a multi-line comment (`/*`), [others](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L41) use a single line comment (`//`). Consider sticking to a single style.

## 13. Unnecessary Headers

There are generic headers in [`BathHouseV2.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L41), [`BathBuddy.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L42), [`Position.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L61), and [`FeeWrapper.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L26). Generic headers help with readability, but add unnecessary bulk - as long as the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#introduction) is followed. Specifically, the two key elements that should be followed from the Style Guide are [Order of Layout](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout) and [Order of Functions](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions). Consider removing generic headers.

## 14. Use of Reserved NatSpec Syntax

Some code is commented using the [NatSpec](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html#documentation-example) reserved triple dash (`///`). There should not be any commented code in production code; however, if there is it should not be commented with `///`. `///` is reserved specifically for [NatSpec](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html#documentation-example) comments. 

The following lines have been commented using `///`:

*/contracts/RubiconMarket.sol*

* [L16](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L16), [L99-L116](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L99-L116), [L128-L150](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L128-L150), [L163-L172](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172), [L184-L195](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L184-L195), [L197](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L197), [L207](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L207), [L299-L309](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L299-L309), [L386-L396](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396), [L409-L417](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417), [L428-L434](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L428-L434), [L464-L472](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472), [L542-L551](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551).

## 15. Scientific Notation Can Be Used

Exponential form (`10 ** 9`) is used throughout the codebase ([ex](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1059)). Consider simplifying all code by using scientific notation (`1e9`).

## 16. Contracts Can Be Divided

[`RubiconMarket.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol) consists of 9 contracts ([`DSAuthEvents`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L15), [`DSAuth`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L22), [`DSMath`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45), [`EventfulMarket`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L96), [`SimpleMarket`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L218), [`ExpiringMarket`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L593), [`DSNote`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L633), [`MatchingEvents`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L660), [`RubiconMarket`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L674)). Consider creating a Solidity file per contract in [`RubiconMarket.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol) for easier readability.

## 17. Commented Code

There are large amounts of commented code in [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol) that add a lot of bulk. Production code should not have commented code. The following lines should be removed:

* [L16](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L16), [L116](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L99-L116), [L128-L150](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L128-L150), [L163-L172](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172), [L185](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L185), [L187-L195](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L187-L195), [L207](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L207), [L300-L309](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L300-L309), [L386-L396](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396), [L409-L417](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417), [L429-L434](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L429-L434), [L438](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L438), [L464-L472](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472), [L542-L551](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551), [L661](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L661), [L663](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L663), [L666](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L666), [L838](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L838), [L850-L851](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L850-L851), [L1343-L1359](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1343-L1359), [L1382](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1382).

## 18. Reentrancy Guard Naming

The [`synchronized`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269) modifier acts as a reentrancy guard, but is oddly named. Consider renaming the modifier - specifically indicating it is a reentrancy guard.

## 19. Extra Comment Dashes

There is an extra comment indicator [here](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L250) (`// ///`). Only the NatSpec (`///`) should be present.

## 20. Contradictory Function

A contradictory function (opposite of a tautological function) is present [here](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L622). If a function always returns `false` it is either not needed or indicative of incomplete functionality and should be addressed.

## 21. Parameter Documentation Should Be In NatSpec

There are instances like so, where parameters are documented:

*/contracts/RubiconMarket.sol*

```solidity
803:    uint256 pay_amt, //maker (ask) sell how much
804:    ERC20 pay_gem, //maker (ask) sell which token
805:    uint256 buy_amt, //maker (ask) buy how much
806:    ERC20 buy_gem, //maker (ask) buy which token
807:    uint256 pos, //position to insert offer, 0 should be used if unknown
```

Consider using a properly annotated function [NatSpec](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html#tags) header with `@param` tags.

## 22. Inconsistent Trailing Period

[This](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L882) line has a trailing period, while [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1038) line does not. Consider removing all trailing periods to maintain style.

## 23. Inconsistent Comment Capitalization

There is an inconsistency in the capitalization of comments. For example, [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1055) line is not capitilized, while [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1050) line is. Consider capitilizing all comments.

## 24. Extra Space

There is an inconsistency in comment spacing. For example, [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L936) line has 2 spaces after the `//`, [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L972) line has 6 spaces, and [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L951) line has 4 spaces. Consider sticking to a single spacing style.

## 25. Inconsistant require Messages

Some require statements are the exact same, except they differ in their message. One has a message, the other does not. Consider using the same require message for all instances of the same statement.

*/contracts/RubiconMarket.sol*

```solidity
321:    require(uint128(spend) == spend, "spend is not an int");
322:    require(uint128(quantity) == quantity, "quantity is not an int");

519:    require(uint128(pay_amt) == pay_amt);
520:    require(uint128(buy_amt) == buy_amt);
```

```solidity
859:    require(!locked, "Reentrancy attempt");
874:    require(!locked, "Reentrancy attempt");
938:    require(!locked);
1034:   require(!locked);
1075:   require(!locked);
```

## 26. Inconsistant require Message Punctuation

[This](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L720) require message has a trailing period, while [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L701) message does not. Consider removing all trailing periods to maintain style.

## 27. Statement Can Be Simplified

The [following function](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41) can be simplified as follows:

**Original**
```solidity
35:    function isAuthorized(address src) internal view returns (bool) {
36:        if (src == owner) {
37:            return true;
38:        } else {
39:            return false;
40:        }
41:    }
```

**Simplified**
```solidity
35:    function isAuthorized(address src) internal view returns (bool) {
36:        return (src == owner);
41:    }
```

# Style Guide Violations

## 1. Code Layout

According to the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#code-layout): 
> "Spaces are the preferred indentation method". 

although not forced unless mixed with space indentation. There are a few places where tabs are used over 4 spaces. Consider changing all indentation to space indentation (preffered) or at the least keep the same indentation style.

The following contracts contain tabs:

[Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol), and [FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol).

## 2. Maximum Line Length

Lines with greater length than 120 characters are used. The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-lengthhttps://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) suggests that all lines should be 120 characters or less in width.

The following lines are longer than 120 characters, it is suggested to shorten these lines:

*contracts/RubiconMarket.sol*
*  [9](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L9), [250](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L250), [312](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L312), [348](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L348), [522](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L522), [524](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L524), [693](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L693), [726](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L726). 

*contracts/periphery/BathBuddy.sol*
*  [28](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L28), [31](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L31), [35](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L35), [36](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L36), [66](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L66), [166](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L166), [167](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L167). 

## 3. Order of Functions

The Solidity Style Guide suggests the following function order: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private.

The following contracts are not compliant (examples are only to prove the functions are out of order NOT a full description): 

* [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol): 
	* `SimpleMarket`: external functions are positioned after public functions.
	* `ExpiringMarket`: external functions are positioned after public functions.
	* `RubiconMarket`: external functions are positioned after public functions.
* [BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol): external functions are positioned after public functions.
* [V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol): constructor is positioned after external functions.
* [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol): external functions are positioned after public functions.
* [Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol): external functions are positioned after public functions.

## 4. Function Declaration Style

The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#function-declaration) states that: 

> "[f]or short function declarations, it is recommended for the opening brace of the function body to be kept on the same line as the function declaration". 

It is not clear what length is exactly meant by "short" or "long". The [maximum line length](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-lengthhttps://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length) of 120 characters may be an indication, and in that case any expanded function declaration under 120 characters should be on a single line.

The following functions in their respective contracts are not compliant by this standard: 

*contracts/RubiconMarket.sol*
* `getOffer`, `buy`, `cancel`, `calcAmountAfterFee`, `make`, `buy`, `cancel`, `setMinSell`, `getMinSell`, `getBestOffer`, `getOfferCount`, `getBuyAmount`, `getPayAmount`, `_isPricedLtOrEq`, `_sort`, `_unsort`, `_hide`. 

*contracts/BathHouseV2.sol*
* `getBathTokenFromAsset`, `whoIsBuddy`, `claimRewards`. 

*contracts/periphery/BathBuddy.sol*
* `spawnBuddy`, `lastTimeRewardApplicable`, `earned`, `getRewardForDuration`, `setRewardsDuration`. 

*contracts/utilities/poolsUtility/Position.sol*
* `borrowBalanceOfPos`, `_maxBorrow`, `_supply`, `_updateMargin`, `_rubiconSwap`, `_marketBuy`, `_marketSell`. 

*contracts/utilities/FeeWrapper.sol*
* `rubicall`, `_rubicall`, `_rubicallPayable`, `_chargeFeePayable`. 

## 5. Order of Layout

The [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout) suggests the following contract layout order: Type declarations, State variables, Events, Modifiers, Functions.

The following contracts are not compliant (examples are only to prove the layout are out of order NOT a full description): 

* [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)
	* `DSAuth`: Functions are positioned before Modifiers
	* `DSMath`: State is positioned after Functions.
	* `RubiconMarket`: Modifiers are positioned after Functions.
* [V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol): State is positioned after Functions.
* [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol): State is positioned after Functions.
