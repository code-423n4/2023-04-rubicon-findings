#QA
## General: 
- There is a lot of commented out code and TODOs
- Including multiple contracts in the same file makes navigating difficult, especially for core functionality (e.g. `RubiconMarket.sol` has the whole inheritance tree)
- There is some minor organizational inconsistency within contracts - e.g. `RubiconMarket#_matcho()` is in middle of the sorting logic
- BathBuddy.sol#onlyBuddy() modifier is never used
- Note that MakerDAO has extensive foundry tests around the OasisDex. These could be leveraged, as testing around the core marketplace is light (and a few findings were related to marketplace corner-cases)

## Typos/Comments:
- Position.sol incorrect natspec: `/// @title PoolsUtility`
- Typo in BathHouseV2.sol - `"createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"`
- Typo in FeeWrapper.sol - `"interatcions"`