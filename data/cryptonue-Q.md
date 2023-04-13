# [L] `claimRewards` will claim all users's reward from compound market, while it might just be reward from CToken registered via rubicon

in `BathHouseV2.sol`, the claimRewards function will claim all existing position from the user in all compound market (by using `comptroller.claimComp(msg.sender)`), even though it's not started from Rubicon.

There is another parameter alternative

```js
function claimComp(address holder, CToken[] memory cTokens) public
```

which can be used to just select the related cToken with user's involvement with rubicon

```js
File: BathHouseV2.sol
115:     function claimRewards(
116:         address[] memory buddies,
117:         address[] memory rewardsTokens
118:     ) external {
119:         // claim rewards from comptroller
120:         comptroller.claimComp(msg.sender);
121:         // get rewards from bathBuddy
122:         for (uint256 i = 0; i < buddies.length; ++i) {
123:             IBathBuddy(buddies[i]).getReward(
124:                 IERC20(rewardsTokens[i]),
125:                 msg.sender
126:             );
127:         }
128:     }
```

# [L] In several occasion, there are no check of the same length between pair of arrays

For example, array length of bathTokensV1 and bathTokensV2.
also, the buddies and rewardsTokens.

```js
File: V2Migrator.sol
30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {
32:             // set v1 to v2 bathTokens
33:             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
34:         }
35:     }

File: BathHouseV2.sol
115:     function claimRewards(
116:         address[] memory buddies,
117:         address[] memory rewardsTokens
118:     ) external {
119:         // claim rewards from comptroller
120:         comptroller.claimComp(msg.sender);
121:         // get rewards from bathBuddy
122:         for (uint256 i = 0; i < buddies.length; ++i) {
123:             IBathBuddy(buddies[i]).getReward(
124:                 IERC20(rewardsTokens[i]),
125:                 msg.sender
126:             );
127:         }
128:     }
```

Simply, it just need to be add:

```js
require(bathTokensV1.length == bathTokensV2.length, "different length");
require(buddies.length == rewardsTokens.length, "different length");
```

# [N] Use struct for same mapping variables

There are several mapping variables which actually can be structured as `struct` since the key of mapping for all those variables is same

```js
File: BathBuddy.sol
52:     mapping(address => uint256) public periodFinish; // Token specific
53:     mapping(address => uint256) public rewardRates; // Token specific reward rates
54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:     mapping(address => uint256) public lastUpdateTime; //Token specific
56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific
```

can be changed to:

```js
struct TokenData{
  uint256 periodFinish;
  uint256 rewardRates;
  uint256 rewardsDuration;
  uint256 lastUpdateTime;
  uint256 rewardsPerTokensStored;
}
mapping(address => TokenData) public tokenData;
```
