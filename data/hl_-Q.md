# Table of contents

- [N-01] Insufficient test coverage
- [N-02] Import declarations should import specific identifiers, rather than the whole file
- [N-03] Use a more recent version of Solidity
- [N-04] Non-library/interface files should use fixed compiler versions, not floating ones
- [N-05] Incorrect require() reason string
- [N-06] NatSpec comments should be increased in contracts e.g. `FeeWrapper.sol`
- [N-07] Confusing variable name
- [L-01] Include checks to ensure fee inputs are valid
- [L-02] Limited error handling for `claimRewards` function
- [L-03] Truncation of decimals may result in no fees paid
- [L-04] Function `isClosed()` is irrelevant
- [L-05] Include check to ensure that `bathToken` is listed

## [N-01] Insufficient test coverage
The test coverage rate of the project is 60%. Testing all functions is best practice in terms of security criteria.

While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.

## [N-02] Import declarations should import specific identifiers, rather than the whole file

Using import declarations of the form `import {<identifier_name>} from "some/file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation.

For example:

```
File:  2023-04-rubicon/contracts/BathHouseV2.sol 

4:    import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
5:    import "./compound-v2-fork/InterestRateModel.sol";
6:    import "./compound-v2-fork/CErc20Delegator.sol";
7:    import "./compound-v2-fork/Comptroller.sol";
8:    import "./compound-v2-fork/Unitroller.sol";
9:    import "./periphery/BathBuddy.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L4-L9

## [N-03] Use a more recent version of Solidity

Old version of solidity is used, consider using the new one 0.8.17.
You can see what new versions offer regarding bug fixed [here](https://github.com/ethereum/solidity/blob/develop/Changelog.md).

For example:

```
File: 2023-04-rubicon/contracts/RubiconMarket.sol 
2:    pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2

## [N-04] Non-library/interface files should use fixed compiler versions, not floating ones

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

For example:

```
File: 2023-04-rubicon/contracts/RubiconMarket.sol 
2:    pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2

## [N-05] Incorrect require() reason string

For the second require() statement of the `can_cancel` modifier, consider changing the word "and" to "or" in the reason string to match the code. 

i.e. "Offer can not be cancelled because user is not owner, **or** market is open, **or** offer sells required amount of tokens."

```
File: 2023-04-rubicon/contracts/RubiconMarket.sol#L718-729

 // // After close, anyone can cancel an offer
    modifier can_cancel(uint256 id) override {
        require(isActive(id), "Offer was deleted or taken, or never existed.");
        require(
            isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );
        _;
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L718-L729

## [N-06] NatSpec comments should be increased in contracts e.g. `FeeWrapper.sol`

In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the [Solidity official documentation](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html).

## [N-07] Confusing variable name

In function `calculateFee` of `FeeWrapper.sol`, the calculation for `amountsWithFee` is derived by subtracting the `fee` from the `tokenAmounts`. 

Consider changing the name `amountsWithFee` to `amountsWithFeeDeducted` for clarity. 

```
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol#L28-44

function calculateFee(
        uint256[] memory tokenAmounts,
        uint256 feeType,
        uint256 feeValue
    )
        external
        view
        returns (uint256[] memory amountsWithFee, uint256[] memory fees)
    {
        amountsWithFee = new uint256[](tokenAmounts.length);
        fees = new uint256[](tokenAmounts.length);

        for (uint256 i = 0; i < tokenAmounts.length; ++i) {
            fees[i] = (tokenAmounts[i] * feeValue) / feeType;
            amountsWithFee[i] = tokenAmounts[i] - fees[i];
        }
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L28-L44

## [L-01] Include checks to ensure fee inputs are valid

In function `calculateFee` of `FeeWrapper.sol`, include checks to ensure inputs like `feeType` and `feeValue` are valid.

For example, to mitigate the following scenarios: 

- Division by zero
- Integer overflow or underflow
- Precision Loss

```
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol#L28-44

 function calculateFee(
        uint256[] memory tokenAmounts,
        uint256 feeType,
        uint256 feeValue
    )
        external
        view
        returns (uint256[] memory amountsWithFee, uint256[] memory fees)
    {
        amountsWithFee = new uint256[](tokenAmounts.length);
        fees = new uint256[](tokenAmounts.length);

        for (uint256 i = 0; i < tokenAmounts.length; ++i) {
            fees[i] = (tokenAmounts[i] * feeValue) / feeType;
            amountsWithFee[i] = tokenAmounts[i] - fees[i];
        }
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L28-L44

## [L-02] Limited error handling for `claimRewards` function

There are limited error handling mechanisms for the `claimRewards` function in `BathHouseV2.sol`. If any of the external calls to the `comptroller` contract or `BathBuddy` contracts fail, it may result in the entire function failing and potentially leaving the rewards claiming process incomplete or leaving the contract in an inconsistent state. 

Consider proper error handling mechanisms (e.g. try-catch).

```
File: 2023-04-rubicon/contracts/BathHouseV2.sol#L115-128

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
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L128

## [L-03] Truncation of decimals may result in no fees paid

Token with low decimal places could avoid paying fees. In function `calcAmountAfterFee` of `RubiconMarket.sol`, the amount is multiplied by feeBPS then divided by the denominator of 100,000. 

As a result, the fee can be avoided when transacting with tokens that has low or zero decimals.

```
File:  2023-04-rubicon/contracts/RubiconMarket.sol#L578-588

function calcAmountAfterFee(
        uint256 amount
    ) public view returns (uint256 _amount) {
        require(amount > 0);
        _amount = amount;
        _amount -= mul(amount, feeBPS) / 100_000;

        if (makerFee() > 0) {
            _amount -= mul(amount, makerFee()) / 100_000;
        }
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L578-L588

## [L-04] Function `isClosed()` is irrelevant

Function `isClosed` of `RubiconMarket.sol` will always return false. This makes checks to this function redundant i.e. waste gas. Consider removing it. 

```
File: 2023-04-rubicon/contracts/RubiconMarket.sol#L620-622

function isClosed() public pure returns (bool closed) {
        return false;
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620-L622

## [L-05] Include check to ensure that `bathToken` is listed

Function `_borrowLimit` of `Position.sol` does not check if `bathToken` is listed. While 3 values are returned when calling `comptroller.markets` (isListed, collateralFactorMantissa, isComped), `_borrowLimit` only returns 1 value. 

It is recommended to check whether the bathToken is listed. 

```
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol#L532

  (, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L532