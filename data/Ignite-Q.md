### Low Issues
| |Issue|
|---|---|
| Low-1 | Anyone can transfer any token held in the `FeeWrapper` contract |
| Low-2 | The `rewardsToken` should be withdrawn by the owner when the period has finished |

### [Low-1] Anyone can transfer any token held in the `FeeWrapper` contract

## Impact
Anyone can transfer any token held in the `FeeWrapper` contract.

## Proof of Concept

The `FeeWrapper` contract allows third-party protocols to charge their own fees by calling `rubicall()` function.

```solidity=48
    function rubicall(
        CallParams memory params
    ) external payable returns (bytes memory) {
        if (msg.value == 0) {
            return _rubicall(params);
        } else {
            return _rubicallPayable(params);
        }
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L48-L56

If the value of `_params.args` passed to the transfer function at line 105 is lower than the amount of tokens being transferred, there will be some tokens left in the contract.
```solidity=60
    function _rubicall(
        CallParams memory _params
    ) internal returns (bytes memory) {
        // charge fee from feeParams
        _chargeFee(_params.feeParams, _params.target);

        (bool _OK, bytes memory _data) = _params.target.call(
            bytes.concat(_params.selector, _params.args)
        );

        require(_OK, "low-level call to the Rubicon failed");

        return _data;
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L60-L73

```solidity=93
    function _chargeFee(FeeParams memory _feeParams, address _target) internal {
        address _feeToken = _feeParams.feeToken;
        uint256 _totalAmount = _feeParams.totalAmount;
        uint256 _feeAmount = _feeParams.feeAmount;
        address _feeTo = _feeParams.feeTo;

        // transfer total amount to the FeeWrapper
        IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);
        // transfer fee to the 3rd party protocol
        IERC20(_feeToken).transfer(_feeTo, _feeAmount);

        // approve tokens to the `_target`
        IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93-L106

As a result, any user can transfer the remaining tokens in the contract to themselves by calling the `rubicall()` function.

## Tools Used
Manual Review.

## Recommended Mitigation Steps
Add access control to the `rubicall()` function to ensure that only trusted third-party protocols can transfer funds.

### [Low-2] The `rewardsToken` should be withdrawn by the owner when the period has finished

## Impact
Rewards tokens may be locked in the contract.

## Proof of Concept

The `notifyRewardAmount()` function is used by the onlyOwner to set the `rewardRates` in the `BathBuddy` contract.

```solidity=191
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

        // Ensure the provided reward amount is not more than the balance in the contract.
        // This keeps the reward rate in the right range, preventing overflows due to
        // very high values of rewardRate in the earned and rewardsPerToken functions;
        // Reward + leftover must be less than 2^256 / 10^18 to avoid overflow.
        // Note********** ERC20s must be here*************
        uint256 balance = rewardsToken.balanceOf(address(this));
        require(
            rewardRates[address(rewardsToken)] <=
                balance.div(rewardsDuration[address(rewardsToken)]),
            "Provided reward too high"
        );

        lastUpdateTime[address(rewardsToken)] = block.timestamp;
        periodFinish[address(rewardsToken)] = block.timestamp.add(
            rewardsDuration[address(rewardsToken)]
        );
        emit RewardAdded(reward);
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191-L228

Since the `rewardRates` must be lesser or equal to `balance.div(rewardsDuration[address(rewardsToken)])` at lines 217-221, the owner needs to send `rewardsToken` to the contract first.

If the owner miscalculates the `rewardRates` and has already sent the `rewardsToken` to the contract, the `rewardsToken` will be locked in the contract.

## Tools Used
Manual Review.

## Recommended Mitigation Steps
An `withdraw()` function should be added for the owner to withdraw `rewardsToken` when either the `periodFinish` has not been set or has already passed.

### Non-Critical Issues

| | Issues | Instances |
| -------- | -------- | -------- |
| NC-1     | Using `uint256` rather than `uint`     | 10 |
| NC-2     | Typo revert string                     |  2 |
| NC-3     | Mislead revert string                  |  1 |
| NC-4     | Floating pragma                        |  2 |
| NC-5     | NatSpec should be increased | All Contracts |
### [NC-1] Using `uint256` rather than `uint`

Using `uint256` instead of `uint` is for clarity and consistency. Using `uint256` indicates that you are specifically declaring an unsigned integer of 256 bits, while using `uint` may be less clear and may lead to confusion with other integer types that have a different number of bits.

#### <ins>Instances(10):</ins>

```solidity
785: uint pos, //position to insert offer, 0 should be used if unknown

888: uint[] calldata payAmts,

890: uint[] calldata buyAmts,

899: for (uint i = 0; i < payAmts.length; i++) {
    
910: function batchCancel(uint[] calldata ids) external {

911: for (uint i = 0; i < ids.length; i++) {
    
918: uint[] calldata ids,

919: uint[] calldata payAmts,
    
921: uint[] calldata buyAmts,
    
924: for (uint i = 0; i < ids.length; i++) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L785

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L888

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L890

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L910

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L918

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L919

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L921

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924
#### <ins>Recommendation</ins>
Using `uint256` rather than `uint`.

### [NC-2] Typo revert string

#### <ins>Instances(2):</ins>

```solidity
593: "_leverageCheck{Long}: INVLAID LEVERAGE"

597: "_leverageCheck{Short}: INVLAID LEVERAGE"
```
Correct[593]: "_leverageCheck{Long}: INVALID LEVERAGE"

Correct[597]: "_leverageCheck{Short}: INVALID LEVERAGE"

#### <ins>Recommendation</ins>
Correct the typo revert string.

### [NC-3] Mislead revert string

For example:
```solidity
115: require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115

`FeeWrapper: not enough ETH sent` could mislead protocols into thinking that the issue is with the amount of ETH sent, when in fact the issue could be with other factors such as protocol send more `msg.value` than `_totalAmount`.

#### <ins>Instances(1):</ins>
```solidity
115: require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115

#### <ins>Recommendation</ins>
Correcting the revert string to accurately reflect the condition.

For example:
```solidity
115: require(msg.value == _totalAmount, "FeeWrapper: incorrect amount of ETH sent. Please send exactly [totalAmount].");
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115

### [N-04] Floating pragma

When deploying a contract, it is important to use the same compiler version and settings that were used during thorough testing. To prevent accidentally using an outdated compiler version, it's a good practice to set a specific compiler version. Using an outdated compiler version can cause known bugs (https://github.com/ethereum/solidity/releases).

#### <ins>Instances(2):</ins>

```solidity
pragma solidity ^0.8.9;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

#### <ins>Recommendation</ins>
Lock the pragma version.


### [N-05] NatSpec should be increased

It is important to fully annotate all public interfaces in Solidity contracts using NatSpec. This makes it easier to read and audit the code, which is especially important in complex projects like Defi. Solidity official documentation recommends doing so.
https://docs.soliditylang.org/en/v0.8.19/natspec-format.html

#### <ins>Instances:</ins>

All Contracts

#### <ins>Recommendation</ins>
Increasing the NatSpec comments in contracts.