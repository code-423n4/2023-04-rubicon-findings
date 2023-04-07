G1. There is no need to update the state variable unless there is an increase. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L248](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L248)

Correction: 
```diff
- rewardsPerTokensStored[token] = rewardPerToken(token);
+ uint256 rpt = rewardPerToken(token);
+ if(rpt > rewardsPerTokensStored[token]) rewardsPerTokensStored[token] = rpt;

```