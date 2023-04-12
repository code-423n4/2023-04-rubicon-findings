# Summary

## Low Risk Issues

| |Issue|Instances|
|-|:-|:-:|
| [L-01](#L-01) | Update codes to avoid Compile Errors | 14 |
| [L-02](#L-02) | Keccak Constant values should changed to immutable rather than constant | 1 |

*Total 2 issues.*

## Non-Critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | The synchronized modifier should occur before all other modifiers | 1 |
| [NC-2](#NC-2) | Unnecessary use of modifier `can_offer()` in `SimpleMarket.sol`| 1 |
| [NC-3](#NC-3) | Events should be declared in the beginning of the contract | 6 |
| [NC-4](#NC-4) | Unused Events | 3 |
| [NC-5](#NC-5) | Use a more recent version of solidity | 2 |
| [NC-6](#NC-6) | Missing event for critical parameter change | 3 |
| [NC-7](#NC-7) | Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18) | 12 |
| [NC-8](#NC-8) | Use SMTChecker | |
| [NC-9](#NC-9) | Use the `delete` keyword instead of assigning a value of `0` | 5 |
| [NC-10](#NC-10) | Assembly Codes Specific – Should Have Comments | 7 |
| [NC-11](#NC-11) | Unused code in comments should be removed for better readability | 13 |

*Total 13 issues.*


## [L-01] Update codes to avoid Compile Errors

*There are 14 instances of this issue.*

```solidity
Warning: This declaration shadows an existing declaration.
   --> contracts/compound-v2-fork/Comptroller.sol:454:13:
    |
454 |             Error err = addToMarketInternal(CToken(msg.sender), borrower);
    |             ^^^^^^^^^
Note: The shadowed declaration is here:
   --> contracts/compound-v2-fork/Comptroller.sol:475:10:
    |
475 |         (Error err, , uint shortfall) = getHypotheticalAccountLiquidityInternal(
    |          ^^^^^^^^^


Warning: This declaration shadows an existing declaration.
  --> contracts/utilities/poolsUtility/Position.sol:33:5:
   |
33 |     struct Position {
   |     ^ (Relevant source part starts here and spans across multiple lines).
Note: The shadowed declaration is here:
  --> contracts/utilities/poolsUtility/Position.sol:15:1:
   |
15 | contract Position is Ownable, DSMath {
   | ^ (Relevant source part starts here and spans across multiple lines).


Warning: This contract has a payable fallback function, but no receive ether function. Consider adding a receive ether function.
  --> contracts/compound-v2-fork/CErc20Delegator.sol:11:1:
   |
11 | contract CErc20Delegator is CTokenInterface, CErc20Interface, CDelegatorInterface {
   | ^ (Relevant source part starts here and spans across multiple lines).
Note: The payable fallback function is defined here.
   --> contracts/compound-v2-fork/CErc20Delegator.sol:462:5:
    |
462 |     fallback() external payable {
    |     ^ (Relevant source part starts here and spans across multiple lines).


Warning: This contract has a payable fallback function, but no receive ether function. Consider adding a receive ether function.
  --> contracts/compound-v2-fork/Unitroller.sol:11:1:
   |
11 | contract Unitroller is UnitrollerAdminStorage, ComptrollerErrorReporter {
   | ^ (Relevant source part starts here and spans across multiple lines).
Note: The payable fallback function is defined here.
   --> contracts/compound-v2-fork/Unitroller.sol:136:5:
    |
136 |     fallback() payable external {
    |     ^ (Relevant source part starts here and spans across multiple lines).


Warning: Visibility for constructor is ignored. If you want the contract to be non-deployable, making it "abstract" is sufficient.
  --> contracts/compound-v2-fork/Unitroller.sol:33:5:
   |
33 |     constructor() public {
   |     ^ (Relevant source part starts here and spans across multiple lines).


Warning: Unused local variable.
   --> contracts/RubiconMarket.sol:298:9:
    |
298 |         uint256 id = uint256(id_);
    |         ^^^^^^^^^^


Warning: Unused function parameter. Remove or comment out the variable name to silence this warning.
   --> contracts/utilities/poolsUtility/Position.sol:528:9:
    |
528 |         address _asset,
    |         ^^^^^^^^^^^^^^


Warning: Function state mutability can be restricted to pure
   --> contracts/compound-v2-fork/Comptroller.sol:414:5:
    |
414 |     function redeemVerify(
    |     ^ (Relevant source part starts here and spans across multiple lines).


Warning: Function state mutability can be restricted to view
Warning: Function state mutability can be restricted to pure
  --> contracts/utilities/FeeWrapper.sol:28:5:
   |
28 |     function calculateFee(
   |     ^ (Relevant source part starts here and spans across multiple lines).


Warning: Function state mutability can be restricted to view
   --> contracts/utilities/poolsUtility/Position.sol:526:5:
    |
526 |     function _borrowLimit(
    |     ^ (Relevant source part starts here and spans across multiple lines).

```
## [L-02] Keccak Constant values should changed to immutable rather than constant

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn't save any gas because the compiler knows that developers often make this mistake, it's still best to use the right tool for the task at hand.

```solidity
File: contracts/RubiconMarket.sol

L232:    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232


## [NC-1] The synchronized modifier should occur before all other modifiers

This is a best-practice to protect against reentrancy in other modifiers.

```solidity
File: contracts/RubiconMarket.sol

L:264      modifier synchronized() {
              require(!locked);
              locked = true;
              _;
              locked = false;
           }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

## [NC-2] Unnecessary use of modifier `can_offer()` in `SimpleMarket`

The modifier `can.offer()` has no conditions of any sort in the contract `SimpleMarket` & is used in a function in the same contract which seems unnecessary.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260-L262

```solidity
File: contracts/RubiconMarket.sol

L:260         modifier can_offer() virtual {
                   _;
              }


L:518         ) public virtual can_offer synchronized returns (uint256 id) 
```
Its better to declare it in the contract `ExpiringMarket` instead of doing a override.

## [NC-3] Events should be declared in the beginning of the contract

*There are 6 instances of this issue:*

```solidity
File:contracts/periphery/BathBuddy.sol

L:261          event RewardAdded(uint256 reward);
               event Staked(address indexed user, uint256 amount);
               event Withdrawn(address indexed user, uint256 amount);
               event RewardPaid(address indexed user, uint256 reward);
               event RewardsDurationUpdated(uint256 newDuration);
               event Recovered(address token, uint256 amount);
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L261-L266

## [NC-4] Unused Events

*There are 3 instanes of this issue:*

```solidity
File:contracts/periphery/BathBuddy.sol

L:262          event Staked(address indexed user, uint256 amount);
L:263          event Withdrawn(address indexed user, uint256 amount);
L:266          event Recovered(address token, uint256 amount);
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L262-L263

## [NC‑5] Use a more recent version of solidity

Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>).
There are 2 instance of this issue:

```solidity
File: contracts/RubiconMarket.sol

L:2        pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

```solidity
File:contracts/periphery/BathBuddy.sol

L:2        pragma solidity ^0.8.0;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

## [NC-6] Missing event for critical parameter change
There are 4 instances of this issue:

```solidity
File: contracts/RubiconMarket.sol

L:1466     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
           feeBPS = _newFeeBPS;
           return true;
           }

L:1471     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
           StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
           return true;
           }

L:1476     function setFeeTo(address newFeeTo) external auth returns (bool) {
           require(newFeeTo != address(0));
           feeTo = newFeeTo;
           return true;
           }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1480

## [NC-7] Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

*There are 12 instances of this issue:*

```solidity
File: contracts/RubiconMarket.sol

L:74      uint256 constant WAD = 10 ** 18;

L:75      uint256 constant RAY = 10 ** 27;

L:1057    mul(pay_amt, 10 ** 9),

L:1059    ) / 10 ** 9;

L:1099    mul(buy_amt, 10 ** 9),

L:1101    ) / 10 ** 9;

L:1142    mul(pay_amt, 10 ** 9),

L:1144   ) / 10 ** 9;

L:1175    mul(buy_amt, 10 ** 9),

L:1177    ) / 10 ** 9;
```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

L:317     _max = (_liq.mul(10 ** 18)).div(_price);

L:331     ).div(10 ** 18);
```

## [NC-8] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19

## [NC-9] Use the `delete` keyword instead of assigning a value of `0`
Using the 'delete' keyword instead of assigning a '0' value is a detailed optimization that increases code readability and audit quality, and clearly indicates the intent.

Other hand, if use delete instead 0 value assign , it will be gas saved

```solidity
File: contracts/RubiconMarket.sol

L:1062       pay_amt = 0; //All amount is sold

L:1104       buy_amt = 0; //All amount is bought

L:1449       _near[id] = 0; //delete order from unsorted order list

L:1462       _near[id] = 0; //delete order from unsorted order list
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1062

```solidity
File: contracts/periphery/BathBuddy.sol
L:181        tokenRewards[address(rewardsToken)][holderRecipient] = 0;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L181

## [NC-10] Assembly Codes Specific – Should Have Comments

Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does

This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Aseembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.

```solidity
File: contracts/RubiconMarket.sol

L:648       assembly {
               foo := calldataload(4)
               bar := calldataload(36)
               wad := callvalue()
            }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L648-L652

```solidity
File: contracts/utilities/poolsUtility/Position.sol

L:367         assembly {
                 switch _initBathTokenAmount
                 case 0 {
                     _bathTokenAmount := _currentBathTokenAmount
                 }
                 default {
                     _bathTokenAmount := sub(
                         _currentBathTokenAmount,
                         _initBathTokenAmount
                     )
                 }
             }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L367-L378

## [NC-11] Unused code in comments should be removed for better readability

*There are 13 instances in `RubiconMarket.sol`*

```solidity
File: contracts/RubiconMarket.sol

L:100    /// event LogTrade(
L:101    ///     uint256 pay_amt,
L:102    ///     address indexed pay_gem,
L:103    ///     uint256 buy_amt,
L:104    ///     address indexed buy_gem
L:105    /// );
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L100-L105