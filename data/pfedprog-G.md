# Gas Report

## [GAS-1]: Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L122 => for (uint256 i = 0; i < buddies.length; ++i) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L894 => payAmts.length == payGems.length &&
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L895 => payAmts.length == buyAmts.length &&
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L896 => payAmts.length == buyGems.length,
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899 => for (uint i = 0; i < payAmts.length; i++) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911 => for (uint i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924 => for (uint i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L31 => for (uint256 i = 0; i < bathTokensV1.length; ++i) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L37 => amountsWithFee = new uint256[](tokenAmounts.length);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L38 => fees = new uint256[](tokenAmounts.length);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L40 => for (uint256 i = 0; i < tokenAmounts.length; ++i) {

## [GAS-2]: Use != 0 instead of > 0 for Unsigned Integer Comparison

When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L277 => return offers[id].timestamp > 0;
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345 => if (makerFee() > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L521 => require(pay_amt > 0);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L523 => require(buy_amt > 0);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L581 => require(amount > 0);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L585 => if (makerFee() > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1037 => while (pay_amt > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1077 => while (buy_amt > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1133 => if (pay_amt > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1166 => if (buy_amt > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1209 => require(id > 0);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1226 => require(id > 0);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1289 => while (best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1325 => t_buy_amt > 0 &&
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1326 => t_pay_amt > 0 &&
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1410 => require(span[pay_gem][buy_gem] > 0);
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1452 => while (uid > 0 && uid != id) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L41 => require(bathBalance > 0, "migrate: ZERO AMOUNT");
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L180 => if (reward > 0) {

## [GAS-3]: Long Revert Strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L70 => "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L74 => "createBathToken: UNDERLYING == ADDRESS 0"
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L78 => "createBathToken: IMPLEMENTATION == ADDRESS 0"
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L376 => "\_offer.buy_gem.transferFrom(msg.sender, offer.recipient, spend) failed - check that you can pay the fee"
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L381 => "offer.pay_gem.transfer(msg.sender, quantity) failed"
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L720 => require(isActive(id), "Offer was deleted or taken, or never existed.");
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L726 => "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L65 => "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L105 => require(msg.sender == bathHouse, "You are not my beloved bath house!");
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L122 => require(friendshipStarted, "I have not started a bathToken friendship");
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L143 => require(friendshipStarted, "I have not started a bathToken friendship");
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L238 => "Previous rewards period must be complete before changing the duration for the new period"

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L70 => require(OK, "low-level call to the Rubicon failed");
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L86 => require(OK, "low-level call to the router failed");

## [GAS-4]: Math operations

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.

While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

For example division by 2 can be transformed with `uint256 b = a >> 1;`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L78 => z = add(mul(x, y), WAD / 2) / WAD;
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L82 => z = add(mul(x, y), RAY / 2) / RAY;
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L86 => z = add(mul(x, WAD), y / 2) / y;
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L90 => z = add(mul(x, RAY), y / 2) / y;

## [GAS-5]: Don't Initialize Variables with Default Value

Uninitialized variables are assigned with the types default value.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L122 => for (uint256 i = 0; i < buddies.length; ++i) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899 => for (uint i = 0; i < payAmts.length; i++) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911 => for (uint i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924 => for (uint i = 0; i < ids.length; i++) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L31 => for (uint256 i = 0; i < bathTokensV1.length; ++i) {
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L40 => for (uint256 i = 0; i < tokenAmounts.length; ++i) {

## [GAS-6]: Unused local variable(s)

State variables that are not used following deployment should be removed to save gas.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L298 => uint256 id = uint256(id\_);

## [GAS-7]: State variables that could be declared constant

State variables that are not updated following deployment should be declared constant to save gas.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L684

## [GAS-8]: Unused functions

The following functions are never used and should be removed:

DSMath.imax(int256,int256) (https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70-L72)
DSMath.imin(int256,int256) (https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66-L68)
DSMath.max(uint256,uint256) (https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62-L64)
RubiconMarket.\_buys(uint256,uint256) (https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1183-L1205)

## [GAS-9] Using bools for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41

```
function isAuthorized(address src) internal view returns (bool) {
    if (src == owner) {
        return true;
    } else {
        return false;
    }
}
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

```
modifier synchronized() {
    require(!locked);
    locked = true;
    _;
    locked = false;
}
```
