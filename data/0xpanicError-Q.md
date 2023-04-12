# Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Anyone can mint v2BathTokens and send to V2Migrator Contract | 1 |
### [L-1] Anyone can mint v2BathTokens and send to V2Migrator Contract 
V2BathTokens are CTokens that anyone can mint and send to the V2Migrator contract. When transfering V2BathTokens to
`msg.sender`, consider sending only `amountWithdrawn` instead of entire balance of bathTokenV2. 

If this is implemented then require statement that checks `blanceOf(adrress(this)) == 0` needs to be removed. 

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

*Instances (1)*:
```solidity
File: contracts/V2Migrator.sol

55:   CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,

59:   IERC20(bathTokenV2).transfer(

60:   msg.sender,

61:   IERC20(bathTokenV2).balanceOf(address(this))

62:   );

```


# Non-Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [N-1](#N-1) | Check that array lenghts match  | 3 |
| [N-2](#N-2) | No need to use SafeMath on solidity version >= 0.8.0  | 16 |
### [N-1] Check that array lenghts match
If two arrays are taken as input and are matched one to one, make sure their lengths match to prevent any typos.

Consider adding the statement `require(bathTokensV1.length == bathTokensV2.length)`

Consider adding the statement `require(buddies.length == rewardsTokens.length)`

Consider adding the statement `require(payAmts.length == ids.length && payAmts.length == payGems.length && payAmts.length == buyAmts.length && payAmts.length == buyGems.length,)`

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

*Instances (3)*:
```solidity
File: contracts/V2Migrator.sol

30:   constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

31:   for (uint256 i = 0; i < bathTokensV1.length; ++i) {

33:   v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];

```

```solidity
File: contracts/BathHouseV2.sol

115:   function claimRewards(

116:   address[] memory buddies,

117:   address[] memory rewardsTokens

123:   IBathBuddy(buddies[i]).getReward(

124:   IERC20(rewardsTokens[i]),

```

```solidity
File: contracts/RubiconMarkets.sol

262:   cancel(ids[i]);

263:   this.offer(

264:   payAmts[i],

265:   ERC20(payGems[i]),

266:   buyAmts[i],

267:   ERC20(buyGems[i])

```

### [N-2] No need to use SafeMath on solidity version >= 0.8.0
Solidity ^0.8.0 intrinsically checks for arithmetic errors and there's no need to use SafeMath library
to perform arithmetic operations like `add`, `sub`, `mul` and `div`. 

Contracts in scope: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

*Instances (16)*:
```solidity
File: contracts/periphery/BathBuddy.sol

130:   .sub(lastUpdateTime[token])

131:   .mul(rewardRates[token])

132:   .mul(1e18)

133:   .div(IERC20(myBathTokenBuddy).totalSupply())

148:   .mul(

149:   rewardPerToken(token).sub(

153:   .div(1e18)

154:   .add(tokenRewards[token][account]);

160:   return rewardRates[token].mul(rewardsDuration[token]);

196:   rewardRates[address(rewardsToken)] = reward.div(

200:   uint256 remaining = periodFinish[address(rewardsToken)].sub(

203:   uint256 leftover = remaining.mul(

206:   rewardRates[address(rewardsToken)] = reward.add(leftover).div(

219:   balance.div(rewardsDuration[address(rewardsToken)]),

224:   periodFinish[address(rewardsToken)] = block.timestamp.add(

```