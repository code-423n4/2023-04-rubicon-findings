##### QA-01 : Typos 


Typo in the comments of batchOffer() function

/// @notice Batch offer functionality - multuple offers in a single transaction
fix the typo ( multuple -> multiple)

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L887




Typo in the require message in the createBathToken() function
fix the type ("createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY" -> "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALREADY")

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L70


##### QA-02 : Unused imports

SafeERC20 and CTokenInterfaces contracts are imported in V2Migrator.sol but never used 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L4

