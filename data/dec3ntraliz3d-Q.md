

## Low-01

`batchRequote()` function don't check for matching array length which can cause `Array out of bound` error if not properly handled.

## Summary 

This function takes 5 arrays as input. `ids`, `payAmts`, `payGems`,`buyAmts`,`buyGems` to update offers. However it doesn't check if the array length is same for all the arrays. 


[Link to the code on Github](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933)

```solidity

    /// @notice Update outstanding offers to new offers, in a batch, in a single transaction
    function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {

        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }
    }

```

Modify function as below to address this issue.


```solidity

    /// @notice Update outstanding offers to new offers, in a batch, in a single transaction
    function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
        require(
            ids.length == payAmts.length &&
                ids.length == payGems.length &&
                ids.length == buyAmts.length &&
                ids.length == buyGems.length,
            "Array lengths do not match"
        );
        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }
    }

```


## Low-02

`claimRewards()` function in `BathHouseV2.sol`  don't check for matching array length which can cause `Array out of bound` error if not handled. 

## Summary 

This function takes an array of `buddies` and `rewardsTokens`, however it doesn't check if the array length is same.

[Link to the code on Github](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L128)

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

Update the function as below .

```solidity

    function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {

     require(buddies.length == rewardsTokens.length,
    "Array lengths do not match"
);
        
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
