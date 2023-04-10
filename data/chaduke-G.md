G1. rewardsPerTokensStored[token] might be updated unnecessarily by updateReward().

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L247-L257](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L247-L257)
 
Mitigation: Change the function ``rewardPerToken()`` so that it will return a flag (true/false) to indicate whether there is a need to ``rewardPoerTokensStored``. 

```diff
- function rewardPerToken(address token) public view returns (uint256) {
+ function rewardPerToken(address token) public view returns (bool shouldupdate, uint256) {

        require(friendshipStarted, "I have not started a bathToken friendship");

        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
-            return rewardsPerTokensStored[token];
+            return (false, rewardsPerTokensStored[token]);
        }
-        return rewardsPerTokensStored[token].add(
-                lastTimeRewardApplicable(token)
-                    .sub(lastUpdateTime[token])
-                    .mul(rewardRates[token])
-                    .mul(1e18)
-                    .div(IERC20(myBathTokenBuddy).totalSupply())
-            );

+        return (true, 
+            rewardsPerTokensStored[token].add(
+                lastTimeRewardApplicable(token)
+                    .sub(lastUpdateTime[token])
+                    .mul(rewardRates[token])
+                    .mul(1e18)
+                    .div(IERC20(myBathTokenBuddy).totalSupply())
+            ));
    }

 modifier updateReward(address account, address token) {
-        rewardsPerTokensStored[token] = rewardPerToken(token);
+        (bool shouldUpdate, uint256 rpt) = rewardPerToken(token);
+        if(shouldUpdate) rewardsPerTokensStored[token] = rpt;

        lastUpdateTime[token] = lastTimeRewardApplicable(token);
        if (account != address(0)) {
            tokenRewards[token][account] = earned(account, token);
            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
                token
            ];
        }
        _;
    }

```