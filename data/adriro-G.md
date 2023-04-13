# `BathHouseV2` contract

- The condition `_underlying != address(0)` has been already checked by the caller to the `_bathify` function.  
  https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/BathHouseV2.sol#L144  

# `BathBuddy` contract

- As the contract is not upgradeable or cloneable, the `owner`, `myBathTokenBuddy` and `bathHouse` storage variables can be immutable. The `spawnBuddy` function should be changed to use the constructor, and the `friendshipStarted` variable can be removed.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L46-L49

- The contract uses OpenZeppelin SafeMath to implement safe arithmetic operations, this can be removed and changed to use Solidity 0.8 native checked math to save gas.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6

# `RubiconMarket` contract

- The contract uses the DSMath library to implement safe arithmetic operations, this can be removed and changed to use Solidity 0.8 native checked math to save gas.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45
  
- The `can_cancel` modifier calls twice the `getOwner` function accross its different implementations  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L610  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L719  

- The `synchronized` modifier can use uint256 with values 1/2 instead of bools to save gas.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269  

- The whole `OfferInfo` struct is copied into memory in the implementation of `getOffer`.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L291  

- The `makerFee()` function is called twice in the implementation of the `buy` function.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L346
  
- In the `buy` function, there is no need to call the `getRecipient` function, which will re-read the value from storage, as the value is directly available in the offer scope.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349
  
- In the `offer` function, the `OfferInfo` struct can be directly stored in storage instead of using first memory and then copying from memory to storage.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L527
  
- The `makerFee()` function is called twice in the implementation of the `calcAmountAfterFee` function.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L585  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586

- The `can_offer` modifier can be removed since the `isClosed` function will always return false.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L597
  
- The `_next_id` function reads the `last_offer_id` storage variable twice.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568

- The `batchOffer` and `batchRequote` functions should call the `offer` internally to execute a jump instead of a function call
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L900  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L926
  
- The contract has lots of code from previous version that are being used in the latest version. While the storage variable may be needed in order to not break the storage layout, all the associated functionality in code can be removed in order to save deployment gas costs.

# `FeeWrapper` contract

- Use `calldata` for array or structs coming from function parameters of external functions.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L29  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L49  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L61  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L77  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L109  

# `Position` contract

- The contract uses OpenZeppelin SafeMath to implement safe arithmetic operations, this can be removed and changed to use Solidity 0.8 native checked math to save gas.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6

- The following storage variables can be declared as immutable as they remain constant in the contract: `oracle`, `rubiconMarket`, `bathHouseV2`, `comptroller`.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44-L47
 
- In the function `_savePosition`, the `lastPositionId` storage variable is read 3 times. Consider caching its value in a local variable.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L401

- In the function `_savePosition`, the `Position` struct can be directly stored in storage instead of using first memory and then copying from memory to storage.  
  https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L408
