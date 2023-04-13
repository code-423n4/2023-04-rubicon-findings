# Gas Optimizations Report

Report Date: 2023-04-13 16:42:46

| |Issue|Instances|
|-|:-|:-:|
|[G-01]|x += y or x -= y costs more gas than x = x + y or x = x - y for state variables|2|
|[G-02]|Splitting require() statements that use `&&` saves gas|5|
|[G-03]|Use custom errors rather than revert()/require() strings to save gas|61|
|[G-04]|Functions guaranteed to revert when called by normal users can be marked payable|8|
|[G-05]|Multiple accesses of a `mapping/array` should use a local variable cache|3|
|[G-06]|Use a more recent version of solidity|1|
|[G-07]|++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops|7|
|[G-08]|Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate|4|
|[G-09]|public function that could be declared external|4|
|[G-10]| Optimize names to save gas|5|

## [G-01] x += y or x -= y costs more gas than x = x + y or x = x - y for state variables

### Impact
Using the addition operator instead of plus-equals saves 113 gas. Usually does not work with struct and mappings.

### Findings
Total:2

[contracts/utilities/poolsUtility/Position.sol#L431](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L431)
```solidity
431:    positions[_positionId].bathTokenAmount += _bathTokenAmount;
```
[contracts/utilities/poolsUtility/Position.sol#L560](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L560)
```solidity
560:    _assetAmount += _loopBorrowed;
```



## [G-02] Splitting require() statements that use `&&` saves gas

### Impact
When using `&&` in a `require()` statement, the entire statement will be evaluated before the transaction reverts. If the first condition is false, the second condition will not be evaluated. This means that if the first condition is false, the second condition will not be evaluated, which is a waste of gas. Splitting the `require()` statement into two separate statements will save gas.

### Findings
Total:5

[contracts/RubiconMarket.sol#L1412-L1415](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1412-L1415)
```solidity
1412:    require(
1413:                _rank[id].delb == 0 && //assert id is in the sorted list
1414:                    isOfferSorted(id)
1415:            );
```
[contracts/RubiconMarket.sol#L893-L898](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L893-L898)
```solidity
893:    require(
894:                payAmts.length == payGems.length &&
895:                    payAmts.length == buyAmts.length &&
896:                    payAmts.length == buyGems.length,
897:                "Array lengths do not match"
898:            );
```
[contracts/RubiconMarket.sol#L940-L944](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L940-L944)
```solidity
940:    require(
941:                !isActive(id) &&
942:                    _rank[id].delb != 0 &&
943:                    _rank[id].delb < block.number - 10
944:            );
```
[contracts/periphery/BathBuddy.sol#L95-L100](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L95-L100)
```solidity
95:    require(
96:                msg.sender == myBathTokenBuddy &&
97:                    msg.sender != address(0) &&
98:                    friendshipStarted,
99:                "You are not my buddy!"
100:            );
```
[contracts/utilities/poolsUtility/Position.sol#L591-L598](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L591-L598)
```solidity
591:    require(
592:                    _leverage > _wad && _leverage <= _leverageMax,
593:                    "_leverageCheck{Long}: INVLAID LEVERAGE"
594:                )
595:                : require(
596:                    _leverage >= _wad && _leverage <= _leverageMax,
597:                    "_leverageCheck{Short}: INVLAID LEVERAGE"
598:                );
```


## [G-03] Use custom errors rather than revert()/require() strings to save gas

### Impact
Custom errors are available from solidity version 0.8.4. Custom errors save [~50 gas](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas. 

### Findings
Total:61

[contracts/RubiconMarket.sol#L379-L382](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L379-L382)
```solidity
379:    require(
380:                _offer.pay_gem.transfer(msg.sender, quantity),
381:                "_offer.pay_gem.transfer(msg.sender, quantity) failed"
382:            );
```
[contracts/RubiconMarket.sol#L322](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L322)
```solidity
322:    require(uint128(quantity) == quantity, "quantity is not an int");
```
[contracts/RubiconMarket.sol#L31](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L31)
```solidity
31:    require(isAuthorized(msg.sender), "ds-auth-unauthorized");
```
[contracts/RubiconMarket.sol#L339-L342](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L339-L342)
```solidity
339:    require(
340:                _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
341:                "Insufficient funds to cover fee"
342:            );
```
[contracts/RubiconMarket.sol#L879](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L879)
```solidity
879:    require(_hide(id), "can't hide");
```
[contracts/RubiconMarket.sol#L893-L898](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L893-L898)
```solidity
893:    require(
894:                payAmts.length == payGems.length &&
895:                    payAmts.length == buyAmts.length &&
896:                    payAmts.length == buyGems.length,
897:                "Array lengths do not match"
898:            );
```
[contracts/RubiconMarket.sol#L51](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L51)
```solidity
51:    require((z = x - y) <= x, "ds-math-sub-underflow");
```
[contracts/RubiconMarket.sol#L374-L377](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L374-L377)
```solidity
374:    require(
375:                _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
376:                "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
377:            );
```
[contracts/RubiconMarket.sol#L350-L357](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L350-L357)
```solidity
350:    require(
351:                        _offer.buy_gem.transferFrom(
352:                            msg.sender,
353:                            _offer.recipient,
354:                            mFee
355:                        ),
356:                        "Insufficient funds to cover fee"
357:                    );
```
[contracts/RubiconMarket.sol#L720](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L720)
```solidity
720:    require(isActive(id), "Offer was deleted or taken, or never existed.");
```
[contracts/RubiconMarket.sol#L834](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L834)
```solidity
834:    require(!locked, "Reentrancy attempt");
```
[contracts/RubiconMarket.sol#L859](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L859)
```solidity
859:    require(!locked, "Reentrancy attempt");
```
[contracts/RubiconMarket.sol#L874](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L874)
```solidity
874:    require(!locked, "Reentrancy attempt");
```
[contracts/RubiconMarket.sol#L1107-L1110](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1107-L1110)
```solidity
1107:    require(
1108:                fill_amt <= max_fill_amount,
1109:                "fill_amt exceeds max_fill_amount"
1110:            );
```
[contracts/RubiconMarket.sol#L1065](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1065)
```solidity
1065:    require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");
```
[contracts/RubiconMarket.sol#L1080](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1080)
```solidity
1080:    require(offerId != 0, "offerId == 0");
```
[contracts/RubiconMarket.sol#L613-L619](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L613-L619)
```solidity
613:    require(
614:                isClosed() ||
615:                    msg.sender == getOwner(id) ||
616:                    id == dustId ||
617:                    (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
618:                "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
619:            );
```
[contracts/RubiconMarket.sol#L721-L727](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L721-L727)
```solidity
721:    require(
722:                isClosed() ||
723:                    msg.sender == getOwner(id) ||
724:                    id == dustId ||
725:                    (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
726:                "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
727:            );
```
[contracts/RubiconMarket.sol#L359-L362](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L359-L362)
```solidity
359:    require(
360:                        _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),
361:                        "Insufficient funds to cover fee"
362:                    );
```
[contracts/RubiconMarket.sol#L321](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L321)
```solidity
321:    require(uint128(spend) == spend, "spend is not an int");
```
[contracts/RubiconMarket.sol#L55](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L55)
```solidity
55:    require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");
```
[contracts/RubiconMarket.sol#L701](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L701)
```solidity
701:    require(!initialized, "contract is already initialized");
```
[contracts/RubiconMarket.sol#L1040](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1040)
```solidity
1040:    require(offerId != 0, "0 offerId");
```
[contracts/RubiconMarket.sol#L47](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L47)
```solidity
47:    require((z = x + y) >= x, "ds-math-add-overflow");
```
[contracts/RubiconMarket.sol#L1444](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1444)
```solidity
1444:    require(!isOfferSorted(id), "offer sorted");
```
[contracts/BathHouseV2.sol#L76-L79](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L76-L79)
```solidity
76:    require(
77:                implementation != address(0),
78:                "createBathToken: IMPLEMENTATION == ADDRESS 0"
79:            );
```
[contracts/BathHouseV2.sol#L68-L71](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L68-L71)
```solidity
68:    require(
69:                tokenToBathToken[underlying] == address(0),
70:                "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
71:            );
```
[contracts/BathHouseV2.sol#L27](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L27)
```solidity
27:    require(msg.sender == admin, "onlyAdmin: !admin");
```
[contracts/BathHouseV2.sol#L33](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L33)
```solidity
33:    require(!initialized, "BathHouseV2 already initialized!");
```
[contracts/BathHouseV2.sol#L144](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L144)
```solidity
144:    require(_underlying != address(0), "_bathify: ADDRESS ZERO");
```
[contracts/BathHouseV2.sol#L72-L75](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L72-L75)
```solidity
72:    require(
73:                underlying != address(0),
74:                "createBathToken: UNDERLYING == ADDRESS 0"
75:            );
```
[contracts/V2Migrator.sol#L41](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L41)
```solidity
41:    require(bathBalance > 0, "migrate: ZERO AMOUNT");
```
[contracts/V2Migrator.sol#L63-L66](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L63-L66)
```solidity
63:    require(
64:                IERC20(bathTokenV2).balanceOf(address(this)) == 0,
65:                "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
66:            );
```
[contracts/V2Migrator.sol#L54-L57](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L54-L57)
```solidity
54:    require(
55:                CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
56:                "migrate: MINT FAILED"
57:            );
```
[contracts/periphery/BathBuddy.sol#L95-L100](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L95-L100)
```solidity
95:    require(
96:                msg.sender == myBathTokenBuddy &&
97:                    msg.sender != address(0) &&
98:                    friendshipStarted,
99:                "You are not my buddy!"
100:            );
```
[contracts/periphery/BathBuddy.sol#L122](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L122)
```solidity
122:    require(friendshipStarted, "I have not started a bathToken friendship");
```
[contracts/periphery/BathBuddy.sol#L143](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L143)
```solidity
143:    require(friendshipStarted, "I have not started a bathToken friendship");
```
[contracts/periphery/BathBuddy.sol#L236-L239](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L236-L239)
```solidity
236:    require(
237:                block.timestamp > periodFinish[token],
238:                "Previous rewards period must be complete before changing the duration for the new period"
239:            );
```
[contracts/periphery/BathBuddy.sol#L76](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L76)
```solidity
76:    require(!friendshipStarted, "I already have a buddy!");
```
[contracts/periphery/BathBuddy.sol#L105](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L105)
```solidity
105:    require(msg.sender == bathHouse, "You are not my beloved bath house!");
```
[contracts/periphery/BathBuddy.sol#L217-L221](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L217-L221)
```solidity
217:    require(
218:                rewardRates[address(rewardsToken)] <=
219:                    balance.div(rewardsDuration[address(rewardsToken)]),
220:                "Provided reward too high"
221:            );
```
[contracts/utilities/poolsUtility/Position.sol#L359-L362](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L359-L362)
```solidity
359:    require(
360:                CErc20Interface(_bathToken).mint(_amount) == 0,
361:                "_supply: MINT FAILED"
362:            );
```
[contracts/utilities/poolsUtility/Position.sol#L591-L598](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L591-L598)
```solidity
591:    require(
592:                    _leverage > _wad && _leverage <= _leverageMax,
593:                    "_leverageCheck{Long}: INVLAID LEVERAGE"
594:                )
595:                : require(
596:                    _leverage >= _wad && _leverage <= _leverageMax,
597:                    "_leverageCheck{Short}: INVLAID LEVERAGE"
598:                );
```
[contracts/utilities/poolsUtility/Position.sol#L344](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L344)
```solidity
344:    require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");
```
[contracts/utilities/poolsUtility/Position.sol#L76](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L76)
```solidity
76:    require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");
```
[contracts/utilities/poolsUtility/Position.sol#L114-L117](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L114-L117)
```solidity
114:    require(
115:                openPosition(asset, quote, assetPayAmount, leverage),
116:                "sellAllAmountWithLeverage: FAILED TO OPEN POSITION"
117:            );
```
[contracts/utilities/poolsUtility/Position.sol#L312](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L312)
```solidity
312:    require(_err == 0, "_maxBorrow: ERROR");
```
[contracts/utilities/poolsUtility/Position.sol#L318](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L318)
```solidity
318:    require(_max > 0, "_maxBorrow: can't borrow 0");
```
[contracts/utilities/poolsUtility/Position.sol#L228](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L228)
```solidity
228:    require(pos.isActive, "increaseMargin: POS ISN'T ACTIVE");
```
[contracts/utilities/poolsUtility/Position.sol#L288-L291](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L288-L291)
```solidity
288:    require(
289:                _amountToRepay <= _quoteBalance,
290:                "_repay: balance of quote lt. debt"
291:            );
```
[contracts/utilities/poolsUtility/Position.sol#L298-L301](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L298-L301)
```solidity
298:    require(
299:                CErc20Interface(_bathTokenQuote).repayBorrow(_amountToRepay) == 0,
300:                "_repay: ERROR"
301:            );
```
[contracts/utilities/poolsUtility/Position.sol#L313](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L313)
```solidity
313:    require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
```
[contracts/utilities/poolsUtility/Position.sol#L212](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L212)
```solidity
212:    require(pos.isActive, "closePosition: POS ISN'T ACTIVE");
```
[contracts/utilities/poolsUtility/Position.sol#L273-L276](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L273-L276)
```solidity
273:    require(
274:                CErc20Interface(_cToken).borrow(_amount) == 0,
275:                "_borrow: BORROW FAILED"
276:            );
```
[contracts/utilities/poolsUtility/Position.sol#L314](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L314)
```solidity
314:    require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");
```
[contracts/utilities/poolsUtility/Position.sol#L385-L388](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L385-L388)
```solidity
385:    require(
386:                CErc20Interface(_bathTokenAsset).redeem(_bathTokenAmount) == 0,
387:                "_redeem: REDEEM FAILED"
388:            );
```
[contracts/utilities/poolsUtility/Position.sol#L100-L103](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L100-L103)
```solidity
100:    require(
101:                openPosition(quote, asset, quotePayAmount, leverage),
102:                "buyAllAmountWithLeverage: FAILED TO OPEN POSITION"
103:            );
```
[contracts/utilities/FeeWrapper.sol#L115](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L115)
```solidity
115:    require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
```
[contracts/utilities/FeeWrapper.sol#L86](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L86)
```solidity
86:    require(_OK, "low-level call to the router failed");
```
[contracts/utilities/FeeWrapper.sol#L119](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L119)
```solidity
119:    require(OK, "ETH transfer failed");
```
[contracts/utilities/FeeWrapper.sol#L70](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L70)
```solidity
70:    require(_OK, "low-level call to the Rubicon failed");
```

### Recommendation
Using Custom errors

## [G-04] Functions guaranteed to revert when called by normal users can be marked payable

### Impact
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
   The extra opcodes avoided are `CALLVALUE(2)`,`DUP1(3)`,`ISZERO(3)`,`PUSH2(3)`,`JUMPI(10)`,`PUSH1(3)`,`DUP1(3)`,`REVERT(0)`,`JUMPDEST(1)`,`POP(2)`, which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

### Findings
Total:8

[contracts/BathHouseV2.sol#L60](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L60)
```solidity
60:    function createBathToken(
```
[contracts/periphery/BathBuddy.sol#L232](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L232)
```solidity
232:    function setRewardsDuration(
```
[contracts/periphery/BathBuddy.sol#L191](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L191)
```solidity
191:    function notifyRewardAmount(
```
[contracts/utilities/poolsUtility/Position.sol#L93](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L93)
```solidity
93:    function buyAllAmountWithLeverage(
```
[contracts/utilities/poolsUtility/Position.sol#L210](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L210)
```solidity
210:    function closePosition(uint256 posId) external onlyOwner {
```
[contracts/utilities/poolsUtility/Position.sol#L107](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L107)
```solidity
107:    function sellAllAmountWithLeverage(
```
[contracts/utilities/poolsUtility/Position.sol#L242](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L242)
```solidity
242:    function withdraw(address token, uint256 amount) external onlyOwner {
```
[contracts/utilities/poolsUtility/Position.sol#L226](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L226)
```solidity
226:    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
```

### Recommendation
Mark the function as payable.

## [G-05] Multiple accesses of a `mapping/array` should use a local variable cache

### Impact
The instances below point to the second+ access of a value inside a mapping/array, within a function. Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into `memory/calldata` 

### Findings
Total:3

[contracts/RubiconMarket.sol#L691-L948](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L691-L948)
```solidity
691:    mapping(uint256 => sortInfo) public _rank;

937:		function del_rank(uint256 id) external returns (bool) {
			require(!locked);

			require(
				!isActive(id) &&
					_rank[id].delb != 0 &&
					_rank[id].delb < block.number - 10
			);
			delete _rank[id];
			emit LogDelete(msg.sender, id);
			return true;
948:		}
```
[contracts/RubiconMarket.sol#L692-L1417](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L692-L1417)
```solidity
692:    mapping(address => mapping(address => uint256)) public _best;
...
1273:    function _matcho(
...
1289:        while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
                best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];
...
1405:    function _unsort(
...
1417:	    if (id != _best[pay_gem][buy_gem]) {
				// offers[id] is not the highest offer
				require(_rank[_rank[id].next].prev == id);
				_rank[_rank[id].next].prev = _rank[id].prev;
			} else {
				//offers[id] is the highest offer
				_best[pay_gem][buy_gem] = _rank[id].prev;
			}
```
[contracts/periphery/BathBuddy.sol#L54-L224](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L54-L224)
```solidity
54:    mapping(address => uint256) public rewardsDuration;
...
196:            rewardRates[address(rewardsToken)] = reward.div(
                rewardsDuration[address(rewardsToken)]
            );

206:            rewardRates[address(rewardsToken)] = reward.add(leftover).div(
                rewardsDuration[address(rewardsToken)]
            );

217:    require(
            rewardRates[address(rewardsToken)] <=
                balance.div(rewardsDuration[address(rewardsToken)]),
            "Provided reward too high"
        );


224:    periodFinish[address(rewardsToken)] = block.timestamp.add(
            rewardsDuration[address(rewardsToken)]
        );
```


## [G-06] Use a more recent version of solidity

### Impact
 - Use a solidity version of at least `0.8.2` to get simple compiler automatic inlining 
 - Use a solidity version of at least `0.8.3` to get better struct packing and cheaper multiple storage reads 
  - Use a solidity version of at least `0.8.4` to get custom errors,  which are cheaper at deployment than `revert()/require()` strings  
 - Use a solidity version of at least `0.8.10` to have external calls skip contract existence checks  if the external call has a return value 
 

### Findings
Total:1

[contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L2)
```solidity
2:    pragma solidity ^0.8.0;
```

### Recommendation
Current version: 0.8.17 (2022-11)


## [G-07] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

### Impact
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked). 

### Findings
Total:7

[contracts/RubiconMarket.sol#L899](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L899)
```solidity
899:    for (uint i = 0; i < payAmts.length; i++) {
```
[contracts/RubiconMarket.sol#L911](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L911)
```solidity
911:    for (uint i = 0; i < ids.length; i++) {
```
[contracts/RubiconMarket.sol#L924](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L924)
```solidity
924:    for (uint i = 0; i < ids.length; i++) {
```
[contracts/BathHouseV2.sol#L122](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L122)
```solidity
122:    for (uint256 i = 0; i < buddies.length; ++i) {
```
[contracts/V2Migrator.sol#L31](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L31)
```solidity
31:    for (uint256 i = 0; i < bathTokensV1.length; ++i) {
```
[contracts/utilities/poolsUtility/Position.sol#L158](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L158)
```solidity
158:    for (uint256 i = 0; i < vars.limit; ++i) {
```
[contracts/utilities/FeeWrapper.sol#L40](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L40)
```solidity
40:    for (uint256 i = 0; i < tokenAmounts.length; ++i) {
```

## [G-08] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

### Impact
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to [not having to recalculate the key's keccak256 hash](https://gist.github.com/IllIllI000/ec23a57daa30a8f8ca8b9681c8ccefb0) (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

### Findings
Total:4

[contracts/RubiconMarket.sol#L691-L694](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L691-L694)
```solidity
691:    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692:        mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693:        mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694:        mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
```
[contracts/BathHouseV2.sol#L20-L21](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L20-L21)
```solidity
20:    mapping(address => address) private tokenToBathToken;
21:        mapping(address => address) private bathTokenToBuddy;
```
[contracts/periphery/BathBuddy.sol#L59-L61](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L59-L61)
```solidity
59:    mapping(address => mapping(address => uint256))
60:            public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61:        mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```
[contracts/periphery/BathBuddy.sol#L52-L56](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L52-L56)
```solidity
52:    mapping(address => uint256) public periodFinish; // Token specific
53:        mapping(address => uint256) public rewardRates; // Token specific reward rates
54:        mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:        mapping(address => uint256) public lastUpdateTime; //Token specific
56:        mapping(address => uint256) public rewardsPerTokensStored; // Token specific
```

## [G-09] public function that could be declared external

### Impact
`public` functions that are never called by the contract should be declared `external`, and its immutable parameters should be located in calldata to save gas. (see [here](https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-external))

### Findings
Total:4

[contracts/RubiconMarket.sol#L574](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L574)
```solidity
574:    function getFeeBPS() public view returns (uint256) {
```
[contracts/RubiconMarket.sol#L624](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L624)
```solidity
624:    function getTime() public view returns (uint64) {
```
[contracts/RubiconMarket.sol#L1010](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1010)
```solidity
1010:    function getFirstUnsortedOffer() public view returns (uint256) {
```
[contracts/utilities/poolsUtility/Position.sol#L86](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L86)
```solidity
86:    function borrowRate(address bathToken) public view returns (uint256 rate) {
```

### Recommendation
Use the `external` attribute for functions never called from the contract, and change the location of immutable parameters to `calldata` to save gas.

## [G-10] Optimize names to save gas

### Impact
public/external function names and public member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call

### Findings
Total:5

[contracts/periphery/BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol)
```solidity
38:    contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
```

| Function Signature | Function Name |
|--------------------|---------------|
| 0x093b8e | getReward(IERC20 token, address recipient) |
| 0xd4f21c | earned( address account, address token) |
| 0x2b9044 | spawnBuddy(address _owner,address newBud,address _bathHouse) |
| 0xbf9322 | lastTimeRewardApplicable(address token) |
| 0x732d7c | rewardPerToken(address token) |
| 0xd4f21c | earned(address account,address token) |
| 0x279b38 | getRewardForDuration(address token) |
| 0xd8409d | getReward(IERC20 rewardsToken,address holderRecipient) |
| 0xf005ec | notifyRewardAmount(uint256 reward,IERC20 rewardsToken ) |
| 0xb0dd14 | setRewardsDuration(uint256 _rewardsDuration,address token) |

[contracts/BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol)
```solidity
12:    contract BathHouseV2 {
```
| Function Signature | Function Name |
|--------------------|---------------|
| 0x5dac62 | initialize(address _comptroller, address _pAdmin) |
| 0xf78c98 | getBathTokenFromAsset(address asset) |
| 0x846ada | whoIsBuddy(address bathToken) |
| 0xf8f60b | createBathToken(address underlying,InterestRateModel interestRateModel,uint256 initialExchangeRateMantissa,address implementation,bytes memory becomeImplementationData) |
| 0xb22983 | claimRewards(address[] memory buddies,address[] memory rewardsTokens) |
| 0x2ff5b2 | getReward(address buddy, address rewardsToken) |

[contracts/V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol)
```solidity
8:    interface IBathToken is IERC20 {
```

| Function Signature | Function Name |
|--------------------|---------------|
| 0x3db42a | withdraw(uint256 shares) |
| 0x2495a5 | underlyingToken() |

[contracts/utilities/poolsUtility/Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)
```solidity
15:   contract Position is Ownable, DSMath {
```


| Function Signature | Function Name |
|--------------------|---------------|
| 0x2b6de9 | borrowBalance(address bathToken) |
| 0x7d3620 | borrowBalanceOfPos(uint256 posId) |
| 0x93fd92 | borrowRate(address bathToken) |
| 0xce0833 | buyAllAmountWithLeverage(address quote,address asset,uint256 quotePayAmount,uint256 leverage) |
| 0x3bebb0 | sellAllAmountWithLeverage(address asset,address quote,uint256 assetPayAmount,uint256 leverage) |
| 0xe00eb5 | openPosition(address asset,address quote,uint256 initMargin,uint256 leverage) |
| 0x958043 | closePosition(uint256 posId) |
| 0x9c9bd6 | increaseMargin(uint256 posId, uint256 amount) |
| 0x04d59c | withdraw(address token, uint256 amount) |

[contracts/RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)
```solidity
22:      contract DSAuth is DSAuthEvents {

45:      contract DSMath {

218:     contract SimpleMarket is EventfulMarket, DSMath {

593:     contract ExpiringMarket is DSAuth, SimpleMarket {

674:     contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

```
| Function Signature | Function Name |
|--------------------|---------------|
| 0x472de9 | setOwner(address owner_) |
| 0x260111 | isAuthorized(address src) |
| 0x156854 | add(uint256 x, uint256 y) |
| 0x19100d | sub(uint256 x, uint256 y) |
| 0x83f76a | mul(uint256 x, uint256 y) |
| 0x484a25 | min(uint256 x, uint256 y) |
| 0x2f3be4 | max(uint256 x, uint256 y) |
| 0x7aa940 | imin(int256 x, int256 y) |
| 0xdd7977 | imax(int256 x, int256 y) |
| 0xf196bd | wmul(uint256 x, uint256 y) |
| 0xfe6de0 | rmul(uint256 x, uint256 y) |
| 0x96e785 | wdiv(uint256 x, uint256 y) |
| 0xdd1e98 | rdiv(uint256 x, uint256 y) |
| 0xfc741c | makerFee() |
| 0xe2e8b2 | isActive(uint256 id) |
| 0xe7400c | getOwner(uint256 id) |
| 0x6abf9f | getRecipient(uint256 id) |
| 0x256c39 | getOffer(uint256 id) |
| 0x7f23cf | bump(bytes32 id_) |
| 0x1476b8 | buy(uint256 id,uint256 quantity) |
| 0xd89151 | cancel(uint256 id) |
| 0x8748ab | kill(bytes32 id) |
| 0x5e4b9e | make(ERC20 pay_gem,ERC20 buy_gem,uint128 pay_amt,uint128 buy_amt) |
| 0x101e83 | offer(uint256 pay_amt,ERC20 pay_gem,uint256 buy_amt,ERC20 buy_gem,address owner,address recipient) |
| 0x1e5aba | take(bytes32 id, uint128 maxTakeAmount) |
| 0x6f7c18 | getFeeBPS() |
| 0x452ec7 | calcAmountAfterFee(uint256 amount) |
| 0xc2b6b5 | isClosed() |
| 0x557ed1 | getTime() |
| 0x07da68 | stop() |
| 0x869b76 | initialize(address _feeTo) |
| 0x5e4b9e | make(ERC20 pay_gem, ERC20 buy_gem, uint128 pay_amt,uint128 buy_amt) |
| 0x1e5aba | take(bytes32 id, uint128 maxTakeAmount) |
| 0x8748ab | kill(bytes32 id) |
| 0x91d269 | buy(uint256 id, uint256 amount) |
| 0xd89151 | cancel(uint256 id) |
| 0x7587f5 | batchOffer(uint[] calldata payAmts,address[] calldata payGems,uint[] calldata buyAmts,address[] calldata buyGems) |
| 0xc3e921 | batchCancel(uint[] calldata ids) |
| 0x3dcdd6 | batchRequote(uint[] calldata ids,uint[] calldata payAmts,address[] calldata payGems,uint[] calldata buyAmts,address[] calldata buyGems) |
| 0x24c064 | del_rank(uint256 id) |
| 0x91b4f8 | getMinSell(ERC20 pay_gem) |
| 0xf1862b | getBestOffer( ERC20 sell_gem,ERC20 buy_gem) |
| 0xd372d3 | getWorseOffer(uint256 id) |
| 0x0216e0 | getBetterOffer(uint256 id) |
| 0xe26282 | getOfferCount(ERC20 sell_gem,ERC20 buy_gem) |
| 0x8af82a | getFirstUnsortedOffer() |
| 0xa7ea1e | getNextUnsortedOffer(uint256 id) |
| 0x94210c | isOfferSorted(uint256 id) |
| 0xfe5b3d | sellAllAmount( ERC20 pay_gem,uint256 pay_amt,ERC20 buy_gem,uint256 min_fill_amount) |
| 0x635485 | buyAllAmount(ERC20 buy_gem,uint256 buy_amt,ERC20 pay_gem,uint256 max_fill_amount) |
| 0xaae2e5 | getBuyAmountWithFee(ERC20 buy_gem,ERC20 pay_gem,uint256 pay_amt) |
| 0x0f135a | getBuyAmount(ERC20 buy_gem,ERC20 pay_gem,uint256 pay_amt ) |
| 0xd4cad3 | getPayAmountWithFee(ERC20 pay_gem,ERC20 buy_gem,uint256 buy_amt) |
| 0x5eea62 | getPayAmount(ERC20 pay_gem,ERC20 buy_gem,uint256 buy_amt) |
| 0x66038d | setFeeBPS(uint256 _newFeeBPS) |
| 0xfd468a | setMakerFee(uint256 _newMakerFee) |
| 0x325a55 | setFeeTo(address newFeeTo) |
| 0x6611f5 | getFeeTo() |
