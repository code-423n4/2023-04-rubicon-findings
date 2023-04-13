# rewardPerToken returns confusing value due to decimals error
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L132

In the function `rewardPerToken` it’s assumed that underlying token has 18 decimals. But it’s not always the case. BathTokens are created from the BathHouse `createBathToken` function:

[BathHouseV2.sol#L60](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60)

And we can see that it fetches the underlying token’s decimals: [BathHouseV2.sol#L148](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L148)

And then create the BathToken with those decimals: [BathHouseV2.sol#L87](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L87)

Therefore a token like USDC (6 decimals) would have its associated bathToken with 6 decimals too. If we go back to `rewardPerToken`, and we have a rewardToken with 18 decimals. The `rewardsPerTokensStored` would en up being in base 30.

```solidity
rewardsPerTokensStored[token].add( // base 30
	lastTimeRewardApplicable(token)
	    .sub(lastUpdateTime[token])
	    .mul(rewardRates[token]) // base 18
	    .mul(1e18) // base 36
	    .div(IERC20(myBathTokenBuddy).totalSupply()) // base 30
);
```

Though we can see that the rewards distributed will be right, because in the `earned` function, we divide by `1e18` so the decimal error is “fixed”.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L153

**Mitigation**
don’t multiply by 18, use instead the decimals of the BathToken.
Replace `.mul(1e18)`
with `.mul(IERC20(myBathTokenBuddy).decimals())`