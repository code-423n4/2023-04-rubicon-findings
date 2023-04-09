### 3rd party protocol fees can go to `address(0)`

In `_chargeFeePayable` and `_chargeFee` is recommended to check if the `_feeTo` value is different than `address(0)`. In some cases, the protocol can set the value for this field to `address(0)` by mistake, which will cause them to lose the fees.

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L97
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L114

### `calculateFee` can be bypassed

A 3rd party protocol can call the `rubicall` with different fees than the ones calculated by `calculateFee`, this way bypassing the recommended way of calculating the fees. 

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L48-L56

### Remove unused events in the `BathBuddy` smart contract

Unused events:
[L262](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L262): `event Staked(address indexed user, uint256 amount);`
[L263](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L263): `event Withdrawn(address indexed user, uint256 amount);`
[L266](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L266): `event Recovered(address token, uint256 amount);`

### Update `BathBuddy` events to include the reward token address that the event is triggered for

Since the smart contract supports multiple reward tokens, it's a good idea to specify the reward token (not just the reward amount) as a field in the following events:

[L261](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L261): `event RewardAdded(uint256 reward);`
[L264](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L264): `event RewardPaid(uint256 reward);`
[L265](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L265): `event RewardsDurationUpdated(uint256 newDuration);`

### Remove unused `onlyBuddy` modifier in `BathBuddy` contract

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L104-L107

### `BathBuddy` smart contract doesn't implement pausable functionality which makes `Pausable` inheritance redundant

**Context:**
- [L38](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L38):  `contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {`
- [L168-L177](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L168-L177): 

`whenNotPaused` modifier usage:

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
```


The inheritance of `Pausable` contract can be safely removed from the `BathBuddy` contract since the contract doesn't have any function that allows the contract owner to pause/unpause the contract. In this case, the only actions required are removing the `whenNotPaused` modifier usage, `Pausable` inheritance, and the import statement(`import "@openzeppelin/contracts/security/Pausable.sol";`).  

However, it's a good idea for the contract owner to be able to pause the rewards distribution or other user-facing operations. For this, the `BathBuddy` contract should have owner-only functions for pausing or unpausing user-facing operations. Here's an example:

```solidity
contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
...
function pause() external onlyOwner {
        _pause();
    }

    function resume() external onlyOwner {
        _unpause();
    }
...
}
```