## BathHouseV2

Use `msg.sender` instead of `admin` at L104 to avoid the storage read since `msg.sender` is always going to be `admin` because of the `onlyAdmin` modifier in `createBathToken`

## BathBuddy

1- Require new buddy in `spawnBuddy` to be a non-zero address (`require(newBud != address(0))`) and remove the `msg.sender != address(0) && friendshipStarted` checks from the `onlyBuddy` modifier as they are redundant. Otherwise, since the `onlyBuddy` modifier is not used, it can be removed altogether.
2- Call `IERC20(myBathTokenBuddy).totalSupply()` only once in `rewardPerToken`, store the result in memory and reuse elsewhere in the function.
3- Since [`BathBuddy` is not meant to be an upgradeable contact](https://discord.com/channels/810916927919620096/1092789958923784292/1093918395478581278) `spawnBuddy` can be changed to be the actual `constructor` of `BathBuddy` and all storage variables set in it (`owner`, `myBathTokenBuddy`, `bathHouse`, and `friendshipStarted`) can be changed to `immutable` to avoid storage reads thoughought the contract for them.

## Position

1- All variables set in the constructor (`oracle`, `rubiconMarket`, `bathHouseV2`, and `comptroller`) can be set to `immutable`
2- Change `Position memory pos` in L75 to `Position storage pos` to avoid the exta storage reads of `pos.asset` and `pos.bathTokenAmount` that do not need to be read in `borrowBalanceOfPos()`