## Summary<a name="Summary">

### Gas Optimizations
| |Issue|Contexts|Estimated Gas Saved|
|-|:-|:-|:-:|
| [GAS&#x2011;1](#GAS&#x2011;1) | Setting the `constructor` to `payable` | 2 | 26 |
| [GAS&#x2011;2](#GAS&#x2011;2) | Do not calculate constants | 2 | - |
| [GAS&#x2011;3](#GAS&#x2011;3) | Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function | 26 | 728 |
| [GAS&#x2011;4](#GAS&#x2011;4) | Using `delete` statement can save gas | 2 | - |
| [GAS&#x2011;5](#GAS&#x2011;5) | Use `assembly` to write address storage values | 4 | - |
| [GAS&#x2011;6](#GAS&#x2011;6) | Functions guaranteed to revert when called by normal users can be marked `payable` | 11 | 231 |
| [GAS&#x2011;7](#GAS&#x2011;7) | Use hardcoded address instead `address(this)` | 21 | - |
| [GAS&#x2011;8](#GAS&#x2011;8) | `internal` functions only called once can be inlined to save gas | 31 | 682 |
| [GAS&#x2011;9](#GAS&#x2011;9) | Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate | 7 | - |
| [GAS&#x2011;10](#GAS&#x2011;10) | Optimize names to save gas | 6 | 132 |
| [GAS&#x2011;11](#GAS&#x2011;11) | `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables | 5 | - |
| [GAS&#x2011;12](#GAS&#x2011;12) | Public Functions To External | 33 | - |
| [GAS&#x2011;13](#GAS&#x2011;13) | Save gas with the use of specific import statements | 11 | - |
| [GAS&#x2011;14](#GAS&#x2011;14) | Shorten the array rather than copying to a new one | 1 | - |
| [GAS&#x2011;15](#GAS&#x2011;15) | Structs can be packed into fewer storage slots | 2 | 4000 |
| [GAS&#x2011;16](#GAS&#x2011;16) | Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead | 1 | - |
| [GAS&#x2011;17](#GAS&#x2011;17) | Using `unchecked` blocks to save gas | 2 | 40 |
| [GAS&#x2011;18](#GAS&#x2011;18) | Use assembly to check for `address(0)` | 3 | - |
| [GAS&#x2011;19](#GAS&#x2011;19) | Use solidity version 0.8.19 to gain some gas boost | 6 | 528 |

Total: 176 contexts over 19 issues

## Gas Optimizations



### <a href="#Summary">[GAS&#x2011;1]</a><a name="GAS&#x2011;1"> Setting the `constructor` to `payable`

Saves ~13 gas per instance

#### <ins>Proof Of Concept</ins>

```solidity
30: constructor(address[] memory bathTokensV1, address[] memory bathTokensV2)
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L30

```solidity
54: constructor(address _oracle, address _rubiconMarket, address _bathHouseV2)
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L54





### <a href="#Summary">[GAS&#x2011;2]</a><a name="GAS&#x2011;2"> Do not calculate constants

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

#### <ins>Proof Of Concept</ins>

```solidity
74: uint256 constant WAD = 10 ** 18;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L74

```solidity
75: uint256 constant RAY = 10 ** 27;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L75





#### <a href="#Summary">[GAS&#x2011;3]</a><a name="GAS&#x2011;3"> Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function

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








### <a href="#Summary">[GAS&#x2011;4]</a><a name="GAS&#x2011;4"> Using `delete` statement can save gas

#### <ins>Proof Of Concept</ins>

```solidity
1214: uint256 old_top = 0;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1214

```solidity
181: tokenRewards[address(rewardsToken)][holderRecipient] = 0;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L181




### <a href="#Summary">[GAS&#x2011;5]</a><a name="GAS&#x2011;5"> Use `assembly` to write address storage values

#### <ins>Proof Of Concept</ins>

```solidity
582: _amount = amount;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L582

```solidity
1357: _head = id;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1357

```solidity
294: _amountToRepay = _borrowBalance;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L294

```solidity
587: _wad = WAD;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L587





### <a href="#Summary">[GAS&#x2011;6]</a><a name="GAS&#x2011;6"> Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier or require such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

#### <ins>Proof Of Concept</ins>

```solidity
60: function createBathToken(
        address underlying,
        InterestRateModel interestRateModel,
        uint256 initialExchangeRateMantissa,
        address implementation,
        bytes memory becomeImplementationData
    ) external onlyAdmin {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L60

```solidity
13: function getReward(
        IERC20 rewardsToken,
        address holderRecipient
    )
        external
        override
        nonReentrant
        whenNotPaused
        updateReward(holderRecipient, address(rewardsToken))
        onlyBathHouse
    {
157: function getReward(
        IERC20 rewardsToken,
        address holderRecipient
    )
        external
        override
        nonReentrant
        whenNotPaused
        updateReward(holderRecipient, address(rewardsToken))
        onlyBathHouse
    {
168: function getReward(
        IERC20 rewardsToken,
        address holderRecipient
    )
        external
        override
        nonReentrant
        whenNotPaused
        updateReward(holderRecipient, address(rewardsToken))
        onlyBathHouse
    {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L13

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L157

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L168



```solidity
191: function notifyRewardAmount(
        uint256 reward,
        IERC20 rewardsToken
    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L191

```solidity
232: function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L232

```solidity
93: function buyAllAmountWithLeverage(
        address quote,
        address asset,
        uint256 quotePayAmount,
        uint256 leverage
    ) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L93

```solidity
107: function sellAllAmountWithLeverage(
        address asset,
        address quote,
        uint256 assetPayAmount,
        uint256 leverage
    ) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L107

```solidity
210: function closePosition(uint256 posId) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L210

```solidity
226: function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L226

```solidity
242: function withdraw(address token, uint256 amount) external onlyOwner {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L242



#### <ins>Recommended Mitigation Steps</ins>
Functions guaranteed to revert when called by normal users can be marked payable.



### <a href="#Summary">[GAS&#x2011;7]</a><a name="GAS&#x2011;7"> Use hardcode address instead `address(this)`

Instead of using `address(this)`, it is more gas-efficient to pre-calculate and use the hardcoded `address`. Foundry's script.sol and solmate's `LibRlp.sol` contracts can help achieve this.

References: 
https://book.getfoundry.sh/reference/forge-std/compute-create-address 

https://twitter.com/transmissions11/status/1518507047943245824

#### <ins>Proof Of Concept</ins>

```solidity
104: buddy.spawnBuddy(admin, bathToken, address(this));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L104

```solidity
538: require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L538

```solidity
44: bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);
61: IERC20(bathTokenV2).balanceOf(address(this))
64: IERC20(bathTokenV2).balanceOf(address(this)) == 0,

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L44

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L61

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L64



```solidity
216: uint256 balance = rewardsToken.balanceOf(address(this));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L216

```solidity
100: IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L100

```solidity
67: address(this)

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L67

```solidity
140: vars.initAssetBalance = IERC20(asset).balanceOf(address(this));
143: IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);
160: uint256 assetBalance = IERC20(asset).balanceOf(address(this));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L140

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L143

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L160



```solidity
233: IERC20(asset).safeTransferFrom(msg.sender, address(this), amount);

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L233

```solidity
286: uint256 _quoteBalance = IERC20(_quote).balanceOf(address(this));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L286

```solidity
310: .getAccountLiquidity(address(this));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L310

```solidity
356: address(this)
356: address(this)

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L356

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L356



```solidity
392: IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L392

```solidity
487: uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));
495: address(this),
487: IERC20(_asset).balanceOf(address(this))

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L487

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L495

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L487



```solidity
537: uint256 _minted = IERC20(_bathToken).balanceOf(address(this));

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L537



#### <ins>Recommended Mitigation Steps</ins>

Use hardcoded `address`





### <a href="#Summary">[GAS&#x2011;8]</a><a name="GAS&#x2011;8"> `internal` functions only called once can be inlined to save gas

#### <ins>Proof Of Concept</ins>

```solidity
35: function isAuthorized

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L35

```solidity
62: function max

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L62

```solidity
66: function imin

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L66

```solidity
70: function imax

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L70

```solidity
77: function wmul

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L77

```solidity
568: function _next_id

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L568

```solidity
1183: function _buys

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1183

```solidity
1225: function _findpos

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1225

```solidity
1273: function _matcho

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1273

```solidity
1362: function _sort

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1362

```solidity
60: function _rubicall
76: function _rubicall

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L60

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L76



```solidity
76: function _rubicallPayable

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L76

```solidity
93: function _chargeFee
108: function _chargeFee

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L93

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L108



```solidity
108: function _chargeFeePayable

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L108

```solidity
251: function _borrowLoop

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L251

```solidity
251: function _borrow
272: function _borrow
526: function _borrow

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L251

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L272

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L526



```solidity
280: function _repay

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L280

```solidity
322: function _calculateDebt

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L322

```solidity
336: function _enterMarkets

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L336

```solidity
343: function _exitMarket

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L343

```solidity
382: function _redeem

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L382

```solidity
401: function _savePosition

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L401

```solidity
421: function _removePosition

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L421

```solidity
426: function _updateMargin

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L426

```solidity
454: function _marketBuy

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L454

```solidity
475: function _marketSell

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L475

```solidity
526: function _borrowLimit

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L526






### <a href="#Summary">[GAS&#x2011;9]</a><a name="GAS&#x2011;9"> Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.

#### <ins>Proof Of Concept</ins>


```solidity
20: mapping(address => address) private tokenToBathToken;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L20

```solidity
21: mapping(address => address) private bathTokenToBuddy;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L21

These two can be combined to:
```solidity
struct bathTokenStruct {
    address tokenToBathToken;
    address bathTokenToBuddy;
}
```


```solidity
52: mapping(address => uint256) public periodFinish; // Token specific
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L52

```solidity
53: mapping(address => uint256) public rewardRates; // Token specific reward rates
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L53

```solidity
54: mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L54

```solidity
55: mapping(address => uint256) public lastUpdateTime; //Token specific
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L55

```solidity
56: mapping(address => uint256) public rewardsPerTokensStored; // Token specific
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L56

These  can be combined to:
```solidity
struct bathBuddyRewardsStruct {
    uint256 periodFinish;
    uint256 rewardRates;
    uint256 rewardsDuration;
    uint256 lastUpdateTime;
    uint256 rewardsPerTokensStored;
}
```






### <a href="#Summary">[GAS&#x2011;10]</a><a name="GAS&#x2011;10"> Optimize names to save gas

Contracts most called functions could simply save gas by function ordering via Method ID. Calling a function at runtime will be cheaper if the function is positioned earlier in the order (has a relatively lower Method ID) because 22 gas are added to the cost of a function for every position that came before it. The caller can save on gas if you prioritize most called functions. 

See more <a href="https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92">here</a>

#### <ins>Proof Of Concept</ins>

All in-scope contracts

#### <ins>Recommended Mitigation Steps</ins>
Find a lower method ID name for the most called functions for example Call() vs. Call1() is cheaper by 22 gas
For example, the function IDs in the Gauge.sol contract will be the most used; A lower method ID may be given.



### <a href="#Summary">[GAS&#x2011;11]</a><a name="GAS&#x2011;11"> `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

#### <ins>Proof Of Concept</ins>


```solidity
583: _amount -= mul(amount, feeBPS) / 100_000;
586: _amount -= mul(amount, makerFee()) / 100_000;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L583

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L586



```solidity
184: vars.currentBathTokenAmount += _borrowLoop(

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L184

```solidity
431: positions[_positionId].bathTokenAmount += _bathTokenAmount;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L431

```solidity
560: _assetAmount += _loopBorrowed;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L560





### <a href="#Summary">[GAS&#x2011;12]</a><a name="GAS&#x2011;12"> Public Functions To External

The following functions could be set external to save gas and improve code quality.
External call cost is less expensive than of public functions.

#### <ins>Proof Of Concept</ins>


```solidity
function getBathTokenFromAsset(
        address asset
    ) public view returns (address) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L45

```solidity
function makerFee() public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L272

```solidity
function isActive(uint256 id) public view returns (bool active) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L276

```solidity
function getOwner(uint256 id) public view returns (address owner) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L280

```solidity
function getRecipient(uint256 id) public view returns (address owner) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L284

```solidity
function getOffer(
        uint256 id
    ) public view returns (uint256, ERC20, uint256, ERC20) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L288

```solidity
function buy(
        uint256 id,
        uint256 quantity
    ) public virtual can_buy(id) synchronized returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L314

```solidity
function cancel(
        uint256 id
    ) public virtual can_cancel(id) synchronized returns (bool success) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L452

```solidity
function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L511

```solidity
function getFeeBPS() public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L574

```solidity
function calcAmountAfterFee(
        uint256 amount
    ) public view returns (uint256 _amount) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L578

```solidity
function isClosed() public pure returns (bool closed) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L620

```solidity
function getTime() public view returns (uint64) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L624

```solidity
function initialize(address _feeTo) public {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L700

```solidity
function make(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    ) public override returns (bytes32) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L491

```solidity
function take(bytes32 id, uint128 maxTakeAmount) public override {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L564

```solidity
function offer(
        uint256 pay_amt, 
        ERC20 pay_gem, 
        uint256 buy_amt, 
        ERC20 buy_gem, 
        uint256 pos, 
        bool matching, 
        address owner, 
        address recipient 
    ) public can_offer returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L511

```solidity
function buy(
        uint256 id,
        uint256 amount
    ) public override can_buy(id) returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L314

```solidity
function getBestOffer(
        ERC20 sell_gem,
        ERC20 buy_gem
    ) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L974

```solidity
function getWorseOffer(uint256 id) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L985

```solidity
function getOfferCount(
        ERC20 sell_gem,
        ERC20 buy_gem
    ) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L998

```solidity
function getFirstUnsortedOffer() public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1010

```solidity
function getNextUnsortedOffer(uint256 id) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1016

```solidity
function isOfferSorted(uint256 id) public view returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1020

```solidity
function getBuyAmount(
        ERC20 buy_gem,
        ERC20 pay_gem,
        uint256 pay_amt
    ) public view returns (uint256 fill_amt) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1124

```solidity
function getPayAmountWithFee(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint256 buy_amt
    ) public view returns (uint256 fill_amt) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1149

```solidity
function getPayAmount(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint256 buy_amt
    ) public view returns (uint256 fill_amt) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1157

```solidity
function lastTimeRewardApplicable(
        address token
    ) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L112

```solidity
function rewardPerToken(address token) public view returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L121

```solidity
function earned(
        address account,
        address token
    ) public view override returns (uint256) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L17

```solidity
function borrowBalance(address bathToken) public returns (uint256 balance) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L65

```solidity
function borrowBalanceOfPos(
        uint256 posId
    ) public view returns (uint256 balance) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L72

```solidity
function borrowRate(address bathToken) public view returns (uint256 rate) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L86




### <a href="#Summary">[GAS&#x2011;13]</a><a name="GAS&#x2011;13"> Non-usage of specific imports

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



### <a href="#Summary">[GAS&#x2011;14]</a><a name="GAS&#x2011;14"> Shorten the array rather than copying to a new one

Inline-assembly can be used to shorten the array by changing the length slot, so that the entries don't have to be copied to a new, shorter array

#### <ins>Proof Of Concept</ins>


```solidity
337: address[] memory _bathTokens = new address[](1);

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L337







### <a href="#Summary">[GAS&#x2011;15]</a><a name="GAS&#x2011;15"> Structs can be packed into fewer storage slots

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

#### <ins>Proof Of Concept</ins>

```solidity
33: struct Position {
        address asset; // supplied as collateral
        address quote; // borrowed token
        uint256 borrowedAmount; // amount of borrowed quote
        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
        uint256 blockNum; // block number on which position was opened
        bool isActive; // false by default, when active - true
    }

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L33

Can save up to 2 storage slots of we are ever unlikely to reach the max uint256 for amounts:

```solidity
33: struct Position {
        address asset; // supplied as collateral
        address quote; // borrowed token
        bool isActive; // false by default, when active - true
        uint128 borrowedAmount; // amount of borrowed quote
        uint128 bathTokenAmount; // amount of bathTokens to which collateral was supplied
        uint256 blockNum; // block number on which position was opened
    }

```





### <a href="#Summary">[GAS&#x2011;16]</a><a name="GAS&#x2011;16"> Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead

When using elements that are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html
Each operation involving a `uint8` costs an extra 22-28 gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed

#### <ins>Proof Of Concept</ins>


```solidity
240: uint64 timestamp;
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L240







### <a href="#Summary">[GAS&#x2011;17]</a><a name="GAS&#x2011;17"> Using `unchecked` blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isnâ€™t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an `unchecked` block

#### <ins>Proof Of Concept</ins>


```solidity
943: _rank[id].delb < block.number - 10

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L943

```solidity
120: _msgValue = msg.value - _feeAmount;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol#L120





### <a href="#Summary">[GAS&#x2011;18]</a><a name="GAS&#x2011;18"> Use assembly to check for `address(0)`

Save 6 gas per instance if using assembly to check for address(0)

e.g.
```
assembly {
    if iszero(_addr) {
        mstore(0x00, "AddressZero")
        revert(0x00, 0x20)
    }
}
```

#### <ins>Proof Of Concept</ins>


```solidity
function _bathify(
        address _underlying
    )
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)
    {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L137

```solidity
function initialize(address _feeTo) public {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L700

```solidity
function setFeeTo(address newFeeTo) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1476







### <a href="#Summary">[GAS&#x2011;19]</a><a name="GAS&#x2011;19"> Use solidity version 0.8.19 to gain some gas boost

Upgrade to the latest solidity version 0.8.19 to get additional gas savings.
See latest release for reference: https://blog.soliditylang.org/2023/02/22/solidity-0.8.19-release-announcement/

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


