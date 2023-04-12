# Dead Code In Bathbuddy

## Description

The code contains an unused modifier called `onlyBuddy()`. This modifier is never called in the code, and it does not serve any purpose. It appears to have been written but was never implemented.

## Code Location
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94
```javascript
    modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }
```


## Recommendation
To ensure clean and concise code, it is recommended to remove this unnecessary modifier. Leaving unused code in the program increases complexity and reduces code readability, and may also confuse developers who are working on the code in the future. Therefore, it is best to remove the modifier to avoid any confusion and reduce the size of the codebase.


# Do Not Use State Variable For Event Emitting

## Description
In the `BathBuddy.sol` contract, the `setRewardsDuration` function is currently emitting an event that emits the `rewardsDuration` state variable. This variable is then immediately read by the event listener, causing unnecessary gas consumption. To improve efficiency, the function parameter can be used instead of the state variable, which can eliminate the need for reading the state variable from the event listener.

## Code Location
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L241
```javascript
    function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );
        rewardsDuration[token] = _rewardsDuration;
        emit RewardsDurationUpdated(rewardsDuration[token]);
    }
```

## Recommendation
It is recommended to use the `_rewardsDuration` function parameter instead of the `rewardsDuration` state variable in the `setRewardsDuration` function. By doing so, the emit statement can be updated to `emit` the `_rewardsDuration` parameter, and the event listener can be updated to use the parameter instead of the state variable.

Example of the modified code:

```javascript
function setRewardsDuration(
    uint256 _rewardsDuration,
    address token
) external onlyOwner {
    require(
        block.timestamp > periodFinish[token],
        "Previous rewards period must be complete before changing the duration for the new period"
    );
    rewardsDuration[token] = _rewardsDuration;
    emit RewardsDurationUpdated(_rewardsDuration);
}
```

# State Variable Can Be Made Immutable

## Description
In the `BathHouseV2` contract, the variables `proxyAdmin`, `admin`, and `comptroller` are declared as regular state variables but never set or modified after the initialization. These variables can be safely declared as immutable to save gas and improve code efficiency.

## Code Location
```javascript
contract BathHouseV2 {
    /// @notice unitroller's address
    Comptroller public comptroller;
    address public admin;
    // guy who manages BathTokens
    address public proxyAdmin;
    bool private initialized;

    ...
    ...
    function initialize(address _comptroller, address _pAdmin) external {
        require(!initialized, "BathHouseV2 already initialized!");
        comptroller = Comptroller(_comptroller);
        admin = msg.sender;
        proxyAdmin = _pAdmin;

        initialized = true;
    }
```

## Recommendation
It is recommended to change the declarations of these variables to immutable to make the intent clear and reduce gas consumption. The `initialize` function can also be updated to initialize these variables as immutable.

