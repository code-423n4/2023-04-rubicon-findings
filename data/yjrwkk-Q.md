### [L-01] Events associated with setter functions

Consider having events associated with setter functions emit both the new and old values instead of just the new value.  

[contracts/RubiconMarket.sol#L960](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L960)  
```
emit LogMinSell(address(pay_gem), dust);
```
[contracts/RubiconMarket.sol#L27](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L27)  
[contracts/periphery/BathBuddy.sol#L241](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L241)  

### [L-02] Unspecific compiler version pragma

Avoid floating pragmas for non-library contracts.  
It is recommended to pin to a concrete compiler version.  

[contracts/RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L2)  
```
pragma solidity ^0.8.9;
```
[contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L2)  

### [L-03] Do not use deprecated library functions

The usage of deprecated library functions should be discouraged.  
This issue is mostly related to OpenZeppelin libraries.  

[contracts/utilities/poolsUtility/Position.sol#L358](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L358)  
```
IERC20(_token).safeApprove(_bathToken, _amount);
```

### [L-04] Lines too long

Keep line width to max 120 characters for better readability where possible.  

[contracts/RubiconMarket.sol#L9](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L9)  
```
/// @notice This contract is a derivative work, and spiritual continuation, of the open-source work from Oasis DEX: https://github.com/OasisDEX/oasis
```
[contracts/RubiconMarket.sol#L250](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L250)  
[contracts/RubiconMarket.sol#L312](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L312)  
[contracts/RubiconMarket.sol#L348](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L348)  
[contracts/RubiconMarket.sol#L522](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L522)  
[contracts/RubiconMarket.sol#L524](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L524)  
[contracts/RubiconMarket.sol#L693](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L693)  
[contracts/RubiconMarket.sol#L726](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L726)  
[contracts/periphery/BathBuddy.sol#L28](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L28)  
[contracts/periphery/BathBuddy.sol#L31](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L31)  
[contracts/periphery/BathBuddy.sol#L35](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L35)  
[contracts/periphery/BathBuddy.sol#L36](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L36)  
[contracts/periphery/BathBuddy.sol#L66](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L66)  
[contracts/periphery/BathBuddy.sol#L166](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L166)  
[contracts/periphery/BathBuddy.sol#L167](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L167)  

### [L-05] Not completely using OpenZeppelin contracts

OpenZeppelin maintains a library of standard, audited, community-reviewed, and battle-tested smart contracts. Instead of always importing these contracts, the protocol project re-implements them in some cases. This increases the amount of code that the protocol team will have to maintain and miss all the improvements and bug fixes that the OpenZeppelin team is constantly implementing with the help of the community.  
Consider importing the OpenZeppelin contracts instead of re-implementing or copying them. These contracts can be extended to add the extra functionalities required if need be.  

[contracts/periphery/BathBuddy.sol#L87](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L87)  
```
modifier onlyOwner() {
```

### [L-06] Use a more recent version of solidity


[contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L2)  
```
pragma solidity ^0.8.0;
```
[contracts/BathHouseV2.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L2)  
[contracts/RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L2)  
[contracts/V2Migrator.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L2)  
[contracts/utilities/poolsUtility/Position.sol#L2](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L2)  

### [L-07] Use a single version of solidity across contracts

At the moment, files in the scope use three different versions of solidity: `^0.8.0`, `^0.8.9` and `0.8.17`.
