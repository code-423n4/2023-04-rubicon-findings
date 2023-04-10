## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use assembly to check for `address(0)` | 14 |
| [GAS-2](#GAS-2) | Using bools for storage incurs overhead | 7 |
| [GAS-3](#GAS-3) | Cache array length outside of loop | 6 |
| [GAS-4](#GAS-4) | State variables should be cached in stack variables rather than re-reading them from storage | 2 |
| [GAS-5](#GAS-5) | Use calldata instead of memory for function arguments that do not get mutated | 7 |
| [GAS-6](#GAS-6) | Use Custom Errors | 36 |
| [GAS-7](#GAS-7) | Don't initialize variables with default value | 8 |
| [GAS-8](#GAS-8) | Long revert strings | 7 |
| [GAS-9](#GAS-9) | Functions guaranteed to revert when called by normal users can be marked `payable` | 3 |
| [GAS-10](#GAS-10) | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 9 |
| [GAS-11](#GAS-11) | Use shift Right/Left instead of division/multiplication if possible | 4 |
| [GAS-12](#GAS-12) | Use != 0 instead of > 0 for unsigned integer comparison | 21 |
| [GAS-13](#GAS-13) | `internal` functions not called by the contract should be removed | 4 |
| [GAS-14](#GAS-14) | 10 ** 18,10 **  27 and 10 ** 9 can be changed to 1e18 , 1e27 , 1e9 and save some gas | 12 |
| [GAS-15](#GAS-15) | Functions guaranteed to revert when called by normal users can be marked payable | 14 |
| [GAS-16](#GAS-16) | Unuesd events  | 6 |
| [GAS-17](#GAS-17) | Unuesd Functions | 4 |
| [GAS-18](#GAS-18) | With assembly, .call (bool success) transfer can be done gas-optimized | 1 |
| [GAS-19](#GAS-19) | Unused function parameter  | 1 |
| [GAS-20](#GAS-20) | Shifting cheaper than division | 4 |
| [GAS-21](#GAS-21) | Using unchecked blocks to save gas | * |

### [GAS-1] Use assembly to check for `address(0)`
*Saves 6 gas per instance*

*Instances (14)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

69:             tokenToBathToken[underlying] == address(0),

73:             underlying != address(0),

77:             implementation != address(0),

144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

459:         _offer.owner == address(0) && msg.sender == _offer.recipient

615:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

702:         require(_feeTo != address(0));

725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),

1477:         require(newFeeTo != address(0));

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

97:                 msg.sender != address(0) &&

250:         if (account != address(0)) {

```

### [GAS-2] Using bools for storage incurs overhead
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (7)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

18:     bool private initialized;

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

594:     bool public stopped;

675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!

676:     bool public matchingEnabled = true; //true: enable matching,

679:     bool public initialized;

682:     bool public AqueductDistributionLive;

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

49:     bool public friendshipStarted;

```

### [GAS-3] Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (6)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

122:         for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```

### [GAS-4] State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Saves 100 gas per instance*

*Instances (2)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

422:             feeTo,

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

133:                     .div(IERC20(myBathTokenBuddy).totalSupply())

```

### [GAS-5] Use calldata instead of memory for function arguments that do not get mutated
Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage.

*Instances (7)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

65:         bytes memory becomeImplementationData

116:         address[] memory buddies,

117:         address[] memory rewardsTokens

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

29:         uint256[] memory tokenAmounts,

49:         CallParams memory params

```

### [GAS-6] Use Custom Errors
[Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)
Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

*Instances (36)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

27:         require(msg.sender == admin, "onlyAdmin: !admin");

33:         require(!initialized, "BathHouseV2 already initialized!");

144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

31:         require(isAuthorized(msg.sender), "ds-auth-unauthorized");

47:         require((z = x + y) >= x, "ds-math-add-overflow");

51:         require((z = x - y) <= x, "ds-math-sub-underflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

321:         require(uint128(spend) == spend, "spend is not an int");

322:         require(uint128(quantity) == quantity, "quantity is not an int");

701:         require(!initialized, "contract is already initialized");

720:         require(isActive(id), "Offer was deleted or taken, or never existed.");

834:         require(!locked, "Reentrancy attempt");

859:         require(!locked, "Reentrancy attempt");

874:         require(!locked, "Reentrancy attempt");

879:                 require(_hide(id), "can't hide");

1040:             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1065:         require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");

1080:             require(offerId != 0, "offerId == 0");

1444:         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

41:         require(bathBalance > 0, "migrate: ZERO AMOUNT");

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

76:         require(!friendshipStarted, "I already have a buddy!");

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

119: 		require(OK, "ETH transfer failed");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

76:         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

212:         require(pos.isActive, "closePosition: POS ISN'T ACTIVE");

228:         require(pos.isActive, "increaseMargin: POS ISN'T ACTIVE");

312:         require(_err == 0, "_maxBorrow: ERROR");

313:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

314:         require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

318:         require(_max > 0, "_maxBorrow: can't borrow 0");

344:         require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");

```

### [GAS-7] Don't initialize variables with default value

*Instances (8)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

122:         for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

1214:         uint256 old_top = 0;

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

158:         for (uint256 i = 0; i < vars.limit; ++i) {

```

### [GAS-8] Long revert strings

*Instances (7)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

720:         require(isActive(id), "Offer was deleted or taken, or never existed.");

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

76:         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

```

### [GAS-9] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (3)*:
```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {

```

### [GAS-10] `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
*Saves 5 gas per loop*

*Instances (9)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

569:         last_offer_id++;

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

1400:         _span[address(pay_gem)][address(buy_gem)]++;

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

407:         lastPositionId++;

572:                 _limit++;

576:                 _limit++;

580:                 _limit++;

```

### [GAS-11] Use shift Right/Left instead of division/multiplication if possible

*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

78:         z = add(mul(x, y), WAD / 2) / WAD;

82:         z = add(mul(x, y), RAY / 2) / RAY;

86:         z = add(mul(x, WAD), y / 2) / y;

90:         z = add(mul(x, RAY), y / 2) / y;

```

### [GAS-12] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (21)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

277:         return offers[id].timestamp > 0;

345:         if (makerFee() > 0) {

521:         require(pay_amt > 0);

523:         require(buy_amt > 0);

581:         require(amount > 0);

585:         if (makerFee() > 0) {

1037:         while (pay_amt > 0) {

1077:         while (buy_amt > 0) {

1133:             if (pay_amt > 0) {

1166:             if (buy_amt > 0) {

1209:         require(id > 0);

1226:         require(id > 0);

1289:         while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {

1325:             t_buy_amt > 0 &&

1326:             t_pay_amt > 0 &&

1410:         require(_span[pay_gem][buy_gem] > 0);

1452:         while (uid > 0 && uid != id) {

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

41:         require(bathBalance > 0, "migrate: ZERO AMOUNT");

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

180:         if (reward > 0) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

313:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

318:         require(_max > 0, "_maxBorrow: can't borrow 0");

```

### [GAS-13] `internal` functions not called by the contract should be removed
If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {

```
### [G-14] 10 ** 18,10 **  27 and 10 ** 9 can be changed to 1e18 , 1e27 , 1e9 and save some gas

10 ** 18,10 **  27 and 10 ** 9 can be changed to 1e18 , 1e27 , 1e9 to avoid unnecessary arithmetic operation and save gas.

*Instances (12)*:

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;

1057:                    mul(pay_amt, 10 ** 9),
1059:                ) / 10 ** 9;

1099:                    mul(buy_amt, 10 ** 9),
1101:                ) / 10 ** 9;

1142:                    mul(pay_amt, 10 ** 9),

1144:                ) / 10 ** 9;

1175:                    mul(buy_amt, 10 ** 9),

1177:                ) / 10 ** 9;


```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

317:        _max = (_liq.mul(10 ** 18)).div(_price);

331:         ).div(10 ** 18);
```


[G-15] Functions guaranteed to revert when called by normal users can be marked payable

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

*Instances (14)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

25:    function setOwner(address owner_) external auth {
26:        owner = owner_;
27:        emit LogSetOwner(owner);
28:    }

628:    function stop() external auth {
629:        stopped = true;
630:    }

955:    function setMinSell(
956:        ERC20 pay_gem, //token to assign minimum sell amount to
957:        uint256 dust //maker (ask) minimum sell amount
958:    ) external auth note returns (bool) {
959:        _dust[address(pay_gem)] = dust;
960:        emit LogMinSell(address(pay_gem), dust);
961:        return true;
962:    }

1466:    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1467:        feeBPS = _newFeeBPS;
1468:        return true;
1469:    }

1471:    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1472:        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
1473:        return true;
1474:    }

1476:    function setFeeTo(address newFeeTo) external auth returns (bool) {
1477:        require(newFeeTo != address(0));
1478:        feeTo = newFeeTo;
1479:        return true;
1480:    }

```

```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

60:    function createBathToken(
61:        address underlying,
62:        InterestRateModel interestRateModel,
63:        uint256 initialExchangeRateMantissa,
64:        address implementation,
65:        bytes memory becomeImplementationData
66:    ) external onlyAdmin {

191:    function notifyRewardAmount(
192:        uint256 reward,
193:        IERC20 rewardsToken
194:    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
```

```solidity
File: 2023-04-rubicon/contracts/BathBuddy.sol
232:    function setRewardsDuration(
233:        uint256 _rewardsDuration,
234:        address token
235:    ) external onlyOwner {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

93:    function buyAllAmountWithLeverage(
94:        address quote,
95:        address asset,
96:        uint256 quotePayAmount,
97:        uint256 leverage
98:    ) external onlyOwner {


107:    function sellAllAmountWithLeverage(
108:        address asset,
109:        address quote,
110:        uint256 assetPayAmount,
111:        uint256 leverage
112:    ) external onlyOwner {

210:    function closePosition(uint256 posId) external onlyOwner {

226:    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:    function withdraw(address token, uint256 amount) external onlyOwner {
```

[G-16] Unuesd events 

Unused events increase contract size and gas usage at deployment. 
*Instances (6)*:
```solidity
File: 2023-04-rubicon/contracts/BathBuddy.sol

261:    event RewardAdded(uint256 reward);
262:    event Staked(address indexed user, uint256 amount);
263:    event Withdrawn(address indexed user, uint256 amount);
264:    event RewardPaid(address indexed user, uint256 reward);
265:    event RewardsDurationUpdated(uint256 newDuration);
266:    event Recovered(address token, uint256 amount);
```

[G-17] Unuesd Functions 

*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

66:    function imin(int256 x, int256 y) internal pure returns (int256 z) {
67:        return x <= y ? x : y;
68:    }

70:    function imax(int256 x, int256 y) internal pure returns (int256 z) {
71:        return x >= y ? x : y;
72:    }

77:    function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
78:        z = add(mul(x, y), WAD / 2) / WAD;
79:    }

297:   function bump(bytes32 id_) external can_buy(uint256(id_)) {
```

[G-18] With assembly, .call (bool success) transfer can be done gas-optimized

`return` data `(bool OK,)` has to be stored due to EVM architecture, but in a usage like below, ‘out’ and ‘outsize’ values are given (0,0), this storage disappears and gas optimization is provided.

*Instances (1)*:
```diff
File: 2023-04-rubicon/contracts/FeeWrapper.sol

117:        // transfer fee to the 3rd party protocol
-        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
+	 bool OK;
+	 assembly {                                    
+                OK := call(gas(), _feeTo, _feeAmount, 0, 0, 0, 0)
+            }                                             
+                  
119:		require(OK, "ETH transfer failed");
120:        _msgValue = msg.value - _feeAmount;

```

### [G-19] Unused function parameter 

Unused function parameters increase contract size and gas usage at deployment.

*Instances (1)*:
```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

528:         address _asset,
```

### [G-20] Shifting cheaper than division

A division by 2 can be calculated by shifting one to the right. While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity’s division operation also includes a division-by-0 prevention which is bypassed using shifting.


*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol


78:        z = add(mul(x, y), WAD / 2) / WAD;

82:        z = add(mul(x, y), RAY / 2) / RAY;

86:        z = add(mul(x, WAD), y / 2) / y;

90:        z = add(mul(x, RAY), y / 2) / y;
```
Recommended Mitigation Steps

Replace / 2 with >>1.


### [G-21] Using unchecked blocks to save gas

Solidity version 0.8+ comes with implicit overflow and underflow checks on unsigned integers. When an overflow or an underflow isn’t possible (as an example, when a comparison is made before the arithmetic operation), some gas can be saved by using an unchecked block.

For loops, such optimizations would save a lot of gas.

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
        }

```

to 

```diff

        for (uint i = 0; i < ids.length; i++) {
-            cancel(ids[i]);
+		    unchecked{
+	            cancel(ids[i]);
+	        }
	    }
```
