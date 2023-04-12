
### Low Risk Issues
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [L-01] | lack of check of array.length of `buddies` and `rewarstokens` | 1 |
| [L-02] | function `getOwner` dosen't show the all possible owners after the migration | 1 |
| [L-03] |lack of check of address(0) in claim reward functions | 1 |
| [L-04] | consider adding `withdraw` function in feewrapper.sol | 1 | 
| [L-04] | bad indexing of the variables / arrays | 1 | 

| Total Low Risk Issues | 5 | 
|:--:|:--:|

### [L-01] lack of check of array.length of `buddies` and `rewarstokens`
The following `claimRewards` function in feewraper.sol dosen't check if the length of both the arrays is equal yet proceeds to carry out the function without providing a proper reason for the revert cause.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115
```solidity
function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens               
    ) external {
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);        //@audit-issue if length of two arrays ain't equal 
        // get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }
```

### [L-02] function `getOwner` dosen't show the all possible owners after the migration
The owner field in the OfferInfo struct was replaced with recipient, and owner itself has been moved to the end of the struct. recipient now corresponds to the recipient of the offer's fill, while owner refers to the owner and maker of that offer.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L280
```solidity
function getOwner(uint256 id) public view returns (address owner) { 
        return offers[id].owner;
    }
```
so consider adding 
```solidity
if ( offers[id].owner == address(0)){
return offers[id].Recipient;}
```
### [L-03] lack of check of address(0) in claim reward functions
The following functions dosen't check if the address of buddy/rewardstoken parameter is 0 which might lead to silent revert without actually revering an error log
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L131
```solidity
function getReward(address buddy, address rewardsToken) external {
        IBathBuddy(buddy).getReward(IERC20(rewardsToken), msg.sender);
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115
```solidity
function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens               
    ) external {
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);         
        // get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }
```
### [L-04] consider adding `withdraw` function in feewrapper.sol 
several functions in the contract feewrapper.sol use CALL method to transfer funds when the call is reverted the funds are locked up in the contract cannot be withdrawn 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L66

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L82

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L118


### [L-05] bad indexing of the variables 
variable indexing starts from index 1 instead of 0

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L407
```solidity
function _savePosition(
        address _asset,
        address _quote,
        uint256 _borrowedAmount,
        uint256 _currentBathTokenAmount
    ) internal {
        lastPositionId++;
        Position memory pos = Position(  //@audit-issue position0 is unused?
            _asset,
            _quote,
            _borrowedAmount,
            _currentBathTokenAmount,
            block.number,
            true
        );
        positions[lastPositionId] = pos;

        emit PositionOpened(lastPositionId, pos);
    }
```

consider increasing the variable (++) at the end of the function