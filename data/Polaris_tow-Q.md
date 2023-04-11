## May cause admins to set incorrect reward times
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L191-L209
```
    function notifyRewardAmount(
        uint256 reward,
        IERC20 rewardsToken
    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
        if (block.timestamp >= periodFinish[address(rewardsToken)]) {
            rewardRates[address(rewardsToken)] = reward.div(
                rewardsDuration[address(rewardsToken)]
            );
        } else {
            uint256 remaining = periodFinish[address(rewardsToken)].sub(
                block.timestamp
            );
            uint256 leftover = remaining.mul(
                rewardRates[address(rewardsToken)]
            );
            rewardRates[address(rewardsToken)] = reward.add(leftover).div(
                rewardsDuration[address(rewardsToken)]
            );
        }
```
In this code, the notifyRewardAmount() function takes the reward amount and the token address as parameters. If the current time is greater than or equal to periodFinish[token] (i.e., the end of the previous reward period), it calculates the new reward rate and sets the end time for the next reward period. Otherwise, it calculates the remaining reward and adds it to the total for the new reward.

The potential issue with this code is that if the administrator sets the wrong rewardsDuration[token] value during the previous reward period, it may cause the new reward period to overlap with the old one, resulting in duplicate rewards and possibly insufficient funds in the contract to pay out all the rewards.

To address this issue, a timer or other mechanism could be used to ensure that the previous reward period has ended, and to verify that the rewardsDuration[token] value entered by the administrator is less than or equal to the remaining time in the current reward period. This would ensure that the new reward period always starts after the previous one ends and avoid overlapping rewards.

## Race conditions
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L38-L75
When multiple users attempt to migrate at the same time, race conditions can occur. To address this issue, atomic operations and synchronization mechanisms can be used.
## Unsafe usage of ERC20 transfer and transferFrom

tranfser/transferfrom is directly used to send tokens in many places in the contract and the return value is not checked.Use the OpenZepplin’s safeTransfer and safeTransferFrom functions.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L339-L342
```
        require(
            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
            "Insufficient funds to cover fee"
        );
```