1.  

There is no check on the safeTrasnferFrom to ensure enough asset is transferred
```solidity 
IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);
```
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L142-L144

Recommendation:
checks the transferred amount to ensure the amount  transferred is at least as much as the `initMargin`

```solidity 
IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);
@> require(IERC20(asset).balanceOf(address(this)) >= vars.initAssetBalance + initMargin);
```

---

2. 

In function `openPosition` bool OK is unnecessary, since there is no scenario where OK = false and gets return. All logics involved in the `openPosition` would simply revert if checks fail.

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L125-L206

Recommendation:

Simply call openPosition without returning bool, since there is no case that a false boolean `OK` would be returned.

---

3. can_cancel in RubiconMarket has a useless isClosed() check which always gives false

```solidity
    /// @dev After close, anyone can cancel an offer.
    modifier can_cancel(uint256 id) virtual override {
        require(isActive(id));
        require(
            (msg.sender == getOwner(id)) ||
                isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );
        _;
    }

    function isClosed() public pure returns (bool closed) {
        return false;
    }
```

Recommendation:
remove isClose() in the function can_cancel.