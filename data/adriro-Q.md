# Report


## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Import declarations should import specific symbols | 25 |
| [NC-2](#NC-2) | Use named parameters for mapping type declarations | 21 |
| [NC-3](#NC-3) | Use `uint256` instead of the `uint` alias | 10 |
### <a name="NC-1"></a>[NC-1] Import declarations should import specific symbols
Prefer import declarations that specify the symbol(s) using the form `import {SYMBOL} from "SomeContract.sol"` rather than importing the whole file

*Instances (25)*:
```solidity
File: contracts/BathHouseV2.sol

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "./compound-v2-fork/InterestRateModel.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

7: import "./compound-v2-fork/Comptroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

9: import "./periphery/BathBuddy.sol";

```

```solidity
File: contracts/RubiconMarket.sol

11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

```

```solidity
File: contracts/V2Migrator.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

```

```solidity
File: contracts/periphery/BathBuddy.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

7: import "@openzeppelin/contracts/security/Pausable.sol";

```

```solidity
File: contracts/utilities/FeeWrapper.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import "./RubiconRouter.sol";

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

7: import "@openzeppelin/contracts/access/Ownable.sol";

8: import "../../compound-v2-fork/Comptroller.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

10: import "../../BathHouseV2.sol";

11: import "../../RubiconMarket.sol";

```

### <a name="NC-2"></a>[NC-2] Use named parameters for mapping type declarations
Consider using named parameters in mappings (e.g. `mapping(address account => uint256 balance)`) to improve readability. This feature is present since Solidity 0.8.18

*Instances (21)*:
```solidity
File: contracts/BathHouseV2.sol

20:     mapping(address => address) private tokenToBathToken;

21:     mapping(address => address) private bathTokenToBuddy;

```

```solidity
File: contracts/RubiconMarket.sol

224:     mapping(uint256 => OfferInfo) public offers;

723:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

724:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

724:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

725:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

725:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

726:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers

727:     mapping(uint256 => uint256) public _near; //next unsorted offer id

```

```solidity
File: contracts/V2Migrator.sol

19:     mapping(address => address) public v1ToV2Pools;

```

```solidity
File: contracts/periphery/BathBuddy.sol

54:     mapping(address => uint256) public periodFinish; // Token specific

55:     mapping(address => uint256) public rewardRates; // Token specific reward rates

56:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

57:     mapping(address => uint256) public lastUpdateTime; //Token specific

58:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific

61:     mapping(address => mapping(address => uint256))

61:     mapping(address => mapping(address => uint256))

63:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC

63:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

43:     mapping(uint256 => Position) public positions;

```

### <a name="NC-3"></a>[NC-3] Use `uint256` instead of the `uint` alias
Prefer using the `uint256` type definition over its `uint` alias.

*Instances (10)*:
```solidity
File: contracts/RubiconMarket.sol

819:         uint pos, //position to insert offer, 0 should be used if unknown

924:         uint[] calldata payAmts,

926:         uint[] calldata buyAmts,

935:         for (uint i = 0; i < payAmts.length; i++) {

948:     function batchCancel(uint[] calldata ids) external {

949:         for (uint i = 0; i < ids.length; i++) {

957:         uint[] calldata ids,

958:         uint[] calldata payAmts,

960:         uint[] calldata buyAmts,

964:         for (uint i = 0; i < ids.length; i++) {

```


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Contract files should define a locked compiler version | 2 |
| [L-2](#L-2) | ERC20 functions `name`, `symbol` and `decimals` are optional and may not be implemented | 2 |
| [L-3](#L-3) | Validate parameters in `rubicall` function of FeeWrapper contract | - |
| [L-4](#L-4) | Zero amount ERC20 token transfers may fail some implementations | - |
| [L-5](#L-5) | Reward duration can be set to zero in BathBuddy contract | - |
| [L-6](#L-6) | `batchRequote` function in RubiconMarket contract should validate that arrays have equal lengths | - |


### <a name="L-1"></a>[L-1] Contract files should define a locked compiler version
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

*Instances (2)*:
```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```

### <a name="L-2"></a>[L-2] ERC20 functions `name`, `symbol` and `decimals` are optional and may not be implemented

Even though these functions are usually implemented, they are optional in the standard and may not be available. 

*Instances (2)*:

- https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/BathHouseV2.sol#L146
- https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/BathHouseV2.sol#L148


### <a name="L-3"></a>[L-3] Validate parameters in `rubicall` function of FeeWrapper contract

The function should validate that `feeAmount <= totalAmount` and that `feeTo != address(0)`.

### <a name="L-4"></a>[L-4] Zero amount ERC20 token transfers may fail some implementations

- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L340
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L351-L355

In both of these cases, the calculated fee amount may end up being zero due to rounding. Some ERC20 token implementations revert the operation when transferring a zero amount, which will end up reverting the transaction.

In all of these cases, the implementation should check that the transferred value is greater than zero before calling the transfer function.

### <a name="L-5"></a>[L-5] Reward duration can be set to zero in BathBuddy contract

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232-L242

The `setRewardsDuration` doesn't validate that duration is greater than zero. A zero value for the duration variable will cause a division by zero error in the `notifyRewardAmount` function.

### <a name="L-6"></a>[L-6] `batchRequote` function in RubiconMarket contract should validate that arrays have equal lengths

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917

The `batchRequote` function in RubiconMarket contract should validate that arrays coming from function parameters have equal lengths.
