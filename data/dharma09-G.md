# Gas Optimizations

## Summary

|  | Issue | Instances |
| --- | --- | --- |
| 1 | Structs can be packed into fewer storage slots | 1 |
| 2 | Emitting storage values instead of the memory one. | 1 |
| 3 | State variables only set in the constructor should be declared immutable | 4 |
| 4 | BathBuddy.sol.rewardPerToken(): The results of totalSupply() should be cached instead of calling it twice | 1 |
| 5 | Splitting require() statements that uses && saves gas | 4 |
| 6 | Use nested if and, avoid multiple check combinations | 3 |
| 7 | Using delete instead of  0 saves gas | 2 |
| 8 | A modifier used only once and not being inherited should be inlined to save gas  | 2 |
| 9 | x += y (x -= y) costs more gas than x = x + y (x = x - y) for state variables | 1 |
| 10 | ++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as in the case when used in for & while loops  | 1 |


## Findings

### [G-01] **Structs can be packed into fewer storage slots**

Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

*There are 1 instances of this issue:*

```solidity
File: /contracts/utilities/poolsUtility/Position.sol
33: struct Position {
34:        address asset; // supplied as collateral //@audit: 20 bytes
35:        address quote; // borrowed token  //@audit: 20 bytes
36:        uint256 borrowedAmount; // amount of borrowed quote  //@audit: 32 bytes
37:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied38:  //@audit: 32 bytes 
38:        uint256 blockNum; // block number on which position was opened  //@audit: 32 bytes
39:        bool isActive; // false by default, when active - true //@audit: 1 byte
40:    }
```

- **Mitigation**

```diff
File: /contracts/utilities/poolsUtility/Position.sol
33: struct Position {
34:        address asset; // supplied as collateral
35:        address quote; // borrowed token
+36:       bool isActive; // false by default, when active - true
+37:        uint256 borrowedAmount; // amount of borrowed quote
+38:        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied38:
+39:        uint256 blockNum; // block number on which position was opened
- 39:        bool isActive; // false by default, when active - true
40:    }

```

- [Position.sol#L33](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L33)

### **[G-02] Emitting storage values instead of the memory one.**

Here, the values emitted shouldn’t be read from storage. The existing memory values should be used instead:

*There are 1 instances of this issue:*

```solidity
File: /contracts/periphery/BathBuddy.sol
	232: function setRewardsDuration(
	233:        uint256 _rewardsDuration,
	234:        address token
	235:    ) external onlyOwner {
	236:        require(
	237:            block.timestamp > periodFinish[token],
	238:            "Previous rewards period must be complete before changing the duration for the new period"
	239:        );
	240:        rewardsDuration[token] = _rewardsDuration;
	241:        emit RewardsDurationUpdated(rewardsDuration[token]); //@audit: Use cache value
	242:	    }
```

- [BathBuddy.sol#L240](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L240)

### [G-03] **State variables only set in the constructor should be declared `immutable`**

**Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a `PUSH32` (3 gas)**

*There are 4 instances of this issue:*

```solidity
File: /contracts/utilities/poolsUtility/Position.sol
	55: Comptroller public comptroller;
	56: PriceOracle public oracle;
	57: RubiconMarket public rubiconMarket;
	58: BathHouseV2 public bathHouseV2;
```

- [https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44)

### **[G-04]** BathBuddy.sol**.**rewardPerToken**(): The results of `totalSupply()` should be cached instead of calling it twice**

```solidity
File: /contracts/periphery/BathBuddy.sol
121:function rewardPerToken(address token) public view returns (uint256) {
122:        require(friendshipStarted, "I have not started a bathToken friendship");
123:
124:        if (IERC20(myBathTokenBuddy).totalSupply() == 0) { //@audit: First call(Cache totalSupply())
125:            return rewardsPerTokensStored[token];
126:        }
127:        return
128:            rewardsPerTokensStored[token].add(
129:            lastTimeRewardApplicable(token)
130:                    .sub(lastUpdateTime[token])
131:                    .mul(rewardRates[token])
132:                    .mul(1e18)
133:                    .div(IERC20(myBathTokenBuddy).totalSupply())  //@audit: Second call(Cache totalSupply())
134:            );
135:    }
```

- [https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L121](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L121)

### **[G-05] Splitting `require()` statements that use `&&` saves gas**

See [this issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper

*There are 4 instances of this issue:*

```solidity
File: /contracts/periphery/BathBuddy.sol
	95: require(
	96:            msg.sender == myBathTokenBuddy &&
	97:                msg.sender != address(0) &&
	98:                friendshipStarted,
	99:            "You are not my buddy!"
	100:        );
```

- [https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95)

```solidity

893: require(
894:            payAmts.length == payGems.length &&
895:                payAmts.length == buyAmts.length &&
896:                payAmts.length == buyGems.length,
897:            "Array lengths do not match"
898:        );
...
940: require(
941:            !isActive(id) &&
942:                _rank[id].delb != 0 &&
943:                _rank[id].delb < block.number - 10
944:        );
...
1412: require(
1413:            _rank[id].delb == 0 && //assert id is in the sorted list
1414:                isOfferSorted(id)
1415:        );
```

- [https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L894](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L894)

### [G-06] **Use nested if and, avoid multiple check combinations**

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

*There are 3 instances of this issue:*

```jsx
File: /contracts/utilities/poolsUtility/Position.sol
	391: if (
	392:            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
	393:            borrowBalance(_bathTokenAsset) == 0
	394:        ) {

File: /contracts/RubiconMarket.sol
	1197: if (
	1198:            isActive(id) &&
	1199:            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
	1200:        ) {
...
	1324: if (
	1325:            t_buy_amt > 0 &&
	1326:            t_pay_amt > 0 &&
	1327:            t_pay_amt >= _dust[address(t_pay_gem)]
	1328:        ) {
```

### [G-07] ****Using delete instead of  0 saves gas****

*There are 2 instances of this issue:*

```jsx
File: /contracts/RubiconMarket.sol
	1449: _near[id] = 0;
	1462: _near[id] = 0
```

- [RubiconMarket.sol#L1449](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449)

### **[G-08] A modifier used only once and not being inherited should be inlined to save gas**

*There are 2 instances of this issue:*

```solidity
File: /contracts/periphery/BathBuddy.sol
	104: modifier onlyBathHouse() {
	105:        require(msg.sender == bathHouse, "You are not my beloved bath house!");
	106:        _;
	107:    }

File: /contracts/BathHouseV2.sol
	26: modifier onlyAdmin() {
	27:        require(msg.sender == admin, "onlyAdmin: !admin");
	28:        _;
	29:    }
	
```

- [BathBuddy.sol#L104](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L104)
- [BathHouseV2.sol#L26](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L26)

### [G-09] ****`x += y (x -= y)` costs more gas than `x = x + y (x = x - y)` for state variables**

*There are 1 instances of this issue:*

```solidity
File: /contracts/utilities/poolsUtility/Position.sol
	431: positions[_positionId].bathTokenAmount += _bathTokenAmount;
```

- [Position.sol#L431](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431)

### [G-10] `++i/i++` should be `unchecked{++i}/unchecked{i++}` when it is not possible for them to overflow, as in the case when used in for & while loops**

*There are 1 instances of this issue:*

```solidity
File: /contracts/utilities/poolsUtility/Position.sol
	407: lastPositionId++;
```

- [Position.sol#L407](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L407)