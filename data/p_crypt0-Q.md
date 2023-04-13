# [Low] BathHouseV2 - createBathToken()

## function createBathToken() needs extra validation checks to ensure token is not rogue.

If the `initialExchangeRateMantissa` is set to 0, the token will have no value and until changed through open market, will be worthless. This could mean that a rogue actor gains all the tokens for free.

```solidity
//============================= BATH TOKENS =============================

    /// @notice create new CErc20 based bathToken
    function createBathToken(
        address underlying,
        InterestRateModel interestRateModel, //@audit What is this?
        uint256 initialExchangeRateMantissa, //@audit What happens if this is set to 0?
        address implementation,
        bytes memory becomeImplementationData //@audit what is this data? How do we validate it?
    ) external onlyAdmin {
        // underlying can be used only for one bathToken
        require(
            tokenToBathToken[underlying] == address(0),
            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
        );
        require(
            underlying != address(0),
            "createBathToken: UNDERLYING == ADDRESS 0"
        );
        require(
            implementation != address(0),
            "createBathToken: IMPLEMENTATION == ADDRESS 0"
        );

        //@audit Should require that the adminProxy !=0

        // get bathToken metadata that semantically reflects underlying ERC20
        (string memory name, string memory symbol, uint8 decimals) = _bathify(
            underlying
        );

        // BathTokenDelegator
        address bathToken = address(
            new CErc20Delegator(
                underlying,
                comptroller,
                interestRateModel,
                initialExchangeRateMantissa,
                name,
                symbol,
                decimals,
                payable(proxyAdmin),
                implementation,
                becomeImplementationData
            )
        );

        // spawn buddy
        BathBuddy buddy = new BathBuddy();
        buddy.spawnBuddy(admin, bathToken, address(this));

        tokenToBathToken[underlying] = bathToken;
        bathTokenToBuddy[bathToken] = address(buddy);

        emit BathTokenCreated(bathToken, underlying);
        emit BuddySpawned(bathToken, address(buddy));
    }
```

## Recommendation
Use validation checks to ensure that the `initialExchangeRateMantissa` is greater than 0, so as to avoid users purchasing tokens for free.