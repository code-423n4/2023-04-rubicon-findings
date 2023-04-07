QA Report URLS in scope
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

##L-01
Title
Local variable shadowing

Description
Detection of shadowing using local variables.
The use of owner in a setOwner and isAuthorized function might be incorrect.

POC

Line 23-42
```solidity
    address public owner;

    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }

    modifier auth() {
        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
        _;
    }

    function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }
}
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L23-L42
```solidity
RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,address,address).owner (contracts/RubiconMarket.sol#808) shadows: 

- DSAuth.owner (contracts/RubiconMarket.sol#23) (state variable) 

RubiconMarket.offer(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).owner (contracts/RubiconMarket.sol#831) shadows: 

- DSAuth.owner (contracts/RubiconMarket.sol#23) (state variable) 

RubiconMarket._matcho(uint256,ERC20,uint256,ERC20,uint256,bool,address,address).owner (contracts/RubiconMarket.sol#1280) shadows: 

- DSAuth.owner (contracts/RubiconMarket.sol#23) (state variable) 
```

Tools Used
Manual review

Recommendation
Rename the local variables that shadow another component.

##
##N-01
assembly-usage: 

Position._supply(address,address,uint256) (contracts/utilities/poolsUtility/Position.sol#350-379) uses assembly 

- INLINE ASM (contracts/utilities/poolsUtility/Position.sol#367-378) 

Tools Used
Manual review

##N-02
costly-operations-inside-a-loop: 

SimpleMarket.synchronized() (contracts/RubiconMarket.sol#264-269) has costly operations inside a loop: 

- locked = true (contracts/RubiconMarket.sol#266) 

SimpleMarket.synchronized() (contracts/RubiconMarket.sol#264-269) has costly operations inside a loop: 

- locked = false (contracts/RubiconMarket.sol#268) 

SimpleMarket.cancel(uint256) (contracts/RubiconMarket.sol#452-485) has costly operations inside a loop: 

- delete offers[id] (contracts/RubiconMarket.sol#456) 

RubiconMarket._hide(uint256) (contracts/RubiconMarket.sol#1438-1464) has costly operations inside a loop: 

- _head = _near[id] (contracts/RubiconMarket.sol#1448) 

Tools Used
Manual review

##N-03
dead-code: 

RubiconMarket._buys(uint256,uint256) (contracts/RubiconMarket.sol#1183-1205) is never used and should be removed 

Tools Used
Manual review

##N-04
incorrect-versions-of-solidity
solc-0.7.6 is not recommended for deployment 

Tools Used
Manual review

##N-05 
Incorrect-versions-of-solidity:
Pragma version0.8.17 (contracts/BathHouseV2.sol#2) allows old versions 

Pragma version^0.8.9 (contracts/RubiconMarket.sol#2) allows old versions 

Pragma version0.8.17 (contracts/V2Migrator.sol#2) allows old versions 

Pragma version>=0.7.6 (contracts/interfaces/IBathBuddy.sol#3) allows old versions 

Pragma version>=0.7.6 (contracts/interfaces/IBathToken.sol#3) allows old versions 

Pragma version>=0.8.16 (contracts/interfaces/IRubiconMarket.sol#3) allows old versions 

Pragma version>=0.8.16 (contracts/interfaces/ISwapRouter.sol#2) allows old versions 

Pragma version>=0.8.17 (contracts/interfaces/IWETH.sol#3) allows old versions 

Pragma version^0.8.0 (contracts/periphery/BathBuddy.sol#2) allows old versions 

Pragma version0.8.17 (contracts/utilities/FeeWrapper.sol#2) allows old versions 

Pragma version0.8.17 (contracts/utilities/poolsUtility/Position.sol#2) allows old versions 

solc-0.8.17 is not recommended for deployment 

Tools Used
Manual review

##N-06
low-level-calls:
Low level call in FeeWrapper._rubicall(FeeWrapper.CallParams) (contracts/utilities/FeeWrapper.sol#60-73): 

- (_OK,_data) = _params.target.call(bytes.concat(_params.selector,_params.args)) (contracts/utilities/FeeWrapper.sol#66-68) 

Low level call in FeeWrapper._rubicallPayable(FeeWrapper.CallParams) (contracts/utilities/FeeWrapper.sol#76-89): 

- (_OK,_data) = _params.target.call{value: _msgValue}(bytes.concat(_params.selector,_params.args)) (contracts/utilities/FeeWrapper.sol#82-84) 

Low level call in FeeWrapper._chargeFeePayable(FeeWrapper.FeeParams) (contracts/utilities/FeeWrapper.sol#108-121): 

- (OK) = address(_feeTo).call{value: _feeAmount}() (contracts/utilities/FeeWrapper.sol#118) 

##N-07
conformance-to-solidity-naming-conventions:

Parameter BathHouseV2.initialize(address,address)._comptroller (contracts/BathHouseV2.sol#32) is not in mixedCase 

Parameter BathHouseV2.initialize(address,address)._pAdmin (contracts/BathHouseV2.sol#32) is not in mixedCase 

Event EventfulMarketemitOffer(bytes32,bytes32,address,ERC20,ERC20,uint128,uint128) (contracts/RubiconMarket.sol#118-126) is not in CapWords 

Event EventfulMarketemitTake(bytes32,bytes32,address,address,ERC20,ERC20,uint128,uint128) (contracts/RubiconMarket.sol#152-161) is not in CapWords 

Event EventfulMarketemitCancel(bytes32,bytes32,address,ERC20,ERC20,uint128,uint128) (contracts/RubiconMarket.sol#174-182) is not in CapWords 

Event EventfulMarketemitFee(bytes32,address,address,bytes32,ERC20,uint256) (contracts/RubiconMarket.sol#198-205) is not in CapWords 

Event EventfulMarketemitDelete(bytes32,bytes32,address) (contracts/RubiconMarket.sol#209-213) is not in CapWords 

Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).pay_amt (contracts/RubiconMarket.sol#512) is not in mixedCase 

Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).pay_gem (contracts/RubiconMarket.sol#513) is not in mixedCase 

Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).buy_amt (contracts/RubiconMarket.sol#514) is not in mixedCase 

Parameter SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address).buy_gem (contracts/RubiconMarket.sol#515) is not in mixedCase 

Struct RubiconMarket.sortInfo (contracts/RubiconMarket.sol#686-690) is not in CapWords 

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

Function RubiconMarket.del_rank(uint256) (contracts/RubiconMarket.sol#937-948) is not in mixedCase 

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

Variable RubiconMarket.AqueductDistributionLive (contracts/RubiconMarket.sol#682) is not in mixedCase 

Variable RubiconMarket.AqueductAddress (contracts/RubiconMarket.sol#684) is not in mixedCase 

Variable RubiconMarket._rank (contracts/RubiconMarket.sol#691) is not in mixedCase 

Variable RubiconMarket._best (contracts/RubiconMarket.sol#692) is not in mixedCase 

Variable RubiconMarket._span (contracts/RubiconMarket.sol#693) is not in mixedCase 

Variable RubiconMarket._dust (contracts/RubiconMarket.sol#694) is not in mixedCase 

Variable RubiconMarket._near (contracts/RubiconMarket.sol#695) is not in mixedCase 

Variable RubiconMarket._head (contracts/RubiconMarket.sol#696) is not in mixedCase 

Modifier RubiconMarket.can_cancel(uint256) (contracts/RubiconMarket.sol#719-729) is not in mixedCase 

Parameter IBathToken.placeOffer(uint256,IERC20,uint256,IERC20).pay_amt (contracts/interfaces/IBathToken.sol#13) is not in mixedCase 

Parameter IBathToken.placeOffer(uint256,IERC20,uint256,IERC20).pay_gem (contracts/interfaces/IBathToken.sol#14) is not in mixedCase 

Parameter IBathToken.placeOffer(uint256,IERC20,uint256,IERC20).buy_amt (contracts/interfaces/IBathToken.sol#15) is not in mixedCase 

Parameter IBathToken.placeOffer(uint256,IERC20,uint256,IERC20).buy_gem (contracts/interfaces/IBathToken.sol#16) is not in mixedCase 

Function IBathToken.RubiconMarketAddress() (contracts/interfaces/IBathToken.sol#49) is not in mixedCase 

Function IBathToken.DOMAIN_SEPARATOR() (contracts/interfaces/IBathToken.sol#63) is not in mixedCase 

Function IBathToken.PERMIT_TYPEHASH() (contracts/interfaces/IBathToken.sol#65) is not in mixedCase 

Parameter IRubiconMarket.offer(uint256,IERC20,uint256,IERC20,uint256,bool).pay_amt (contracts/interfaces/IRubiconMarket.sol#11) is not in mixedCase 

Parameter IRubiconMarket.offer(uint256,IERC20,uint256,IERC20,uint256,bool).pay_gem (contracts/interfaces/IRubiconMarket.sol#12) is not in mixedCase 

Parameter IRubiconMarket.offer(uint256,IERC20,uint256,IERC20,uint256,bool).buy_amt (contracts/interfaces/IRubiconMarket.sol#13) is not in mixedCase 

Parameter IRubiconMarket.offer(uint256,IERC20,uint256,IERC20,uint256,bool).buy_gem (contracts/interfaces/IRubiconMarket.sol#14) is not in mixedCase 

Parameter IRubiconMarket.getBestOffer(IERC20,IERC20).sell_gem (contracts/interfaces/IRubiconMarket.sol#21) is not in mixedCase 

Parameter IRubiconMarket.getBestOffer(IERC20,IERC20).buy_gem (contracts/interfaces/IRubiconMarket.sol#22) is not in mixedCase 

Parameter IRubiconMarket.sellAllAmount(IERC20,uint256,IERC20,uint256).pay_gem (contracts/interfaces/IRubiconMarket.sol#33) is not in mixedCase 

Parameter IRubiconMarket.sellAllAmount(IERC20,uint256,IERC20,uint256).pay_amt (contracts/interfaces/IRubiconMarket.sol#34) is not in mixedCase 

Parameter IRubiconMarket.sellAllAmount(IERC20,uint256,IERC20,uint256).buy_gem (contracts/interfaces/IRubiconMarket.sol#35) is not in mixedCase 

Parameter IRubiconMarket.sellAllAmount(IERC20,uint256,IERC20,uint256).min_fill_amount (contracts/interfaces/IRubiconMarket.sol#36) is not in mixedCase 

Parameter IRubiconMarket.buyAllAmount(IERC20,uint256,IERC20,uint256).buy_gem (contracts/interfaces/IRubiconMarket.sol#40) is not in mixedCase 

Parameter IRubiconMarket.buyAllAmount(IERC20,uint256,IERC20,uint256).buy_amt (contracts/interfaces/IRubiconMarket.sol#41) is not in mixedCase 

Parameter IRubiconMarket.buyAllAmount(IERC20,uint256,IERC20,uint256).pay_gem (contracts/interfaces/IRubiconMarket.sol#42) is not in mixedCase 

Parameter IRubiconMarket.buyAllAmount(IERC20,uint256,IERC20,uint256).max_fill_amount (contracts/interfaces/IRubiconMarket.sol#43) is not in mixedCase 

Parameter BathBuddy.spawnBuddy(address,address,address)._owner (contracts/periphery/BathBuddy.sol#72) is not in mixedCase 

Parameter BathBuddy.spawnBuddy(address,address,address)._bathHouse (contracts/periphery/BathBuddy.sol#74) is not in mixedCase 

Parameter BathBuddy.setRewardsDuration(uint256,address)._rewardsDuration (contracts/periphery/BathBuddy.sol#233) is not in mixedCase 

Tools Used
Manual review

##N-08
Variable-names-too-similar: 

Variable V2Migrator.migrate(IBathToken).bathTokenV1 (contracts/V2Migrator.sol#38) is too similar to V2Migrator.migrate(IBathToken).bathTokenV2 (contracts/V2Migrator.sol#51) 

Variable V2Migrator.constructor(address[],address[]).bathTokensV1 (contracts/V2Migrator.sol#30) is too similar to V2Migrator.constructor(address[],address[]).bathTokensV2 (contracts/V2Migrator.sol#30) 

Tools Used
Manual review

