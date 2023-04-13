## Low Risk Issues

|             | Issue                                                                              | Instances |
| ----------- | :--------------------------------------------------------------------------------- | :-------: |
| [L-1](#L-1) | Missing checks for `address(0x0)` when assigning values to address state variables |    13     |

## [L-01] Missing checks for `address(0x0)` when assigning values to address state variables

There are 13 instances of this issue:

```solidity
File: /contracts/BathHouseV2.sol

34        comptroller = Comptroller(_comptroller);

36        proxyAdmin = _pAdmin;

54        buddy = bathTokenToBuddy[bathToken];
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

77        owner = _owner;

78        myBathTokenBuddy = newBud;

79        bathHouse = _bathHouse;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

55        oracle = PriceOracle(_oracle);

56        rubiconMarket = RubiconMarket(_rubiconMarket);

57        bathHouseV2 = BathHouseV2(_bathHouseV2);

131        address bathTokenAsset = bathHouseV2.getBathTokenFromAsset(asset);

132        address bathTokenQuote = bathHouseV2.getBathTokenFromAsset(quote);

281        address _bathTokenQuote = bathHouseV2.getBathTokenFromAsset(_quote);

383        address _bathTokenAsset = bathHouseV2.getBathTokenFromAsset(_asset);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## Non-Critical Issues

|               | Issue                                                                           | Instances |
| ------------- | :------------------------------------------------------------------------------ | :-------: |
| [NC-1](#NC-1) | Import declarations should import specific symbols                              |    25     |
| [NC-2](#NC-2) | Use named parameters for mapping type declarations                              |    15     |
| [NC-3](#NC-3) | Return values of approve() not checked                                          |     3     |
| [NC-4](#NC-4) | require()/revert() statements should have descriptive reason strings            |    22     |
| [NC-5](#NC-5) | public functions not called by the contract should be declared external instead |     8     |
| [NC-6](#NC-6) | Missing event for important parameter change                                    |     2     |
| [NC-7](#NC-7) | Contract files should define a locked compiler version                          |     2     |

## [N-01] Import declarations should import specific symbols

Prefer import declarations that specify the symbol(s) using the form import {SYMBOL} from "SomeContract.sol" rather than importing the whole file.

There are **25** instances of this issue.

```solidity
File: /contracts/RubiconMarket.sol

11      import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
12      import "@openzeppelin/contracts/utils/StorageSlot.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

4   import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
5   import "./compound-v2-fork/InterestRateModel.sol";
6   import "./compound-v2-fork/CErc20Delegator.sol";
7   import "./compound-v2-fork/Comptroller.sol";
8   import "./compound-v2-fork/Unitroller.sol";
9   import "./periphery/BathBuddy.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/V2Migrator.sol

4   import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5   import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
6   import "./compound-v2-fork/CTokenInterfaces.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

4   import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5   import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
6   import "@openzeppelin/contracts/utils/math/SafeMath.sol";
7   import "@openzeppelin/contracts/security/Pausable.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

4   import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5   import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
6   import "@openzeppelin/contracts/utils/math/SafeMath.sol";
7   import "@openzeppelin/contracts/access/Ownable.sol";
8   import "../../compound-v2-fork/Comptroller.sol";
9   import "../../compound-v2-fork/PriceOracle.sol";
10  import "../../BathHouseV2.sol";
11  import "../../RubiconMarket.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: /contracts/utilities/FeeWrapper.sol

4   import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5   import "./RubiconRouter.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

## [N-02] Use named parameters for mapping type declarations

There are **15** instances of this issue.

Consider using named parameters in mappings (e.g. mapping(address account => uint256 balance)) to improve readability. This feature is present since Solidity 0.8.18.

```solidity
File: /contracts/RubiconMarket.sol

222    mapping(uint256 => OfferInfo) public offers;

691    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695    mapping(uint256 => uint256) public _near; //next unsorted offer id
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

20    mapping(address => address) private tokenToBathToken;
21    mapping(address => address) private bathTokenToBuddy;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/V2Migrator.sol

19    mapping(address => address) public v1ToV2Pools;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

52    mapping(address => uint256) public periodFinish; // Token specific
53    mapping(address => uint256) public rewardRates; // Token specific reward rates
54    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55    mapping(address => uint256) public lastUpdateTime; //Token specific
56    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

42    mapping(uint256 => Position) public positions;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [N-03] Return values of approve() not checked

Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

There are **3** instances of this issue.

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

465        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

500        IERC20(_asset).approve(
            address(rubiconMarket),
            IERC20(_asset).balanceOf(address(this))
        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: /contracts/utilities/FeeWrapper.sol

105        IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

## [N-04]require()/revert() statements should have descriptive reason strings

There are **22** instances of this issue.

```solidity
File: /contracts/RubiconMarket.sol

246        require(isActive(id));
252        require(isActive(id));
604        require(isActive(id));
611        require(isActive(id));
1366        require(isActive(id));

265        require(!locked);

938        require(!locked);

1034        require(!locked);

1075        require(!locked);

461            : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

488        require(cancel(uint256(id)));

757        require(cancel(uint256(id)));

519        require(uint128(pay_amt) == pay_amt);

520        require(uint128(buy_amt) == buy_amt);
521        require(pay_amt > 0);
522        require(pay_gem != ERC20(address(0)));
523        require(buy_amt > 0);
524        require(buy_gem != ERC20(address(0)));
525        require(pay_gem != buy_gem);

598        require(!isClosed());
605        require(!isClosed());

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [N-05] PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD

public functions not called in the contract itself should be declared externals.

### Details

There are **8** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

290    ) public view returns (uint256, ERC20, uint256, ERC20) {

574    function getFeeBPS() public view returns (uint256) {

624    function getTime() public view returns (uint64) {

1001    ) public view returns (uint256) {

1010    function getFirstUnsortedOffer() public view returns (uint256) {

1016    function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1153    ) public view returns (uint256 fill_amt) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

47    ) public view returns (address) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

## [N-06] 12. Missing event for critical parameter change

There are 2 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

1466    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }


1476    function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [N-07] Contract files should define a locked compiler version

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Instances (2):

```solidity
File: /contracts/RubiconMarket.sol

2  pragma solidity ^0.8.9;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

2   pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol
