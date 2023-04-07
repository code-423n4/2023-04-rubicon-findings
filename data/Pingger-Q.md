QA Report URLS in scope
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol
##
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
##L-02
missing-zero-address-validation: 

BathHouseV2.initialize(address,address)._pAdmin (contracts/BathHouseV2.sol#32) lacks a zero-check on : 

- proxyAdmin = _pAdmin (contracts/BathHouseV2.sol#36) 

DSAuth.setOwner(address).owner_ (contracts/RubiconMarket.sol#25) lacks a zero-check on : 

- owner = owner_ (contracts/RubiconMarket.sol#26) 

BathBuddy.spawnBuddy(address,address,address)._owner (contracts/periphery/BathBuddy.sol#72) lacks a zero-check on : 

- owner = _owner (contracts/periphery/BathBuddy.sol#77) 

BathBuddy.spawnBuddy(address,address,address).newBud (contracts/periphery/BathBuddy.sol#73) lacks a zero-check on : 

- myBathTokenBuddy = newBud (contracts/periphery/BathBuddy.sol#78) 

BathBuddy.spawnBuddy(address,address,address)._bathHouse (contracts/periphery/BathBuddy.sol#74) lacks a zero-check on : 

- bathHouse = _bathHouse (contracts/periphery/BathBuddy.sol#79) 

Tools Used
Manual review

##

##L-03
calls-inside-a-loop: 

BathHouseV2.claimRewards(address[],address[]) (contracts/BathHouseV2.sol#115-128) has external calls inside a loop: IBathBuddy(buddies[i]).getReward(IERC20(rewardsTokens[i]),msg.sender) (contracts/BathHouseV2.sol#123-126) 

RubiconMarket.batchOffer(uint256[],address[],uint256[],address[]) (contracts/RubiconMarket.sol#887-907) has external calls inside a loop: this.offer(payAmts[i],ERC20(payGems[i]),buyAmts[i],ERC20(buyGems[i])) (contracts/RubiconMarket.sol#900-905) 

SimpleMarket.cancel(uint256) (contracts/RubiconMarket.sol#452-485) has external calls inside a loop: require(bool)(_offer.pay_gem.transfer(_offer.recipient,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

SimpleMarket.cancel(uint256) (contracts/RubiconMarket.sol#452-485) has external calls inside a loop: require(bool)(_offer.pay_gem.transfer(_offer.owner,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

RubiconMarket.batchRequote(uint256[],uint256[],address[],uint256[],address[]) (contracts/RubiconMarket.sol#917-933) has external calls inside a loop: this.offer(payAmts[i],ERC20(payGems[i]),buyAmts[i],ERC20(buyGems[i])) (contracts/RubiconMarket.sol#926-931) 

Tools Used
Manual review

##
##L-04
Reentrancy: 

Reentrancy in RubiconMarket._buys(uint256,uint256) (contracts/RubiconMarket.sol#1183-1205): 

External calls: 

- require(bool)(super.buy(id,amount)) (contracts/RubiconMarket.sol#1194) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,feeTo,fee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#339-342) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#350-357) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.owner,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#359-362) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,spend),_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee) (contracts/RubiconMarket.sol#374-377) 

- require(bool,string)(_offer.pay_gem.transfer(msg.sender,quantity),_offer.pay_gem.transfer(msg.sender, quantity) failed) (contracts/RubiconMarket.sol#379-382) 

State variables written after the call(s): 

- dustId = id (contracts/RubiconMarket.sol#1201) 

Reentrancy in RubiconMarket._matcho(uint256,ERC20,uint256,ERC20,uint256,bool,address,address) (contracts/RubiconMarket.sol#1273-1341): 

External calls: 

- id = super.offer(t_pay_amt,t_pay_gem,t_buy_amt,t_buy_gem,owner,recipient) (contracts/RubiconMarket.sol#1330-1337) 

- require(bool)(pay_gem.transferFrom(msg.sender,address(this),pay_amt)) (contracts/RubiconMarket.sol#538) 

State variables written after the call(s): 

- _sort(id,pos) (contracts/RubiconMarket.sol#1339) 

- _rank[pos].prev = id (contracts/RubiconMarket.sol#1384) 

- _rank[id].next = pos (contracts/RubiconMarket.sol#1385) 

- _rank[prev_id].next = id (contracts/RubiconMarket.sol#1396) 

- _rank[id].prev = prev_id (contracts/RubiconMarket.sol#1397) 

- _sort(id,pos) (contracts/RubiconMarket.sol#1339) 

- _span[address(pay_gem)][address(buy_gem)] ++ (contracts/RubiconMarket.sol#1400) 

Reentrancy in BathHouseV2.createBathToken(address,InterestRateModel,uint256,address,bytes) (contracts/BathHouseV2.sol#60-111): 

External calls: 

- bathToken = address(new CErc20Delegator(underlying,comptroller,interestRateModel,initialExchangeRateMantissa,name,symbol,decimals,address(proxyAdmin),implementation,becomeImplementationData)) (contracts/BathHouseV2.sol#87-100) 

- buddy.spawnBuddy(admin,bathToken,address(this)) (contracts/BathHouseV2.sol#104) 

State variables written after the call(s): 

- bathTokenToBuddy[bathToken] = address(buddy) (contracts/BathHouseV2.sol#107) 

Reentrancy in RubiconMarket._buys(uint256,uint256) (contracts/RubiconMarket.sol#1183-1205): 

External calls: 

- require(bool)(super.buy(id,amount)) (contracts/RubiconMarket.sol#1194) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,feeTo,fee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#339-342) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#350-357) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.owner,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#359-362) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,spend),_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee) (contracts/RubiconMarket.sol#374-377) 

- require(bool,string)(_offer.pay_gem.transfer(msg.sender,quantity),_offer.pay_gem.transfer(msg.sender, quantity) failed) (contracts/RubiconMarket.sol#379-382) 

- cancel(id) (contracts/RubiconMarket.sol#1202) 

- require(bool)(_offer.pay_gem.transfer(_offer.recipient,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

- require(bool)(_offer.pay_gem.transfer(_offer.owner,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

Event emitted after the call(s): 

- LogItemUpdate(id) (contracts/RubiconMarket.sol#463) 

- cancel(id) (contracts/RubiconMarket.sol#1202) 

- emitCancel(bytes32(id),keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),_offer.owner,_offer.pay_gem,_offer.buy_gem,uint128(_offer.pay_amt),uint128(_offer.buy_amt)) (contracts/RubiconMarket.sol#474-482) 

- cancel(id) (contracts/RubiconMarket.sol#1202) 

Reentrancy in RubiconMarket._matcho(uint256,ERC20,uint256,ERC20,uint256,bool,address,address) (contracts/RubiconMarket.sol#1273-1341): 

External calls: 

- id = super.offer(t_pay_amt,t_pay_gem,t_buy_amt,t_buy_gem,owner,recipient) (contracts/RubiconMarket.sol#1330-1337) 

- require(bool)(pay_gem.transferFrom(msg.sender,address(this),pay_amt)) (contracts/RubiconMarket.sol#538) 

Event emitted after the call(s): 

- LogSortedOffer(id) (contracts/RubiconMarket.sol#1401) 

- _sort(id,pos) (contracts/RubiconMarket.sol#1339) 

Reentrancy in RubiconMarket.batchRequote(uint256[],uint256[],address[],uint256[],address[]) (contracts/RubiconMarket.sol#917-933): 

External calls: 

- cancel(ids[i]) (contracts/RubiconMarket.sol#925) 

- require(bool)(_offer.pay_gem.transfer(_offer.recipient,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

- require(bool)(_offer.pay_gem.transfer(_offer.owner,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

- this.offer(payAmts[i],ERC20(payGems[i]),buyAmts[i],ERC20(buyGems[i])) (contracts/RubiconMarket.sol#926-931) 

Event emitted after the call(s): 

- LogItemUpdate(id) (contracts/RubiconMarket.sol#463) 

- cancel(ids[i]) (contracts/RubiconMarket.sol#925) 

- emitCancel(bytes32(id),keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),_offer.owner,_offer.pay_gem,_offer.buy_gem,uint128(_offer.pay_amt),uint128(_offer.buy_amt)) (contracts/RubiconMarket.sol#474-482) 

- cancel(ids[i]) (contracts/RubiconMarket.sol#925) 

Reentrancy in SimpleMarket.buy(uint256,uint256) (contracts/RubiconMarket.sol#314-448): 

External calls: 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,feeTo,fee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#339-342) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#350-357) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.owner,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#359-362) 

Event emitted after the call(s): 

- emitFee(bytes32(id),msg.sender,_offer.owner,keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),_offer.buy_gem,mFee) (contracts/RubiconMarket.sol#365-372) 

Reentrancy in SimpleMarket.buy(uint256,uint256) (contracts/RubiconMarket.sol#314-448): 

External calls: 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,feeTo,fee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#339-342) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#350-357) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.owner,mFee),Insufficient funds to cover fee) (contracts/RubiconMarket.sol#359-362) 

- require(bool,string)(_offer.buy_gem.transferFrom(msg.sender,_offer.recipient,spend),_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee) (contracts/RubiconMarket.sol#374-377) 

- require(bool,string)(_offer.pay_gem.transfer(msg.sender,quantity),_offer.pay_gem.transfer(msg.sender, quantity) failed) (contracts/RubiconMarket.sol#379-382) 

Event emitted after the call(s): 

- LogItemUpdate(id) (contracts/RubiconMarket.sol#384) 

- emitDelete(bytes32(id),keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),_offer.owner) (contracts/RubiconMarket.sol#440-444) 

- emitFee(bytes32(id),msg.sender,feeTo,keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),_offer.buy_gem,fee) (contracts/RubiconMarket.sol#419-426) 

- emitTake(bytes32(id),keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),msg.sender,_offer.owner,_offer.pay_gem,_offer.buy_gem,uint128(quantity),uint128(spend)) (contracts/RubiconMarket.sol#398-407) 

Reentrancy in SimpleMarket.cancel(uint256) (contracts/RubiconMarket.sol#452-485): 

External calls: 

- require(bool)(_offer.pay_gem.transfer(_offer.recipient,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

- require(bool)(_offer.pay_gem.transfer(_offer.owner,_offer.pay_amt)) (contracts/RubiconMarket.sol#459-461) 

Event emitted after the call(s): 

- LogItemUpdate(id) (contracts/RubiconMarket.sol#463) 

- emitCancel(bytes32(id),keccak256(bytes)(abi.encodePacked(_offer.pay_gem,_offer.buy_gem)),_offer.owner,_offer.pay_gem,_offer.buy_gem,uint128(_offer.pay_amt),uint128(_offer.buy_amt)) (contracts/RubiconMarket.sol#474-482) 

Reentrancy in Position.closePosition(uint256) (contracts/utilities/poolsUtility/Position.sol#210-223): 

External calls: 

- _repay(asset,quote,posId) (contracts/utilities/poolsUtility/Position.sol#219) 

- balance = CTokenInterface(bathToken).borrowBalanceCurrent(address(this)) (contracts/utilities/poolsUtility/Position.sol#66-68) 

- IERC20(_quote).approve(address(rubiconMarket),_maxFill) (contracts/utilities/poolsUtility/Position.sol#465) 

- rubiconMarket.buyAllAmount(ERC20(_asset),_buyAmount,ERC20(_quote),_maxFill) (contracts/utilities/poolsUtility/Position.sol#467-472) 

- IERC20(_asset).transferFrom(msg.sender,address(this),_payAmount.sub(_assetBalance).add(_fee)) (contracts/utilities/poolsUtility/Position.sol#493-497) 

- IERC20(_asset).approve(address(rubiconMarket),IERC20(_asset).balanceOf(address(this))) (contracts/utilities/poolsUtility/Position.sol#500-503) 

- IERC20(_quote).approve(_bathTokenQuote,_amountToRepay) (contracts/utilities/poolsUtility/Position.sol#297) 

- rubiconMarket.sellAllAmount(ERC20(_asset),_payAmount,ERC20(_quote),_minFill) (contracts/utilities/poolsUtility/Position.sol#505-510) 

- require(bool,string)(CErc20Interface(_bathTokenQuote).repayBorrow(_amountToRepay) == 0,_repay: ERROR) (contracts/utilities/poolsUtility/Position.sol#298-301) 

- _redeem(asset,bathTokenAmount) (contracts/utilities/poolsUtility/Position.sol#220) 

- require(bool,string)(comptroller.exitMarket(_bathToken) == 0,_exitMarket: ERROR) (contracts/utilities/poolsUtility/Position.sol#344) 

- balance = CTokenInterface(bathToken).borrowBalanceCurrent(address(this)) (contracts/utilities/poolsUtility/Position.sol#66-68) 

- require(bool,string)(CErc20Interface(_bathTokenAsset).redeem(_bathTokenAmount) == 0,_redeem: REDEEM FAILED) (contracts/utilities/poolsUtility/Position.sol#385-388) 

Event emitted after the call(s): 

- PositionClosed(_positionId) (contracts/utilities/poolsUtility/Position.sol#423) 

- _removePosition(posId) (contracts/utilities/poolsUtility/Position.sol#222) 

Reentrancy in BathHouseV2.createBathToken(address,InterestRateModel,uint256,address,bytes) (contracts/BathHouseV2.sol#60-111): 

External calls: 

- bathToken = address(new CErc20Delegator(underlying,comptroller,interestRateModel,initialExchangeRateMantissa,name,symbol,decimals,address(proxyAdmin),implementation,becomeImplementationData)) (contracts/BathHouseV2.sol#87-100) 

- buddy.spawnBuddy(admin,bathToken,address(this)) (contracts/BathHouseV2.sol#104) 

Event emitted after the call(s): 

- BathTokenCreated(bathToken,underlying) (contracts/BathHouseV2.sol#109) 

- BuddySpawned(bathToken,address(buddy)) (contracts/BathHouseV2.sol#110) 

Reentrancy in V2Migrator.migrate(IBathToken) (contracts/V2Migrator.sol#38-74): 

External calls: 

- bathTokenV1.transferFrom(msg.sender,address(this),bathBalance) (contracts/V2Migrator.sol#44) 

- amountWithdrawn = bathTokenV1.withdraw(bathBalance) (contracts/V2Migrator.sol#47) 

- underlying.approve(bathTokenV2,amountWithdrawn) (contracts/V2Migrator.sol#53) 

- require(bool,string)(CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,migrate: MINT FAILED) (contracts/V2Migrator.sol#54-57) 

- IERC20(bathTokenV2).transfer(msg.sender,IERC20(bathTokenV2).balanceOf(address(this))) (contracts/V2Migrator.sol#59-62) 

Event emitted after the call(s): 

- Migrated(msg.sender,address(bathTokenV1),address(bathTokenV2),amountWithdrawn) (contracts/V2Migrator.sol#68-73) 

Reentrancy in SimpleMarket.offer(uint256,ERC20,uint256,ERC20,address,address) (contracts/RubiconMarket.sol#511-562): 

External calls: 

- require(bool)(pay_gem.transferFrom(msg.sender,address(this),pay_amt)) (contracts/RubiconMarket.sol#538) 

Event emitted after the call(s): 

- LogItemUpdate(id) (contracts/RubiconMarket.sol#540) 

- emitOffer(bytes32(id),keccak256(bytes)(abi.encodePacked(pay_gem,buy_gem)),msg.sender,pay_gem,buy_gem,uint128(pay_amt),uint128(buy_amt)) (contracts/RubiconMarket.sol#553-561) 

Tools Used
Manual review

##
##L-05
block-timestamp and Dangerous comparisons

SimpleMarket.isActive(uint256) (contracts/RubiconMarket.sol#276-278) uses timestamp for comparisons 

Dangerous comparisons: 

- offers[id].timestamp > 0 (contracts/RubiconMarket.sol#277) 

SimpleMarket.buy(uint256,uint256) (contracts/RubiconMarket.sol#314-448) uses timestamp for comparisons 

Dangerous comparisons: 

- _offer.owner == address(0) && getRecipient(id) != address(0) (contracts/RubiconMarket.sol#349) 

- offers[id].pay_amt == 0 (contracts/RubiconMarket.sol#436) 

RubiconMarket.del_rank(uint256) (contracts/RubiconMarket.sol#937-948) uses timestamp for comparisons 

Dangerous comparisons: 

- require(bool)(! isActive(id) && _rank[id].delb != 0 && _rank[id].delb < block.number - 10) (contracts/RubiconMarket.sol#940-944) 

RubiconMarket._buys(uint256,uint256) (contracts/RubiconMarket.sol#1183-1205) uses timestamp for comparisons 

Dangerous comparisons: 

- amount == offers[id].pay_amt (contracts/RubiconMarket.sol#1185) 

- isActive(id) && offers[id].pay_amt < _dust[address(offers[id].pay_gem)] (contracts/RubiconMarket.sol#1198-1199) 

RubiconMarket._findpos(uint256,uint256) (contracts/RubiconMarket.sol#1225-1258) uses timestamp for comparisons 

Dangerous comparisons: 

- pos != 0 && ! isActive(pos) (contracts/RubiconMarket.sol#1229) 

RubiconMarket._matcho(uint256,ERC20,uint256,ERC20,uint256,bool,address,address) (contracts/RubiconMarket.sol#1273-1341) uses timestamp for comparisons 

Dangerous comparisons: 

- mul(m_buy_amt,t_buy_amt) > mul(t_pay_amt,m_pay_amt) + m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt (contracts/RubiconMarket.sol#1301-1307) 

RubiconMarket._sort(uint256,uint256) (contracts/RubiconMarket.sol#1362-1402) uses timestamp for comparisons 

Dangerous comparisons: 

- pos == 0 || offers[pos].pay_gem != pay_gem || offers[pos].buy_gem != buy_gem || ! isOfferSorted(pos) (contracts/RubiconMarket.sol#1372-1377) 

RubiconMarket._unsort(uint256) (contracts/RubiconMarket.sol#1405-1435) uses timestamp for comparisons 

Dangerous comparisons: 

- require(bool)(_span[pay_gem][buy_gem] > 0) (contracts/RubiconMarket.sol#1410) 

BathBuddy.lastTimeRewardApplicable(address) (contracts/periphery/BathBuddy.sol#112-119) uses timestamp for comparisons 

Dangerous comparisons: 

- block.timestamp < periodFinish[token] (contracts/periphery/BathBuddy.sol#115-118) 

BathBuddy.notifyRewardAmount(uint256,IERC20) (contracts/periphery/BathBuddy.sol#191-228) uses timestamp for comparisons 

Dangerous comparisons: 

- block.timestamp >= periodFinish[address(rewardsToken)] (contracts/periphery/BathBuddy.sol#195) 

- require(bool,string)(rewardRates[address(rewardsToken)] <= balance.div(rewardsDuration[address(rewardsToken)]),Provided reward too high) (contracts/periphery/BathBuddy.sol#217-221) 

BathBuddy.setRewardsDuration(uint256,address) (contracts/periphery/BathBuddy.sol#232-242) uses timestamp for comparisons 

Dangerous comparisons: 

- require(bool,string)(block.timestamp > periodFinish[token],Previous rewards period must be complete before changing the duration for the new period) (contracts/periphery/BathBuddy.sol#236-239) 

Tools Used
Manual review

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

