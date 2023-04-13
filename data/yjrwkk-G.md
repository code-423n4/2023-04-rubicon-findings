### [G-01] Long revert strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.  
If the contract(s) in scope allow using Solidity >=0.8.4, consider using Custom Errors as they are more gas efficient while allowing developers to describe the error in detail using NatSpec.  

[contracts/RubiconMarket.sol#L720](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L720)  
```
require(isActive(id), "Offer was deleted or taken, or never existed.");
```
[contracts/periphery/BathBuddy.sol#L105](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L105)  
[contracts/periphery/BathBuddy.sol#L122](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L122)  
[contracts/periphery/BathBuddy.sol#L143](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L143)  
[contracts/utilities/poolsUtility/Position.sol#L76](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L76)  

### [G-02] Multiple address mappings can be combined into a single mapping of an address to a struct

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined.  
Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.  
Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.  

[contracts/RubiconMarket.sol#L692-L693](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L692-L693)  
```
mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
```
[contracts/periphery/BathBuddy.sol#L59-L61](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L59-L61)  

### [G-03] Use shift right/left instead of division/multiplication if possible

A division/multiplication by any number x being a power of 2 can be calculated by shifting log2(x) to the right/left.  
While the DIV opcode uses 5 gas, the SHR opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.  

[contracts/RubiconMarket.sol#L78](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L78)  
```
z = add(mul(x, y), WAD / 2) / WAD;
```
[contracts/RubiconMarket.sol#L82](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L82)  
[contracts/RubiconMarket.sol#L86](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L86)  
[contracts/RubiconMarket.sol#L90](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L90)  

### [G-04] Cache array length outside of loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.  

[contracts/BathHouseV2.sol#L122](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L122)  
```
for (uint256 i = 0; i < buddies.length; ++i) {
```
[contracts/RubiconMarket.sol#L894](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L894)  
[contracts/RubiconMarket.sol#L895](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L895)  
[contracts/RubiconMarket.sol#L896](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L896)  
[contracts/RubiconMarket.sol#L897](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L897)  
[contracts/RubiconMarket.sol#L899](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L899)  
[contracts/RubiconMarket.sol#L911](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L911)  
[contracts/RubiconMarket.sol#L924](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L924)  
[contracts/V2Migrator.sol#L31](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L31)  
