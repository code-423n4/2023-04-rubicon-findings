**BathHouseV2.sol**
- L8 - Unitroller is imported, but it is never used, therefore the import should be eliminated, since it generates an unnecessary gas expense.

- L26/66 - The onlyAdmin modifier is created, but it is only used once, therefore it could be directly used in the function that is needed.

- L72/82/144 - The same value is validated twice, this is unnecessary since the internal _bathify() function is only used once in another createBathToken() function that also validates the same thing. The correct thing would be to eliminate the validation of the L144.

- L70/74/78 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**V2Migrator.sol**
- L4 - SafeERC20 is imported, but it is never used, therefore the import should be eliminated, since it generates an unnecessary gas expense.

- L65 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**utilities/FeeWrapper.sol**
- L5 - RubiconRouter is imported, but it is never used, therefore the import should be eliminated, since it generates an unnecessary gas expense.

- L102/112/114 - If the variables are only going to be used once, it is not necessary to create the variable, the operation can be used directly where it is necessary.

- L70/86 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS


**periphery/BathBuddy.sol**
- L105/122/143/238 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

- L263/266 - Withdrawn and Recovered events are created but are never used, therefore they should be removed, as it generates unnecessary gas waste.


**utilities/poolsUtility/Position.sol**
- L76/102/116/290/593/597 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS

- L267/272 - An internal function _borrow() is created which is only a require validation that is only used by one function, therefore the least expensive thing would be to directly use the require in the function. This is better that way, since better readability is not gained by separating the require into another function.


**RubiconMarket.sol**
- L260 - The can_offer modifier does not validate anything, this generates an unnecessary extra expense

- L894/895/896/899 - The uint payAmts.length is used multiple times, therefore a variable should be created in memory to avoid having to consult it so many times.

- L374/379/720/726 - REQUIRE()/REVERT() STRINGS LONGER THAN 32 BYTES COST EXTRA GAS
