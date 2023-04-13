## 1. LOW - Keeper called function del_rank has no associated modifier:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L937

## 2. LOW- Check for the underlying tokens in migrator contract constructor 
In migrator contract v1 and corresponding v2 addresses are set but there is no check if they have same underlying token and are not being mismatched at:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30

Add the following check:
```solodity
    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

        // @audit cache the length variable
        for (uint256 i = 0; i < bathTokensV1.length; ++i) {
            // set v1 to v2 bathTokens
            require(IERC29(bathTokensV1[i].underlyingToken()) == bathTokensV2[i].underlying()); 
            require(address(IBathToken(bathTokensV1[i]).underlyingToken()) == address(CErc20(bathTokensV2[i]).underlying()));
            v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
        }
    }

```

## 3. NC - No zero checks are places at multiple places at multiple contracts including `rubiconMarket` `PoolsUtility`, `BathBuddy` and `Position` contracts.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71


## 4. LOW- No zero checks for the owner and recipient at multiple places:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L511
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L802
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1273

## 5. NC- Event not indexed in `Position` Contract
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L50-L52

## 6. LOW - feeBPS setter function should have necessary upper and lower bounds, if fee set over 100% it can lead to loss of funds:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L574

## 7. NC - All the following setters should have necessary check, should not be like this are vulnerable to operator mistake:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1484

## 8. NC - In `BathBuddy` the modifier `onlyBuddy` is never used, remove it:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94-L102

## 9. NC - Use of floating pragma, used a fixed version instead of floating it can lead to different files compiling in different versions:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2

## 10. NC - block.timestamp is casted into uint64 that means that this contract can only work till some year in 2100's and after that this casting will become unusable thus making contract unusable:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L534

## 11. NC - Remove the commented code that is not being used at multiple places in multiple files.

 


