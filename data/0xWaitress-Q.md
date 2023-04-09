1.  There is no check on the safeTrasnferFrom to ensure enough asset is transferred
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