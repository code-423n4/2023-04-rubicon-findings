# Low Risk Summary
| |Issue|Instances|
|:-:|:-|:-:|
|[L-01]|Use `safeTransfer` instead of `transfer` for token transfers|4|
|[L-02]|Use two-step ownership transfers|2|
|[L-03]|Downcasting `uint` or `int` may result in overflow|12|
|[L-04]|`initialize` can be frontrun|2|
|[L-05]|Use timelock for critical parameter changes|3|

Total issues: 5

Total instances: 23

&nbsp;
# Non-Critical Summary
| |Issue|Instances|
|:-:|:-|:-:|
|[N-01]|Use `indexed` for event parameters|17|
|[N-02]|Use fixed compiler version|2|
|[N-03]|Use appropriate function naming convention|13|
|[N-04]|Lines too long|1|
|[N-05]|Update import usages|25|
|[N-06]|Implementing `renounceOwnership` is dangerous|1|
|[N-07]|Remove/Resolve open TODOs|3|
|[N-08]|Remove unused imports|3|
|[N-09]|Missing `address(0)` checks in constructor/initialize|2|
|[N-10]|Remove deprecated/redundant variables|4|
|[N-11]|Remove commented out code snippets|11|
|[N-12]|Remove useless function|1|

Total issues: 12

Total instances: 83

&nbsp;
# Low Risk Issues
## [L-01] Use `safeTransfer` instead of `transfer` for token transfers

`SafeERC20` is a wrapper around ERC20 operations that throw on failure (when the token contract returns false). This prevents calls executing if a token transfer is unsuccessful. Simply add `using SafeERC20 for ERC20`.

Instances: 4
```solidity
File: contracts/V2Migrator.sol

59:         IERC20(bathTokenV2).transfer(

```
- [contracts/V2Migrator.sol#L59](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L59)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

493:             IERC20(_asset).transferFrom(

```
- [contracts/utilities/poolsUtility/Position.sol#L493](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L493)

```solidity
File: contracts/utilities/FeeWrapper.sol

100:         IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

102:         IERC20(_feeToken).transfer(_feeTo, _feeAmount);

```
- [contracts/utilities/FeeWrapper.sol#L100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L100)
- [contracts/utilities/FeeWrapper.sol#L102](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102)

&nbsp;
## [L-02] Use two-step ownership transfers

The current ownership transfer process involves the current owner calling `transferOwnership()`. This function checks the new owner is not the zero address and proceeds to write the new owner's address into the owner's state variable.

If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier.

Consider implementing a two step process where the owner nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to fully succeed.

This can be achieved using OpenZeppelin's `Ownable2Step` or `Ownable2StepUpgradeable`.

Instances: 2
```solidity
File: contracts/RubiconMarket.sol

25:     function setOwner(address owner_) external auth {

```
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25

```solidity
File: contracts/utilities/poolsUtility/Position.sol

15: contract Position is Ownable, DSMath {

```
- [contracts/utilities/poolsUtility/Position.sol#L15](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L15)

&nbsp;
## [L-03] Downcasting `uint` or `int` may result in overflow

Consider using OpenZeppelin's `SafeCast` library to prevent unexpected overflows.

Instances: 12
```solidity
File: contracts/RubiconMarket.sol

321:         require(uint128(spend) == spend, "spend is not an int");

322:         require(uint128(quantity) == quantity, "quantity is not an int");

405:             uint128(quantity),

406:             uint128(spend)

519:         require(uint128(pay_amt) == pay_amt);

520:         require(uint128(buy_amt) == buy_amt);

559:             uint128(pay_amt),

560:             uint128(buy_amt)

1053:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1061:                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

1093:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1103:                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

```
- [contracts/RubiconMarket.sol#L321](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L321)
- [contracts/RubiconMarket.sol#L322](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L322)
- [contracts/RubiconMarket.sol#L405](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L405)
- [contracts/RubiconMarket.sol#L406](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L406)
- [contracts/RubiconMarket.sol#L519](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L519)
- [contracts/RubiconMarket.sol#L520](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L520)
- [contracts/RubiconMarket.sol#L559](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L559)
- [contracts/RubiconMarket.sol#L560](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L560)
- [contracts/RubiconMarket.sol#L1053](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1053)
- [contracts/RubiconMarket.sol#L1061](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1061)
- [contracts/RubiconMarket.sol#L1093](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1093)
- [contracts/RubiconMarket.sol#L1103](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1103)

&nbsp;
## [L-04] `initialize` can be frontrun
Without a check on `msg.sender`, `initialize` can be frontrun by a malicious account and ownership of the contract can be stolen.

Instances: 2
```solidity
File: contracts/RubiconMarket.sol

700:    function initialize(address _feeTo) public {
```
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700

```solidity
File: contracts/BathHouseV2.sol

32:     function initialize(address _comptroller, address _pAdmin) external {
```
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32


&nbsp;
## [L-05] Use timelock/events for critical parameter changes

Admin functions that change critical contract parameters/addresses/state should emit events and consider adding timelocks so that users and other privileged roles can detect upcoming changes (by offchain monitoring of events) and have the time to react to them.

Instances: 3
```solidity
File: contracts/RubiconMarket.sol

1466:    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:    function setFeeTo(address newFeeTo) external auth returns (bool) {
```
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476

&nbsp;

&nbsp;

# Non-Critical Issues
## [N-01] Use `indexed` for event parameters

Index event fields make the field more quickly accessible to off-chain tools that parse events. 

If the variable is value type (uint, address, bool), then using `indexed` saves gas. Otherwise, each index field costs extra gas during emission.

Instances: 17
```solidity
File: contracts/RubiconMarket.sol

97:     event LogItemUpdate(uint256 id);

662:     event LogMinSell(address pay_gem, uint256 min_amount);

664:     event LogUnsortedOffer(uint256 id);

665:     event LogSortedOffer(uint256 id);

667:     event LogDelete(address keeper, uint256 id);

668:     event LogMatch(uint256 id, uint256 amount);

```
- [contracts/RubiconMarket.sol#L97](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L97)
- [contracts/RubiconMarket.sol#L662](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L662)
- [contracts/RubiconMarket.sol#L664](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L664)
- [contracts/RubiconMarket.sol#L665](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L665)
- [contracts/RubiconMarket.sol#L667](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L667)
- [contracts/RubiconMarket.sol#L668](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L668)

```solidity
File: contracts/BathHouseV2.sol

23:     event BathTokenCreated(address bathToken, address underlying);

24:     event BuddySpawned(address bathToken, address bathBuddy);

```
- [contracts/BathHouseV2.sol#L23](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L23)
- [contracts/BathHouseV2.sol#L24](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L24)

```solidity
File: contracts/periphery/BathBuddy.sol

261:     event RewardAdded(uint256 reward);

262:     event Staked(address indexed user, uint256 amount);

263:     event Withdrawn(address indexed user, uint256 amount);

264:     event RewardPaid(address indexed user, uint256 reward);

265:     event RewardsDurationUpdated(uint256 newDuration);

266:     event Recovered(address token, uint256 amount);

```
- [contracts/periphery/BathBuddy.sol#L261](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L261)
- [contracts/periphery/BathBuddy.sol#L262](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L262)
- [contracts/periphery/BathBuddy.sol#L263](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L263)
- [contracts/periphery/BathBuddy.sol#L264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L264)
- [contracts/periphery/BathBuddy.sol#L265](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L265)
- [contracts/periphery/BathBuddy.sol#L266](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L266)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

50:     event PositionOpened(uint256 positionId, Position position);

51:     event PositionClosed(uint256 positionId);

52:     event MarginIncreased(uint256 positionId, uint256 amount);

```
- [contracts/utilities/poolsUtility/Position.sol#L50](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L50)
- [contracts/utilities/poolsUtility/Position.sol#L51](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L51)
- [contracts/utilities/poolsUtility/Position.sol#L52](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L52)

&nbsp;
## [N-02] Use fixed compiler version

A floating pragma risks a different compiler version being used in production vs testing, which poses security risks.

Instances: 2
```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```
- [contracts/RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2)

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```
- [contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2)

&nbsp;
## [N-03] Use appropriate function naming convention

According to Solidity's style guide and popular convention, function names should be prefixed with an underscore if and only if they are `private` or `internal`.

See [here](https://primitivefi.notion.site/Solidity-Style-44daebebfbd645b0b9cbad7075ba42fe) for more details.


Instances: 13
```solidity
File: contracts/RubiconMarket.sol

35:     function isAuthorized(address src) internal view returns (bool) {

46:     function add(uint256 x, uint256 y) internal pure returns (uint256 z) {

50:     function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {

54:     function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {

58:     function min(uint256 x, uint256 y) internal pure returns (uint256 z) {

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

77:     function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

81:     function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

85:     function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

89:     function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

```
- [contracts/RubiconMarket.sol#L35](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35)
- [contracts/RubiconMarket.sol#L46](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L46)
- [contracts/RubiconMarket.sol#L50](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L50)
- [contracts/RubiconMarket.sol#L54](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L54)
- [contracts/RubiconMarket.sol#L58](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58)
- [contracts/RubiconMarket.sol#L62](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62)
- [contracts/RubiconMarket.sol#L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66)
- [contracts/RubiconMarket.sol#L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70)
- [contracts/RubiconMarket.sol#L77](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L77)
- [contracts/RubiconMarket.sol#L81](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L81)
- [contracts/RubiconMarket.sol#L85](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L85)
- [contracts/RubiconMarket.sol#L89](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L89)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

125:     function openPosition(

```
- [contracts/utilities/poolsUtility/Position.sol#L125](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L125)

&nbsp;
## [N-04] Lines too long

[Solidity's style guide](https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length) states lines should be kept within 79 characters.
 
Also, if lines exceed 164 characters then a horizontal scroll bar will be required when viewing the file on Github.

Extensions such as prettier are a simple solution.

Instances: 1
```solidity
File: contracts/periphery/BathBuddy.sol

28:  * @dev The ~only~ external entrypoint used in the system is the getReward(token) function called by the Bath Token. Extra care should be taken to make sure only the beneficiary can ever access the funds and send them to the withdrawer (and fee to self)

```
- [contracts/periphery/BathBuddy.sol#L28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L28)

&nbsp;
## [N-05] Update import usages

To improve readability and adhere to the rule of modularity and modular programming: only import what you need. Specific imports with curly braces allow us to apply this rule better.

For example:
```
import {ERC721} from "solmate/tokens/ERC721.sol";
```

Instances: 25
```solidity
File: contracts/RubiconMarket.sol

11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

```
- [contracts/RubiconMarket.sol#L11](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L11)
- [contracts/RubiconMarket.sol#L12](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L12)

```solidity
File: contracts/BathHouseV2.sol

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "./compound-v2-fork/InterestRateModel.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

7: import "./compound-v2-fork/Comptroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

9: import "./periphery/BathBuddy.sol";

```
- [contracts/BathHouseV2.sol#L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L4)
- [contracts/BathHouseV2.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L5)
- [contracts/BathHouseV2.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L6)
- [contracts/BathHouseV2.sol#L7](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L7)
- [contracts/BathHouseV2.sol#L8](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L8)
- [contracts/BathHouseV2.sol#L9](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L9)

```solidity
File: contracts/V2Migrator.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

```
- [contracts/V2Migrator.sol#L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L4)
- [contracts/V2Migrator.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L5)
- [contracts/V2Migrator.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L6)

```solidity
File: contracts/periphery/BathBuddy.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

7: import "@openzeppelin/contracts/security/Pausable.sol";

```
- [contracts/periphery/BathBuddy.sol#L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L4)
- [contracts/periphery/BathBuddy.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L5)
- [contracts/periphery/BathBuddy.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6)
- [contracts/periphery/BathBuddy.sol#L7](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L7)

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
- [contracts/utilities/poolsUtility/Position.sol#L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L4)
- [contracts/utilities/poolsUtility/Position.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L5)
- [contracts/utilities/poolsUtility/Position.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6)
- [contracts/utilities/poolsUtility/Position.sol#L7](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L7)
- [contracts/utilities/poolsUtility/Position.sol#L8](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L8)
- [contracts/utilities/poolsUtility/Position.sol#L9](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L9)
- [contracts/utilities/poolsUtility/Position.sol#L10](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L10)
- [contracts/utilities/poolsUtility/Position.sol#L11](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L11)

```solidity
File: contracts/utilities/FeeWrapper.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "./RubiconRouter.sol";

```
- [contracts/utilities/FeeWrapper.sol#L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L4)
- [contracts/utilities/FeeWrapper.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L5)

&nbsp;
## [N-06] Implementing `renounceOwnership` is dangerous

Typically, the contract's owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

The OpenZeppelin's Ownable used in this project contract implements renounceOwnership. This can represent a certain risk if the ownership is renounced for any other reason than by design.

Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

It is recommended to either reimplement the function to disable it, or to clearly specify if it is part of the contract design.


Instances: 1
```solidity
File: contracts/utilities/poolsUtility/Position.sol

15: contract Position is Ownable, DSMath {

```
- [contracts/utilities/poolsUtility/Position.sol#L15](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L15)

&nbsp;
## [N-07] Remove/Resolve open TODOs

Instances: 3
```solidity
File: contracts/periphery/BathBuddy.sol

37: // TODO: not really the case anymore ^

92:     // TODO: do we need this?

```
- [contracts/periphery/BathBuddy.sol#L37](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L37)
- [contracts/periphery/BathBuddy.sol#L92](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L92)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

156:         // TODO: definitely need to work on naming things

```
- [contracts/utilities/poolsUtility/Position.sol#L156](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L156)

&nbsp;
## [N-08] Remove unused imports

Instances: 3
```solidity
File: contracts/BathHouseV2.sol

8: import "./compound-v2-fork/Unitroller.sol";

```
- [contracts/BathHouseV2.sol#L8](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L8)

```solidity
File: contracts/V2Migrator.sol

6: import "./compound-v2-fork/CTokenInterfaces.sol";

```
- [contracts/V2Migrator.sol#L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L6)

```solidity
File: contracts/utilities/FeeWrapper.sol

5: import "./RubiconRouter.sol";

```
- [contracts/utilities/FeeWrapper.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L5)

&nbsp;
## [N-09] Missing `address(0)` checks in constructor/initialize

Failing to check for invalid parameters on deployment may result in an erroneous input and require an expensive redeployment.

Instances: 2
```solidity
File: contracts/BathHouseV2.sol

32:     function initialize(address _comptroller, address _pAdmin) external {

```
- [contracts/BathHouseV2.sol#L32](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

```
- [contracts/utilities/poolsUtility/Position.sol#L54](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54)

&nbsp;
## [N-10] Remove deprecated/redundant variables

`buyEnabled` and `matchingEnabled` are always true, and so can be removed along with any conditionals relying on them to save gas, reduce bytecode size and improve readability. Also `AqueductDistributionLive` and `AqueductAddress` are described as deprecated in comments, and unused throughout the code.

Instances: 4
```solidity
File: contracts/RubiconMarket.sol

675:    bool public buyEnabled = true; //buy enabled TODO: review this decision!

676:    bool public matchingEnabled = true; //true: enable matching,

682:    bool public AqueductDistributionLive;

684:    address public AqueductAddress;
```
- [contracts/RubiconMarket.sol#L675](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675)
- [contracts/RubiconMarket.sol#L676](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676)
- [contracts/RubiconMarket.sol#L682](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682)
- [contracts/RubiconMarket.sol#L684](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L684)

&nbsp;
## [N-11] Remove commented out code snippets
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L99-L116
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L128-L150
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L184-L195
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L207
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L299-L309
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L428-L434
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L438
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L661
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L663
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L666
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L838
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L850-L851
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1343-L1359

&nbsp;
## [N-12] Remove useless function

The function `SimpleMarket::bump` is useless; it does not change state or return anything.

Instances: 1
```solidity
File: contracts/RubiconMarket.sol

297:     function bump(bytes32 id_) external can_buy(uint256(id_)) {
```
- [contracts/RubiconMarket.sol#L297](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297)