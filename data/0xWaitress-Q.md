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

---\n

2. 

In function `openPosition` bool OK is unnecessary, since there is no scenario where OK = false and gets return. All logics involved in the `openPosition` would simply revert if checks fail.

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L125-L206

Recommendation:

Simply call openPosition without returning bool, since there is no case that a false boolean `OK` would be returned.

---\n

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


--- \n
4. 
In function `_unsort ` bool return is unnecessary, since there is no scenario where false is returned. All logics involved in the `_unsort` would simply revert if checks fail.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1405

Recommendation:

Simply call _unsort without returning bool, since there is no case that a false boolean would be returned.

--- \n

5. 
Use struct in batchOffer if all arguments have the same lengths and the length checks are not needed:
```solidity
// *** Batch Functionality ***
    /// @notice Batch offer functionality - multuple offers in a single transaction
    function batchOffer(
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
    require(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );
}
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L886-L898

Recommendation:

```solidity
struct BatchOfferInfo {
   uint payAmt;
   address payGem;
   uint buyAmt;
   address buyGem;
};
function batchOffer(BatchOfferInfo[] calldata offers) {
    _;
}
```
Same suggestion for function `batchRequote`


--- \n

6. Position.isActive is an unnecessary field since it is set to false in any case

Ther only way position is created is through _savePosition, which always create a position with isActive = true. And there is no way to set this field to false in the entire codebase.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L39

```solidity
    function _savePosition(
        address _asset,
        address _quote,
        uint256 _borrowedAmount,
        uint256 _currentBathTokenAmount
    ) internal {
        lastPositionId++;
        Position memory pos = Position(
            _asset,
            _quote,
            _borrowedAmount,
            _currentBathTokenAmount,
            block.number,
            true
        );
```