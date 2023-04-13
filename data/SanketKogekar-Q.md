
### RubiconMarket.sol

1. In RubiconMarket.sol, the `isAuthorized` function is simple and can be condensed to a single line using the ternary operator. It would make the code concise and easier to read.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L35)
    ```
    function isAuthorized(address src) internal view returns (bool) {
        return (src == owner) ? true : false;
    }
    ```

2. In RubiconMarket.sol, `isActive(uint256 id)` and other functions that use the 'timestamp' property The purpose of the isActive() function is to check if the order is active by verifying if the `timestamp` property of the order is greater than zero.
    A better way would be to use a boolean property 'active' that is defaulted to false, and then set it to true when an order is created which would make the code more understandable & robust against unexpected issues with timestamps.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L277)

3. In RubiconMarket.sol, there is an incorrect return type in `getRecipient(uint256 id)` function. The return type of the `getRecipient` function is declared as `address owner`.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284)
    ```
    function getRecipient(uint256 id) public view returns (address recipient) {
        return offers[id].recipient;
    }
    ```

4. In RubiconMarket.sol - Using `assert` in place of `require` in the `synchronized()` modifier. Assert could be used since checked variable is not user input and no message is provided in response.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L265)
    ```
    modifier synchronized() {
        assert(!locked);
        locked = true;
        _;
        locked = false;
    }
    ```

5. In RubiconMarket.sol, the use of the virtual keyword before the buy function is not necessary unless this function is intended to be overridden in a derived contract. This is could lead to unintended state changes.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L317-L318)
    Same goes for several modifiers like `can_cancel()`, `can_offer()`, etc.

6. In RubiconMarket.sol, there is a `kill(bytes32 id)` function which is unused and could be overriden from child contract. This could result in unintended consequences.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L487)

7. In RubiconMarket.sol, the `isClose()` function is redundant because it always returns false. It has multiple references in the contract.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620-L621)

### V2Migrator.sol

8. In V2Migrator.sol, there is a possibility that funds get stuck  if the transfer function fails. The `safeTransfer()` fuction would be recommended.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59)

9. In V2Migrator.sol, the contract imports SafeERC20.sol, however, the contract uses standard ERC20 calls like `approve()`, `transferFrom()`, and `transfer()` which is not recommended for security.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L4)

10. In V2Migrator.sol, the constructor should check if the bathTokensV1 and bathTokensV2 arrays are of equal length. Needs input validation. If the mapping is incorrect or has not been set, the function could result in the loss of user funds.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L31)

### BathHouseV2.sol 

11. In BathHouseV2.sol, the proxyAdmin address is set in the initialize function and can't be changed afterwards. This could be problematic if the address gets compromised, as there's no way to change it. It's better to allow for the proxyAdmin to be changed by the contract owner.
    Also there is no check in the initialize function to ensure that the `_comptroller` and `_pAdmin` arguments are non-zero addresses.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L36-L37)

12. In BathHouseV2.sol, the `createBathToken()` function does not check if InterestRateModel is a valid contract. It could be possible for an attacker to pass in a malicious contract.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L62-L63)

13. In BathHouseV2.sol, the `createBathToken()` function allows for the initialExchangeRateMantissa to be set to zero (0), which could cause division-by-zero error.
    Same function also allows 'becomeImplementationData' to be set to an empty array, which could result in unexpected behavior.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L63-L64)

14. In BathHouseV2.sol, the `_bathify()` function does not ensure that the resulting name and symbol do not exceed the maximum length of 256 characters. If the resulting name or symbol exceeds this length, it could cause issues when interacting with the bath token.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L137)

15. In BathHouseV2.sol, the `getReward()` and `claimRewards()` functions do not perform any input validation to ensure that the 'buddy' and `rewardsToken` arguments are valid contracts.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L116) (1)
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L131) (2)

16. In BathHouseV2.sol, the `getReward()` and `claimRewards()` functions do not perform any input validation to ensure that the buddies and rewardsTokens arrays have the same length. If these arrays have different lengths, it could cause issues when trying to claim rewards.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L116) (1)
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L131) (2)

### BathBuddy.sol

17. In BathBuddy.sol, the `spawnBuddy()` function should have the onlyOwner() modifier to ensure that only the contract owner can call it. (*Could be medium-severity issue. Added here because I'm not sure if it qualifies for it)
    Also same function implies that it can only be called once.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71)
    ```
    require(!friendshipStarted, "I already have a buddy!")
    ```
    
18. In BathBuddy.sol, the onlyBuddy() modifier should have a check to ensure that myBathTokenBuddy is not address(0)
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94)

19. In BathBuddy.sol, the `earned()` function of the IBathBuddy interface can take IERC20 token parameter instead of an address to be explicit.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L19)

20. In BathBuddy.sol, the `rewardsDuration` can be set in the `spawnBuddy()` function to avoid unexpected issues.

21. Suggestion: In BathBuddy.sol, the contract can define onlyBathHouseOrOwner() modifier to allow the owner to perform certain actions in case needed.
    ```
    modifier onlyBathHouseOrOwner(uint256 _bathHouseId) {
        require(msg.sender == bathHouses[_bathHouseId].owner || msg.sender == bathHouses[_bathHouseId].bathHouseAddress, "Only the bath house or owner can perform this action");
        _;
    }
    ```

22. In BathBuddy.sol, there are no functions in the contract that allows the owner to withdraw any funds that are sent to it. This could lead to funds being trapped in the contract.

### Position.sol 

23. In Position.sol, `Position` is used as a struct name as well as the contract name. Its not allowed and can cause errors.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L33)

24. In Position.sol, the `borrowBalance()` is not marked as a `view` function, even though it does NOT modify any state. This can cause unexpected gas costs and should be marked as a view function.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L65)

25. In Position.sol, the contract does not validate the inputs to the `openPosition()`, `buyAllAmountWithLeverage()`, and `sellAllAmountWithLeverage()` functions, which can lead to unexpected behaviors.

26. In Position.sol, the `Ownable` contract is used as a parent contract for Position. Now `OwnableUpgradeable` could be used instead.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L15-L16)

27. In Position.sol, for `_supply()` function, we could add a `safeApprove` call with the 0 amount of tokens to revoke the approval after the transfer has been completed.

28. In Position.sol, the `_updateMargin()` and `_removePosition()` function does not perform any validation on the existence of the position, which could lead to unexpected behavior if the position ID is invalid.

29. In Position.sol, the `_rubiconSwap()` does not verify if `_fillLimit` parameter is greater than zero.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L443)

30. In Position.sol, I found no reference to the code which sets `Position` struct's `isActive` variable to `true` boolean value.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L39)

### FeeWrapper.sol

31. In FeeWrapper.sol, there is a potential reentrancy issue: The `_chargeFee()` function calls an external contract and then immediately approves tokens to that contract. If the external contract is a malicious contract that calls back into FeeWrapper before `_chargeFee()` finishes execution, it could lead to unexpected behavior. Re-entrancy guard could be added.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L93)

32. In FeeWrapper.sol, there is no error handling in the `_chargeFeePayable()` function for cases where the fee transfer fails. This could cause unexpected behavior if the transfer fails and the function does not revert.
    [Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L108)

33. In FeeWrapper.sol, there are no events emitted, which could make it difficult to track & debug issues or provide on-chain updates.