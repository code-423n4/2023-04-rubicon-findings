## In MigratorV2 `constructor` inputs are not validated correctly
The constructor does not validate the input length of **`bathTokensV1`** and **`bathTokensV2`** arrays. This could lead to a situation where an incomplete mapping is created if the lengths are different. A check can be added to ensure that both arrays have the same length:

```solidity
require(bathTokensV1.length == bathTokensV2.length, "Array lengths do not match");
```

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L31](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L31)

## SafeERC20 is imported ,but not used in MigratorV2 smart contract
SafeERC20 is imported ,but not used in MigratorV2 contract. SafeERC20 should be used instead of `transferFrom` `safeTransferFrom` should be used and instead of `approve` `safeApprove` should be used to protect from unexpected behaviour which can occur because ERC20 does not revert on failure.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L4
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L44
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59