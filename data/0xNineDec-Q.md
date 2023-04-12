# L-1: Lack of parameter emission

When setting a new Reward Duration time in `BathBuddy` the following event is emitted:

```solidity
emit RewardsDurationUpdated(rewardsDuration[token]);
```

The parameters of the mentioned event don't provide enough information for users to track such an important parameter as the duration of the reward period (that alters the reward rate) and of which reward token the duration was modified.

## Mitigation
Emit the older and newer duration period along with the reward token address.

# L-2: Bath Buddy will never be paused

The `BathBuddy` contract uses the `WhenNotPaused` modifier to control the reward claiming process in the event of an abnormal scenario:

```solidity
function getReward(
    IERC20 rewardsToken,
    address holderRecipient
)
    external
    override
    nonReentrant
    whenNotPaused
    updateReward(holderRecipient, address(rewardsToken))
    onlyBathHouse
{
```

However, this contract does not implement the pausing feature inherited by `Openzeppelin/Pausable.sol` that implements both `_pause()` and `_unpause()` as [internal functions](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/Pausable.sol#L89-L104).

## Mitigation
Implement an access controlled function that allows the owner to pause and unpause the contract.