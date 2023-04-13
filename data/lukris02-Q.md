# QA Report for Rubicon v2 contest
## Overview
During the audit, 16 low and 26 non-critical issues were found.

№ | Title | Risk Rating  | Instance Count
--- | --- | --- | ---
L-1 | Event should be emitted | Low | 5
L-2 | Misleading function names | Low | 4
L-3 | Rename functions take() and buy() | Low | 1
L-4 | Rename functions kill() and cancel() | Low | 1
L-5 | Emit events with different parameter | Low | 2
L-6 | Not needed function | Low | 1
L-7 | Use SafeCast Library | Low | 1
L-8 | Add more information in the events | Low | 4
L-9 | Critical changes should use two-step procedure | Low | 5
L-10 | Use the two-step-transfer of ownership | Low | 1
L-11 | Return the amount of funds needed to cover fee | Low | 3
L-12 | Use Checks Effects Interactions pattern | Low | 1
L-13 | Owner can renounce ownership | Low | 1
L-14 | Add a timelock to critical functions | Low | 5
L-15 | Misleading comment | Low | 1
L-16 | Function params shadow other (inherited) state variables | Low | 3
NC-1 | Open questions | Non-Critical | 4
NC-2 | Rename some functions | Non-Critical | 2
NC-3 | Change type of function param | Non-Critical | 2+
NC-4 | Different names for the same parameter | Non-Critical | 1
NC-5 | Ugly hack | Non-Critical | 1
NC-6 | Limit fees | Non-Critical | 2
NC-7 | No need to use SafeMath | Non-Critical | 2
NC-8 | Functions with different names make the same | Non-Critical | 2
NC-9 | Commented code | Non-Critical | 13
NC-10 | Typos | Non-Critical | 1
NC-11 | Unused variable | Non-Critical | 2
NC-12 | Prevent zero transfers | Non-Critical | 5
NC-13 | Order of Functions | Non-Critical | 27
NC-14 | Inconsistency when using the number 1000 | Non-Critical | 3
NC-15 | Constants may be used | Non-Critical | 11
NC-16 | Update import usages  | Non-Critical | 14
NC-17 | Missing and extra leading underscores | Non-Critical | 14
NC-18 |  Visibility is not set | Non-Critical | 3
NC-19 | Lack of event emission in initialize() function | Non-Critical | 2
NC-20 | Concatenate multiple lines of code | Non-Critical | 14
NC-21 | Missing NatSpec | Non-Critical | 1+
NC-22 | Order of Layout | Non-Critical | 8
NC-23 | Unused named return variables | Non-Critical | 10
NC-24 | Maximum line length exceeded | Non-Critical | 3
NC-25 | Inconsistency when using uint and uint256 | Non-Critical | 4
NC-26 | Use mixedCase for functions and modifiers | Non-Critical | 9

## Low Risk Findings(16)
### L-1. Event should be emitted
##### Description
The governor functions that change critical parameters should emit events. Events allow capturing the changed parameters so that off-chain tools/interfaces can register such changes and allow users to evaluate them and consider if they would like to engage/exit based on how they perceive the changes as affecting the trustworthiness of the protocol or profitability of the implemented financial services. The alternative of directly querying on-chain contract state for such changes is not considered practical for most users/usages.
##### Instances
- [```function stop() external auth {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628) 
- [```function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466) 
- [```function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471) 
- [```function setFeeTo(address newFeeTo) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476) 
- [```function withdraw(address token, uint256 amount) external onlyOwner {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L242) 

##### Recommendation
Consider adding events for these functions.
#
### L-2. Misleading function names
##### Description
According to comments, function getBuyAmountWithFee() calculates "fill_amt with fee deducted",  function getBuyAmount() - "return fill_amt without fee!". Such names and comments are a bit confusing and can be perceived as the same things. The same applies to functions getPayAmountWithFee() and function getPayAmount().
##### Instances
- [```function getBuyAmountWithFee(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1115)
- [```function getBuyAmount(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1124) 
- [```function getPayAmountWithFee(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1149) 
- [```function getPayAmount(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1157) 

##### Recommendation
Consider names like getBuyAmountWithFeeDeducted and getBuyAmountWithoutFeeDeducted, and make commentsmore precise.
#
### L-3. Rename functions take() and buy()
##### Description
It's not immediately obvious how the take() function differs from buy(), since take() simply calls buy().
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L564

##### Recommendation
Consider renaming to buyByUintId() and buyByBytesId(), and place them side by side.
#
### L-4. Rename functions kill() and cancel()
##### Description
It’s not immediately obvious how the kill() function differs from cancel(), since kill() simply calls cancel().
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L756
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871

##### Recommendation
Consider renaming to cancelByUintId() and cancelByBytesId(), and place them side by side.
#
### L-5. Emit events with different parameter
##### Description
Because of [this](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L348), events with different params should be emitted in different branches of the condition. For example, [event ```emitFee```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L365-L372) should be placed right after line 362, and the same event but with the parameter ```_offer.recipient``` (instead of ```_offer.owner```) should be placed right after line 357.  
The same applies to [event ```emitCancel```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L474-L482).

##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L348
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L368
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L458
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L477

##### Recommendation
Emit events inside if-else blocks.
#
### L-6. Not needed function
##### Description
The bump() function seems to do nothing because '''bytes32 id'_'' is converted to "uint256 id" but not returned.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310

##### Recommendation
Consider changing the implementation of the function, or delete it.
#
### L-7. Use SafeCast Library
##### Description
Downcasting from uint256/int256 in Solidity does not revert on overflow. This can easily result in undesired exploitation or bugs, since developers usually assume that overflows raise errors. SafeCast restores this intuition by reverting the transaction when such an operation overflows.
##### Instances
- [```take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1061)

##### Recommendation
It is better to use [safe casting library](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast).
#
### L-8. Add more information in the events
##### Description
Some events are missing important information.
##### Instances
- [```emit LogSetOwner(owner);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L27) (include info about previous owner)
- [```emit LogMinSell(address(pay_gem), dust);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L960) (include info about previous minimal sell amount)
- [```emit RewardsDurationUpdated(rewardsDuration[token]);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L241) (include info about previous rewardsDuration)
- [```emit MarginIncreased(_positionId, _marginAmount);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L433) (include info about previous marginAmount

#
### L-9. Critical changes should use two-step procedure
##### Description
Lack of two-step procedure for critical operations leaves them error-prone.
##### Instances
- [```function setMinSell(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955)
- [```function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466) 
- [```function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471) 
- [```function setFeeTo(address newFeeTo) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476) 
- [```function setRewardsDuration(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232)

##### Recommendation
Consider adding two step procedure on the critical functions.
#
### L-10. Use the two-step-transfer of ownership
##### Description
If the owner accidentally transfers ownership to an incorrect address, protected functions may become permanently inaccessible.
##### Instances
- [```function setOwner(address owner_) external auth {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25) 

##### Recommendation
Consider using a two-step-transfer of ownership: the current owner would nominate a new owner, and to become the new owner, the nominated account would have to approve the change, so that the address is proven to be valid.
#
### L-11. Return the amount of funds needed to cover fee
##### Description
For convenience, the amount of funds needed to cover fee should be returned in the error message.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L341
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L356
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L361

#
### L-12. Use Checks Effects Interactions pattern
##### Description
For reducing the attack surface for malicious contracts trying to hijack control flow after an external call, it is best practice to use [this pattern](https://fravoll.github.io/solidity-patterns/checks_effects_interactions.html).
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L118-L120

##### Recommendation
Change to:
```
 _msgValue = msg.value - _feeAmount;
(bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
require(OK, "ETH transfer failed");
```
#
### L-13. Owner can renounce ownership
##### Description
Openzeppelin's Ownable.sol implements ```renounceOwnership()``` function which leaves the contract without an owner and removes any functionality that is only available to them.
##### Instances
- [```import "@openzeppelin/contracts/access/Ownable.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L7) 

##### Recommendation
Consider reimplementing the function to disable it.
#
### L-14. Add a timelock to critical functions
##### Description
Giving users time to react and adjust to critical changes in protocol provides more guarantees and increases the transparency of the protocol.
##### Instances
- [```function setMinSell(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955) 
- [```function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466) 
- [```function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471) 
- [```function setFeeTo(address newFeeTo) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476) 
- [```function setRewardsDuration(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232) 

##### Recommendation
Consider adding a timelock.
#
### L-15. Misleading comment
##### Instances
- [```//these variables are global only because of solidity local variable limit```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1270)

##### Recommendation
Probably this comment should be deleted or placed in other place.
#
### L-16. Function params shadow other (inherited) state variables
##### Description
Function param ```owner``` shadows state variable ```DSAuth.owner```.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L819
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L847
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1335

##### Recommendation
It is better to rename function parameter.

#
## Non-Critical Risk Findings(26)
### NC-1. Open questions
##### Instances
- [```// So long as all share logic is just being read from BathToken correctly, then bathbuddy can still sit on top and dish out ERC20s?```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L66) 
- [```// Implicitly staked or not depending on your bathToken share balance!?```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L67) 
- [```IERC20(myBathTokenBuddy) // Care with this?```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L146) 
- [```// Must be used before? notifyRewardAmount to set the new period```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L231) 

##### Recommendation
Resolve issues.
#
### NC-2. Rename some functions
##### Description
Some function names do not make it clear what they are for or sound too general.
##### Instances
For example:
- [```function make(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L491) 
- [```function _matcho(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1273) 
- ...

##### Recommendation
Make function names more specific.
#
### NC-3. Change type of function param
##### Description
To avoid casting uint256 to uint128, you can make the input parameter uint128.
##### Instances
- [```uint256 pay_amt,```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L512) 
- [```uint256 buy_amt,```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L514) 

#
### NC-4. Different names for the same parameter
##### Description
Some function use ```rounding``` and some - ```matching``` for the same parameters.
##### Instances
For example,
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L786
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L830

##### Recommendation
Stick to one term.
#
### NC-5. Ugly hack
##### Description
The phrase "Ugly hack" can give the impression of something unreliable/not completely correct.
##### Instances
- [```// Ugly hack to work around rounding errors. Based on the idea that```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1294)

##### Recommendation
It is better to rephrase the comment.
#
### NC-6. Limit fees
##### Description
The protocol will look more reliable if you limit the maximum possible fee values.
##### Instances
- [```function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466)
- [```function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471)

##### Recommendation
Add a check that the fee values being set do not exceed MaxFeeSize.
#
### NC-7. No need to use SafeMath
##### Description
SafeMath is no longer needed starting with Solidity 0.8. The compiler now has built in overflow checking.
##### Instances
- [```import "@openzeppelin/contracts/utils/math/SafeMath.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6)
- [```import "@openzeppelin/contracts/utils/math/SafeMath.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6)

#
### NC-8. Functions with different names make the same
##### Description
Function min() and imin() (max() and imax()) are the same.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70

#
### NC-9. Commented code
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L100-L116 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L129-L150 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L187-L195 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L207
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L300-L309 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L429-L434 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L438
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1343-L1359

##### Recommendation
Delete commented code.
#
### NC-10. Typos
##### Instances
- [```/// @notice allows 3rd party protocols to charge their own fees, from interatcions with the Rubicon Protocol```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L7) => ```interactions```

#
### NC-11. Unused variable
##### Description
Some variables declared but not used. 
##### Instances
- [```uint256 id = uint256(id_);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L298) 
- [```address _asset,```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L528)

##### Recommendation
For clarity add a comment why they are not used.
#
### NC-12. Prevent zero transfers
##### Description
Check that amount to transfer > 0.
##### Instances
- [```_offer.buy_gem.transferFrom(msg.sender, feeTo, fee),```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L340) 
- [```mFee```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L354) 
- [```_offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L360) 
- [```IERC20(asset).safeTransferFrom(msg.sender, address(this), amount);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L233) 
- [```IERC20(token).safeTransfer(msg.sender, amount);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L243) 

#
### NC-13. Order of Functions
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-functions), ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier.  
Functions should be grouped according to their visibility and ordered:
1) constructor
2) receive function (if exists)
3) fallback function (if exists)
4) external
5) public
6) internal
7) private
##### Instances
Public functions should not be placed before external:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L272-L293
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L45

External functions should not be placed after public:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L487
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L491
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L564
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L756
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L761
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L780
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L802
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L887
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L910
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L937
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L965
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L993
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1069
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1115
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L157
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L168
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232

External functions should not be placed after internal:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1484

Internal functions should not be placed before public/external:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L125

##### Recommendation
Reorder functions where possible.
#
### NC-14. Inconsistency when using the number 1000
##### Description
In some cases, numbers like 100_000 are used, and in some - 10000.
##### Instances
- [```uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L459)
- [```uint256 _fee = _minFill.mul(_feeBPS).div(10000);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L481) 
- [```_fee = _payAmount.mul(_feeBPS).div(10000);```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L490) 

##### Recommendation
Use 10_000, like in other cases.
#
### NC-15. Constants may be used
##### Description
Constants may be used instead of  literal values.
##### Instances
10 ** 9:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1057
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1059
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1099
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1101
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1142
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1144
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1175
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1177

10000:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L459
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L481
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L490

##### Recommendation
Define constant variables for repeated values.
#
### NC-16. Update import usages
##### Description
For modern and more readable code, consider updating import usages.
##### Instances
- [```import "@openzeppelin/contracts/token/ERC20/ERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L11) 
- [```import "@openzeppelin/contracts/utils/StorageSlot.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L12) 
- [```import "@openzeppelin/contracts/token/ERC20/ERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L4) 
- [```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L4) 
- [```import "@openzeppelin/contracts/token/ERC20/IERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L5) 
- [```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L4) 
- [```import "@openzeppelin/contracts/security/ReentrancyGuard.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L5) 
- [```import "@openzeppelin/contracts/utils/math/SafeMath.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6) 
- [```import "@openzeppelin/contracts/security/Pausable.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L7) 
- [```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L4) 
- [```import "@openzeppelin/contracts/token/ERC20/ERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L5) 
- [```import "@openzeppelin/contracts/utils/math/SafeMath.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6) 
- [```import "@openzeppelin/contracts/access/Ownable.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L7) 
- [```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L4) 

##### Recommendation
Change to:
```import {contract1 , contract2} from "filename.sol";```
#
### NC-17. Missing and extra leading underscores
##### Description
Internal and private constants, state variables, and functions should have a leading underscore, public state variables - should not:
##### Instances
Should have:
- [```bool locked;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224)
- [```uint256 internal feeBPS;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L227) 
- [```address internal feeTo;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L230) 
- [```bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232) 
- [```bool private initialized;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L18) 
- [```mapping(address => address) private tokenToBathToken;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L20) 
- [```mapping(address => address) private bathTokenToBuddy;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L21) 
- [```function openPosition(```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L125) 

Should not have:
- [```mapping(uint256 => sortInfo) public _rank;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691)
- [```mapping(address => mapping(address => uint256)) public _best;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L692) 
- [```mapping(address => mapping(address => uint256)) public _span;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L693) 
- [```mapping(address => uint256) public _dust;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L694) 
- [```mapping(uint256 => uint256) public _near;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L695) 
- [```uint256 public _head;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L696) 

##### Recommendation
Add and remove leading underscores where needed.
#
### NC-18.  Visibility is not set
##### Instances
- [```uint256 constant WAD = 10 ** 18;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74) 
- [```uint256 constant RAY = 10 ** 27;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L75) 
- [```bool locked;```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224) 

##### Recommendation
It is better to specify visibility explicitly.
#
### NC-19. Lack of event emission in initialize() function
##### Instances
- [```function initialize(address _feeTo) public {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700) (add event about set _feeTo)
- [```function initialize(address _comptroller, address _pAdmin) external {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32) 

##### Recommendation
Consider emitting an event in the initialize() function
#
### NC-20. Concatenate multiple lines of code
##### Description
This complicates and lengthens the code when several small pieces of code are placed on several lines.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L863-L865 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1197-L1199 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1413-L1414
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L82-L84 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L123-L126
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L59-L60 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L149-L151 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L153-L154 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L200-L202 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L203-L205 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L252-L254 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L66-L68 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L355-L357 
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L363-L365 

##### Recommendation
If you combine the specified lines with the previous ones, then they will not become too long.  
For example, it looks better:
```
uint256 _currentBathTokenAmount = IERC20(_bathToken).balanceOf(address(this));
```
than
```
uint256 _currentBathTokenAmount = IERC20(_bathToken).balanceOf(
    address(this)
);
```
#
### NC-21. Missing NatSpec
##### Description
Most of the functions do not have NatSpec or at least some comments, which makes it very difficult to understand the code.
##### Instances
- almost all functions

##### Recommendation
Add NatSpec for all functions.
#
### NC-22. Order of Layout
##### Description
According to [Order of Layout](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), inside each contract, library or interface, use the following order:
1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions
##### Instances
Modifiers should be placed before functions:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L719
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L87
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L104
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247

Events should be in the beginning of the contract not at the end:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L261-L266

Constants declaration should be placed before functions:
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75

##### Recommendation
Place modifiers, events, and constants before functions.
#
### NC-23. Unused named return variables
##### Description
Both named return variable(s) and return statement are used.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L277
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L281
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L285
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L497
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L621
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L882

##### Recommendation
To improve clarity use only named return variables.  
For example, change:
```
function functionName() returns (uint id) {
    return x;
```
to
```
function functionName() returns (uint id) {
    id = x;
```
#
### NC-24. Maximum line length exceeded
##### Description
According to [Style Guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#maximum-line-length), maximum suggested line length is 120 characters.  Longer lines make the code harder to read.
##### Instances
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L312
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L522
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L524

##### Recommendation
Make the lines shorter.
#
### NC-25. Inconsistency when using uint and uint256
##### Description
Some variables are declared as ```uint``` when majority are as ```uint256```.
##### Instances
- [```uint pos, //position to insert offer, 0 should be used if unknown```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L785) 
- [```for (uint i = 0; i < payAmts.length; i++) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899) 
- [```for (uint i = 0; i < ids.length; i++) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911) 
- [```for (uint i = 0; i < ids.length; i++) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924) 

##### Recommendation
Stick to one style.
#
### NC-26. Use mixedCase for functions and modifiers
##### Description
According to [Naming Conventions](https://docs.soliditylang.org/en/v0.8.19/style-guide.html#naming-conventions), functions and modifiers should use mixedCase ([See1](https://docs.soliditylang.org/en/v0.8.19/style-guide.html#function-names) and [See2](https://docs.soliditylang.org/en/v0.8.19/style-guide.html#modifier-names)).
##### Instances
- [```modifier can_buy(uint256 id) virtual {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L245) 
- [```modifier can_cancel(uint256 id) virtual {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251) 
- [```modifier can_offer() virtual {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260) 
- [```function _next_id() internal returns (uint256) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568) 
- [```modifier can_offer() override {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L597) 
- [```modifier can_buy(uint256 id) override {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L603) 
- [```modifier can_cancel(uint256 id) virtual override {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L610) 
- [```modifier can_cancel(uint256 id) override {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L719) 
- [```function del_rank(uint256 id) external returns (bool) {```](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L937) 

##### Recommendation
For example, change to:
```modifier canBuy(uint256 id) virtual {```
#