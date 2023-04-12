# Gas Optimizations:
## Multiple mappings related to the same data can be combined into a single mapping using a struct:
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.
**9 instances** of this issue
Instances on BathHouseV2.sol#L20-L21:
```
BathHouseV2.sol#L20    mapping(address => address) private tokenToBathToken;
BathHouseV2.sol#L21    mapping(address => address) private bathTokenToBuddy;
```
These mappings could be refactored into the following struct and mapping:
```
struct bathMap {
	address tokenToBathToken;
	address bathTokenToBuddy;
}

mapping (address => bathMap) public bathmapping;
```
Instance on BathBuddy.sol#L52-L56
```
mapping(address => uint256) public periodFinish; // Token specific
    mapping(address => uint256) public rewardRates; // Token specific reward rates
    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
    mapping(address => uint256) public lastUpdateTime; //Token specific
    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
    mapping(address => mapping(address => uint256))
        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
    mapping(address => mapping(address => uint256)) public tokenRewards;
```
These mappings could be refactored into the following struct and mapping:
```
struct Param {
	uint256 periodFinish;
	uint256 rewardRates;
	uint256 rewardsDuration;
	uint256 lastUpdateTime;
	uint256 rewardsPerTokensStored;
	 
}
struct Reward {
	mapping (address => uint256) userRewardsPerTokenPaid;
	mapping (address => uint256) tokenRewards;
}
mapping (address => Param) public params;
mapping (address => Reward) public rewards;
```


## using safeMath on solidity version ^0.8.0 wastes gas.
Solidity v0.8.0 introduces built-in overflow checks, so using SafeMath is a waste of gas.
Remove safeMath from
**Instance:**
   `position.sol#L18 using SafeMath for uint256;`
   
## use non-zero to non-zero in synchronized modifier
moving from non-zero value to non-zero value is cheaper
```
modifier synchronized() {
        require(!locked);
        locked = true;//@audit-info moving from non-zero value to non-zero value is cheaper
        _;
        locked = false;
    }
```
change to
```
modifier synchronized() {
        require(locked == 2);
        locked = 1;
        _;
        locked = 2;
    }
```
## Splitting `require()` statements that use `&&` saves gas
Instead of using && on single require check using two require checks can save gas
Instance:
```
RubiconMarket.sol#L894 
require(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );
```
BathBuddy.sol#L94
```
modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }
```
## Use scientific notation to save gas
use `1e18` instead of `10 ** 18`
Instances:
```
Position.sol#L331 ).div(10 ** 18);
Position.sol#L317 _max = (_liq.mul(10 ** 18)).div(_price);
RubiconMarket.sol#1057 mul(pay_amt, 10 ** 9) 
RubiconMarket.sol#1059 ) / 10 ** 9
RubiconMarket.sol#1099 mul(buy_amt, 10 ** 9); 
RubiconMarket.sol#1101 ) / 10 ** 9
RubiconMarket.sol#1142 mul(pay_amt, 10 ** 9)
RubiconMarket.sol#1144 ) / 10 ** 9
RubiconMarket.sol#1175 mul(buy_amt, 10 ** 9)
RubiconMarket.sol#1177 ) / 10 ** 9
```