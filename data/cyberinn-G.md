# Gas Optimizations Report


## [G-1] Use a more recent version of solidity

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


## [G-2] Splitting require() statements that use `&&` saves gas

### Impact
When using `&&` in a `require()` statement, the entire statement will be evaluated before the transaction reverts. If the first condition is false, the second condition will not be evaluated. This means that if the first condition is false, the second condition will not be evaluated, which is a waste of gas. Splitting the `require()` statement into two separate statements will save gas.

### Findings
Total:3

[contracts/RubiconMarket.sol#L893-L1415](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L893-L1415)
```solidity
893:    require(
894:                payAmts.length == payGems.length &&
895:                    payAmts.length == buyAmts.length &&
896:                    payAmts.length == buyGems.length,
897:                "Array lengths do not match"
898:            );
...
940:    require(
941:                !isActive(id) &&
942:                    _rank[id].delb != 0 &&
943:                    _rank[id].delb < block.number - 10
944:            );
...
1412:    require(
1413:                _rank[id].delb == 0 && //assert id is in the sorted list
1414:                    isOfferSorted(id)
1415:            );
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
[contracts/periphery/BathBuddy.sol#L95-L100](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L95-L100)
```solidity
95:    require(
96:                msg.sender == myBathTokenBuddy &&
97:                    msg.sender != address(0) &&
98:                    friendshipStarted,
99:                "You are not my buddy!"
100:            );
```

## [G-3] Functions guaranteed to revert when called by normal users can be marked payable

### Impact
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
   The extra opcodes avoided are `CALLVALUE(2)`,`DUP1(3)`,`ISZERO(3)`,`PUSH2(3)`,`JUMPI(10)`,`PUSH1(3)`,`DUP1(3)`,`REVERT(0)`,`JUMPDEST(1)`,`POP(2)`, which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

### Findings
Total:3

[contracts/BathHouseV2.sol#L60](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L60)
```solidity
60:    function createBathToken(
```
[contracts/periphery/BathBuddy.sol#L191-L232](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L191-L232)
```solidity
191:    function notifyRewardAmount(
...
232:    function setRewardsDuration(
```
[contracts/utilities/poolsUtility/Position.sol#L93](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L93)
```solidity
93:    function buyAllAmountWithLeverage(

107:    function sellAllAmountWithLeverage(

210:    function closePosition(uint256 posId) external onlyOwner {

226:    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:    function withdraw(address token, uint256 amount) external onlyOwner {
```

### Recommendation
Mark the function as payable.

## [G-4] x += y or x -= y costs more gas than x = x + y or x = x - y for state variables

### Impact
Using the addition operator instead of plus-equals saves 113 gas. Usually does not work with struct and mappings.

### Findings
Total:1

[contracts/utilities/poolsUtility/Position.sol#L431-L560](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L431-L560)
```solidity
560:    _assetAmount += _loopBorrowed;
```

## [G-5] Multiple accesses of a `mapping/array` should use a local variable cache

### Impact
The instances below point to the second+ access of a value inside a mapping/array, within a function. Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into `memory/calldata` 

### Findings
Total:2

[contracts/RubiconMarket.sol#L691-L948](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L691-L948)
```solidity
691:    mapping(uint256 => sortInfo) public _rank;
692:    mapping(address => mapping(address => uint256)) public _best;
...
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

...
1273:    function _matcho(
			uint256 t_pay_amt, //taker sell how much
			ERC20 t_pay_gem, //taker sell which token
			uint256 t_buy_amt, //taker buy how much
			ERC20 t_buy_gem, //taker buy which token
			uint256 pos, //position id
			bool rounding, //match "close enough" orders?
			address owner,
			address recipient
		) internal returns (uint256 id) {
			uint256 best_maker_id; //highest maker id
			uint256 t_buy_amt_old; //taker buy how much saved
			uint256 m_buy_amt; //maker offer wants to buy this much token
			uint256 m_pay_amt; //maker offer wants to sell this much token

			// there is at least one offer stored for token pair
			while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
					best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];
...
1405:    function _unsort(
			uint256 id //id of maker (ask) offer to remove from sorted list
		) internal returns (bool) {
			address buy_gem = address(offers[id].buy_gem);
			address pay_gem = address(offers[id].pay_gem);
			require(_span[pay_gem][buy_gem] > 0);

			require(
				_rank[id].delb == 0 && //assert id is in the sorted list
					isOfferSorted(id)
			);

			if (id != _best[pay_gem][buy_gem]) {
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


## [G-6] ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

### Impact
The unchecked keyword is new in solidity version 0.8.0, so this only applies to that version or higher, which these instances are. This saves 30-40 gas [per loop](https://gist.github.com/hrkrshnn/ee8fabd532058307229d65dcd5836ddc#the-increment-in-for-loop-post-condition-can-be-made-unchecked). 

### Findings
Total:7

[contracts/RubiconMarket.sol#L899-L924](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L899-L924)
```solidity
899:    for (uint i = 0; i < payAmts.length; i++) {

911:    for (uint i = 0; i < ids.length; i++) {

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

## [G-7] Optimize names to save gas

### Impact
public/external function names and public member variable names can be optimized to save gas. See [this](https://gist.github.com/IllIllI000/a5d8b486a8259f9f77891a919febd1a9) link for an example of how it works. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call

### Findings
Total:5
[contracts/RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)
```solidity
         ///setOwner,isAuthorized
22:      contract DSAuth is DSAuthEvents {
         ///add,sub,mul...
45:      contract DSMath {
         ///getRecipient,getOwner,isActive,getOffer...
218:     contract SimpleMarket is EventfulMarket, DSMath {
         ///getTime,isClosed
593:     contract ExpiringMarket is DSAuth, SimpleMarket {
         ////getMinSell,getBestOffer,getWorseOffer,getBetterOffer,getOfferCount...
674:     contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {

```
[contracts/utilities/poolsUtility/Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)
```solidity
      ///withdraw,borrowBalance,openPosition...
15:   contract Position is Ownable, DSMath {
```

[contracts/periphery/BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol)

```solidity
       ///getReward,getRewardForDuration,setRewardsDuration,notifyRewardAmount
38:    contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
```

[contracts/BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol)
```solidity
       ///initialize,getBathTokenFromAsset,whoIsBuddy,createBathToken,claimRewards,getReward
12:    contract BathHouseV2 {
```

[contracts/V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol)
```solidity
      /// withdraw, underlyingToken
8:    interface IBathToken is IERC20 {
```

## [G-8] public function that could be declared external

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


