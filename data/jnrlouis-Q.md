## Low Risk Issues

# [L-01] Front-runable Initializer

There is nothing preventing another account from calling the `initializer` before the contract owner. In the best case, the owner is forced to waste gas and re-deploy. In the worst case, the owner does not notice that his/her call reverts, and everyone starts using a contract under the control of an attacker.

*There are 2 instances of this issue:*

```javascript
32      function initialize(address _comptroller, address _pAdmin) external
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32

```javascript
File: contracts/RubiconMarket.sol

700     function initialize(address _feeTo) public
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700

# [L-02] Return value from `transfer`, `transferFrom` and `approval` is ignored

Not all IERC20 implementations revert() when there’s a failure in the `approve()`, `transfer()` or `transferFrom()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything.

*There are 7 instances of this issue:*

```javascript
File: contracts/V2Migrator.sol

        bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);

        underlying.approve(bathTokenV2, amountWithdrawn);

        /// @dev v2 bathTokens shouldn't be sent to this contract from anywhere other than this function
        IERC20(bathTokenV2).transfer
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L44

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L53

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59

```javascript
File: contracts/utilities/FeeWrapper.sol

        IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);
        // transfer fee to the 3rd party protocol
        IERC20(_feeToken).transfer(_feeTo, _feeAmount);


        // approve tokens to the `_target`
        IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));

```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L100-L105

```javascript
File: contracts/utilities/poolsUtility/Position.sol

493        IERC20(_asset).transferFrom
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L493

# [L-03] Input Validation not done

Input should be properly validated to ensure correct behaviour. The address `token` should be checked for `address(0)`.

*There is 1 instance of this issue:*

```javascript
File: contracts/periphery/BathBuddy.sol

    function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232-L235

