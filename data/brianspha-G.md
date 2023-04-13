## DSAuth

According to the documentation on the contract this contract is used for setting the owner of any contract. It does this by providing functionality and modifiers calls.

#### [GAS-1] isAuthorized function could use int8 to represent true or false

The isAuthorized internal function could use the int8 type to represent true or false

##### Risk

There are no major risks

##### Mitigation

```diff
-        function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }
+       function isAuthorizedInt(address src) internal view returns (uint8) {
        if (src == owner) {
            return 1;
        } else {
            return 0;
        }
    }

```

This will essentially save about ~50 gas units

## SimpleMarket

According to the documentation, this contract is responsible for Core trading logic for ERC-20 pairs, an order book, and transacting of tokens.
#### [GAS-2] OfferInfo can be optimised by packing the variables

Since the contract reads and writes many times during trades the cost could add up over a period of time.

##### Risk

There are no major risks

##### Mitigation

```diff
-     struct OfferInfo {
        uint256 pay_amt;
        ERC20 pay_gem;
        uint256 buy_amt;
        ERC20 buy_gem;
        address recipient;
        uint64 timestamp;
        address owner;
    }
+   struct OfferInfo {
        uint256 pay_amt;
        ERC20 pay_gem;
        ERC20 buy_gem;
        address recipient;
        uint64 timestamp;
        address owner;
    }

```

This saves about 23 gas units which is not significant but could be during peak usage periods when gas costs are high

## BathBuddy
### [GAS-3]- Move all code contained inside modifiers to internal functions

I recommend moving the modifier's require statements into an internal virtual function. This reduces the size of compiled contracts that use the modifiers in this instance BathBuddy.

##### Risk

##### Mitigation

```diff
 -    modifier updateReward(address account, address token) {
        rewardsPerTokensStored[token] = rewardPerToken(token);
        lastUpdateTime[token] = lastTimeRewardApplicable(token);
        if (account != address(0)) {
            tokenRewards[token][account] = earned(account, token);
            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
                token
            ];
        }
        _;
    }
+       modifier updateReward(address account, address token) {
        _updateReward(account, token);
        _;
    }
+     function _updateReward(address account, address token) internal {
        rewardsPerTokensStored[token] = rewardPerToken(token);
        lastUpdateTime[token] = lastTimeRewardApplicable(token);
        if (account != address(0)) {
            tokenRewards[token][account] = earned(account, token);
            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
                token
            ];
        }
    }
```

This saves about ~23 gas units, the same goes for the other modifiers