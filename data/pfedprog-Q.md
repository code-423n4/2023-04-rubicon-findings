## [L-1]: Unsafe ERC20 Operation(s)

ERC20 operations can be unsafe due to different implementations and vulnerabilities in the standard.

It is therefore recommended to always either use OpenZeppelin's SafeERC20 library or at least to wrap each operation in a require statement.

To circumvent ERC20's approve functions race-condition vulnerability use OpenZeppelin's SafeERC20 library's safe{Increase|Decrease}Allowance functions.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L340 => \_offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L351 => offer.buy_gem.transferFrom(
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L360 => offer.buy_gem.transferFrom(msg.sender, offer.owner, mFee),
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L375 => offer.buy_gem.transferFrom(msg.sender, offer.recipient, spend),
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L380 => offer.pay_gem.transfer(msg.sender, quantity),
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L53 => underlying.approve(bathTokenV2, amountWithdrawn);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L59 => IERC20(bathTokenV2).transfer(
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L100 => IERC20(feeToken).transferFrom(msg.sender, address(this), totalAmount);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102 => IERC20(feeToken).transfer(feeTo, feeAmount);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L105 => IERC20(feeToken).approve(target, (totalAmount - feeAmount));

## [L-2]: Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathBuddy.sol#L2 => Pragma version^0.8.0
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2 => pragma solidity ^0.8.9;

## [N-1] Open TODOs

in https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L302

## [N-2]: Function state mutability can be restricted to view

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol:297:5: function bump(bytes32 id*) external can_buy(uint256(id*)) {

## [N-3]: Function state mutability can be restricted to pure

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol:28:5: function calculateFee(

## [N-4]: Conformance to Solidity naming conventions

Solidity defines a [naming convention](https://solidity.readthedocs.io/en/v0.4.25/style-guide.html#naming-conventions) that should be followed.

Function arguments should use mixedCase. Examples: initialSupply, account, recipientAddress, senderAddress, newOwner.

```
Parameter BathBuddy.spawnBuddy(address,address,address)._owner (contracts/periphery/BathBuddy.sol#72) is not in mixedCase
Parameter BathBuddy.spawnBuddy(address,address,address)._bathHouse (contracts/periphery/BathBuddy.sol#74) is not in mixedCase
Parameter BathBuddy.setRewardsDuration(uint256,address)._rewardsDuration (contracts/periphery/BathBuddy.sol#233) is not in mixedCase

Parameter BathHouseV2.initialize(address,address).\_comptroller (contracts/BathHouseV2.sol#32) is not in mixedCase
Parameter BathHouseV2.initialize(address,address).\_pAdmin (contracts/BathHouseV2.sol#32) is not in mixedCase

Parameter SimpleMarket.make(ERC20,ERC20,uint128,uint128).pay_gem (contracts/RubiconMarket.sol#492) is not in mixedCase
Parameter SimpleMarket.make(ERC20,ERC20,uint128,uint128).buy_gem (contracts/RubiconMarket.sol#493) is not in mixedCase
Parameter SimpleMarket.make(ERC20,ERC20,uint128,uint128).pay_amt (contracts/RubiconMarket.sol#494) is not in mixedCase
Parameter SimpleMarket.make(ERC20,ERC20,uint128,uint128).buy_amt (contracts/RubiconMarket.sol#495) is not in mixedCase
Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).pay_amt (contracts/RubiconMarket.sol#512) is not in mixedCase
Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).pay_gem (contracts/RubiconMarket.sol#513) is not in mixedCase
Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).buy_amt (contracts/RubiconMarket.sol#514) is not in mixedCase
Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).buy_gem (contracts/RubiconMarket.sol#515) is not in mixedCase

Parameter RubiconMarket.initialize(address)._feeTo (contracts/RubiconMarket.sol#700) is not in mixedCase
Parameter RubiconMarket.make(ERC20,ERC20,uint128,uint128).pay_gem (contracts/RubiconMarket.sol#734) is not in mixedCase
Parameter RubiconMarket.make(ERC20,ERC20,uint128,uint128).buy_gem (contracts/RubiconMarket.sol#735) is not in mixedCase
Parameter RubiconMarket.make(ERC20,ERC20,uint128,uint128).pay_amt (contracts/RubiconMarket.sol#736) is not in mixedCase
Parameter RubiconMarket.make(ERC20,ERC20,uint128,uint128).buy_amt (contracts/RubiconMarket.sol#737) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20).pay_amt (contracts/RubiconMarket.sol#762) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20).pay_gem (contracts/RubiconMarket.sol#763) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20).buy_amt (contracts/RubiconMarket.sol#764) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20).buy_gem (contracts/RubiconMarket.sol#765) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool).pay_amt (contracts/RubiconMarket.sol#781) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool).pay_gem (contracts/RubiconMarket.sol#782) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool).buy_amt (contracts/RubiconMarket.sol#783) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool).buy_gem (contracts/RubiconMarket.sol#784) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,address,address).pay_amt (contracts/RubiconMarket.sol#803) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,address,address).pay_gem (contracts/RubiconMarket.sol#804) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,address,address).buy_amt (contracts/RubiconMarket.sol#805) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,address,address).buy_gem (contracts/RubiconMarket.sol#806) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).pay_amt (contracts/RubiconMarket.sol#825) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).pay_gem (contracts/RubiconMarket.sol#826) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).buy_amt (contracts/RubiconMarket.sol#827) is not in mixedCase
Parameter RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).buy_gem (contracts/RubiconMarket.sol#828) is not in mixedCase
Parameter RubiconMarket.setMinSell(ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#956) is not in mixedCase
Parameter RubiconMarket.getMinSell(ERC20).pay_gem (contracts/RubiconMarket.sol#966) is not in mixedCase
Parameter RubiconMarket.getBestOffer(ERC20,ERC20).sell_gem (contracts/RubiconMarket.sol#975) is not in mixedCase
Parameter RubiconMarket.getBestOffer(ERC20,ERC20).buy_gem (contracts/RubiconMarket.sol#976) is not in mixedCase
Parameter RubiconMarket.getOfferCount(ERC20,ERC20).sell_gem (contracts/RubiconMarket.sol#999) is not in mixedCase
Parameter RubiconMarket.getOfferCount(ERC20,ERC20).buy_gem (contracts/RubiconMarket.sol#1000) is not in mixedCase
Parameter RubiconMarket.sellAllAmount(ERC20,uint256,ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#1029) is not in mixedCase
Parameter RubiconMarket.sellAllAmount(ERC20,uint256,ERC20,uint256).pay_amt (contracts/RubiconMarket.sol#1030) is not in mixedCase
Parameter RubiconMarket.sellAllAmount(ERC20,uint256,ERC20,uint256).buy_gem (contracts/RubiconMarket.sol#1031) is not in mixedCase
Parameter RubiconMarket.sellAllAmount(ERC20,uint256,ERC20,uint256).min_fill_amount (contracts/RubiconMarket.sol#1032) is not in mixedCase
Parameter RubiconMarket.buyAllAmount(ERC20,uint256,ERC20,uint256).buy_gem (contracts/RubiconMarket.sol#1070) is not in mixedCase
Parameter RubiconMarket.buyAllAmount(ERC20,uint256,ERC20,uint256).buy_amt (contracts/RubiconMarket.sol#1071) is not in mixedCase
Parameter RubiconMarket.buyAllAmount(ERC20,uint256,ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#1072) is not in mixedCase
Parameter RubiconMarket.buyAllAmount(ERC20,uint256,ERC20,uint256).max_fill_amount (contracts/RubiconMarket.sol#1073) is not in mixedCase
Parameter RubiconMarket.getBuyAmountWithFee(ERC20,ERC20,uint256).buy_gem (contracts/RubiconMarket.sol#1116) is not in mixedCase
Parameter RubiconMarket.getBuyAmountWithFee(ERC20,ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#1117) is not in mixedCase
Parameter RubiconMarket.getBuyAmountWithFee(ERC20,ERC20,uint256).pay_amt (contracts/RubiconMarket.sol#1118) is not in mixedCase
Parameter RubiconMarket.getBuyAmount(ERC20,ERC20,uint256).buy_gem (contracts/RubiconMarket.sol#1125) is not in mixedCase
Parameter RubiconMarket.getBuyAmount(ERC20,ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#1126) is not in mixedCase
Parameter RubiconMarket.getBuyAmount(ERC20,ERC20,uint256).pay_amt (contracts/RubiconMarket.sol#1127) is not in mixedCase
Parameter RubiconMarket.getPayAmountWithFee(ERC20,ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#1150) is not in mixedCase
Parameter RubiconMarket.getPayAmountWithFee(ERC20,ERC20,uint256).buy_gem (contracts/RubiconMarket.sol#1151) is not in mixedCase
Parameter RubiconMarket.getPayAmountWithFee(ERC20,ERC20,uint256).buy_amt (contracts/RubiconMarket.sol#1152) is not in mixedCase
Parameter RubiconMarket.getPayAmount(ERC20,ERC20,uint256).pay_gem (contracts/RubiconMarket.sol#1158) is not in mixedCase
Parameter RubiconMarket.getPayAmount(ERC20,ERC20,uint256).buy_gem (contracts/RubiconMarket.sol#1159) is not in mixedCase
Parameter RubiconMarket.getPayAmount(ERC20,ERC20,uint256).buy_amt (contracts/RubiconMarket.sol#1160) is not in mixedCase
Parameter RubiconMarket.setFeeBPS(uint256)._newFeeBPS (contracts/RubiconMarket.sol#1466) is not in mixedCase
Parameter RubiconMarket.setMakerFee(uint256)._newMakerFee (contracts/RubiconMarket.sol#1471) is not in mixedCase
```

Events should be named using the CapWords style. Examples: `Deposit, Transfer, Approval, BeforeTransfer, AfterTransfer`.

```
Event EventfulMarketemitOffer(bytes32,bytes32,address,ERC20,ERC20,uint128,uint128) (contracts/RubiconMarket.sol#118-126) is not in CapWords
Event EventfulMarketemitTake(bytes32,bytes32,address,address,ERC20,ERC20,uint128,uint128) (contracts/RubiconMarket.sol#152-161) is not in CapWords
Event EventfulMarketemitCancel(bytes32,bytes32,address,ERC20,ERC20,uint128,uint128) (contracts/RubiconMarket.sol#174-182) is not in CapWords
Event EventfulMarketemitFee(bytes32,address,address,bytes32,ERC20,uint256) (contracts/RubiconMarket.sol#198-205) is not in CapWords
Event EventfulMarketemitDelete(bytes32,bytes32,address) (contracts/RubiconMarket.sol#209-213) is not in CapWords
```

Local and State Variable Names
Use mixedCase. Examples: `totalSupply, remainingSupply, balancesOf, creatorAddress, isPreSale, tokenExchangeRate`.

```
Variable SimpleMarket.last_offer_id (contracts/RubiconMarket.sol#219) is not in mixedCase
Variable RubiconMarket.AqueductDistributionLive (contracts/RubiconMarket.sol#682) is not in mixedCase
Variable RubiconMarket.AqueductAddress (contracts/RubiconMarket.sol#684) is not in mixedCase
Variable RubiconMarket.\_rank (contracts/RubiconMarket.sol#691) is not in mixedCase
Variable RubiconMarket.\_best (contracts/RubiconMarket.sol#692) is not in mixedCase
Variable RubiconMarket.\_span (contracts/RubiconMarket.sol#693) is not in mixedCase
Variable RubiconMarket.\_dust (contracts/RubiconMarket.sol#694) is not in mixedCase
Variable RubiconMarket.\_near (contracts/RubiconMarket.sol#695) is not in mixedCase
Variable RubiconMarket.\_head (contracts/RubiconMarket.sol#696) is not in mixedCase
```

Modifier Names
Use mixedCase. Examples: `onlyBy, onlyAfter, onlyDuringThePreSale`.

```
Modifier SimpleMarket.can_buy(uint256) (contracts/RubiconMarket.sol#245-248) is not in mixedCase
Modifier SimpleMarket.can_cancel(uint256) (contracts/RubiconMarket.sol#251-258) is not in mixedCase
Modifier SimpleMarket.can_offer() (contracts/RubiconMarket.sol#260-262) is not in mixedCase
Modifier ExpiringMarket.can_offer() (contracts/RubiconMarket.sol#597-600) is not in mixedCase
Modifier ExpiringMarket.can_buy(uint256) (contracts/RubiconMarket.sol#603-607) is not in mixedCase
Modifier ExpiringMarket.can_cancel(uint256) (contracts/RubiconMarket.sol#610-618) is not in mixedCase
Modifier RubiconMarket.can_cancel(uint256) (contracts/RubiconMarket.sol#719-729) is not in mixedCase
```

Function arguments should use mixedCase. Examples: `initialSupply, account, recipientAddress, senderAddress, newOwner`.

```
Function RubiconMarket.del_rank(uint256) (contracts/RubiconMarket.sol#937-948) is not in mixedCase
Function SimpleMarket.\_next_id() (contracts/RubiconMarket.sol#568-571) is not in mixedCase
```

Structs should be named using the CapWords style. Examples: `MyCoin, Position, PositionXY`.

```
Struct RubiconMarket.sortInfo (contracts/RubiconMarket.sol#686-690) is not in CapWords
```
