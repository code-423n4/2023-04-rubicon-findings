## Non Critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [N-1](#N-1) | Use of block.timestamp | 9 
| [N-2](#N-2) | Large multiples of ten should use scientific notation rather than decimal literals, for readability | 15 |
| [N-3](#N-3) | Unused imports | 4 |
| [N-4](#N-4) | Constant values such as a call to keccak256(), should used to immutable rather than constant | 3 |
| [N-5](#N-5) | For modern and more readable code; update import usages |
| [N-6](#N-6) | Assembly Codes Specific – Should Have Comments |2 |
| [N-7](#N-7) | Lack of validation for the _feeTo address in the _chargeFeePayable() function |1 |
| [N-8](#N-8) | Simplify code |1 |
| [N-9](#N-9) | Check the length of the address list in bathTokensV1 and bathTokensV2 is the same|1 |


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Owner can renounce ownership | 6 |
| [L-2](#L-2) | Non-library/interface files should use fixed compiler versions, not floating ones | 2 |
| [L-3](#L-3) |Check if _feeAmount is less than or equal to _totalAmount | 1 |
| [L-4](#L-4) |Not using OpenZeppelin contracts | 1 |
| [L-5](#L-5) |isClosed() always returns false | 1 |
| [L-6](#L-6) |Function Calls in Loop Could Lead to Denial of Service | 7 |

### [N-1] Use of block.timestamp
Block timestamps have historically been used for a variety of applications, such as entropy for random numbers (see the Entropy Illusion for further details), locking funds for periods of time, and various state-changing conditional statements that are time-dependent. Miners have the ability to adjust timestamps slightly, which can prove to be dangerous if block timestamps are used incorrectly in smart contracts.
#### Recommended Mitigation Steps
Block timestamps should not be used for entropy or generating random numbers — i.e., they should not be the deciding factor (either directly or through some derivation) for winning a game or changing an important state.

Time-sensitive logic is sometimes required; e.g., for unlocking contracts (time-locking), completing an ICO after a few weeks, or enforcing expiry dates. It is sometimes recommended to use block.number and an average block time to estimate times; with a 10 second block time, 1 week equates to approximately, 60480 blocks. Thus, specifying a block number at which to change a contract state can be more secure, as miners are unable to easily manipulate the block number.

Instances where block.timestamp is used:

*Find (9) instance(s) in contracts*:
```solidity
File: RubiconMarket.sol

534:         info.timestamp = uint64(block.timestamp);

625:         return uint64(block.timestamp);

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

```solidity
File: periphery/BathBuddy.sol

116:             block.timestamp < periodFinish[token]

117:                 ? block.timestamp

195:         if (block.timestamp >= periodFinish[address(rewardsToken)]) {

201:                 block.timestamp

223:         lastUpdateTime[address(rewardsToken)] = block.timestamp;

224:         periodFinish[address(rewardsToken)] = block.timestamp.add(

237:             block.timestamp > periodFinish[token],

```
[periphery/BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol)




### [N-2] Large multiples of ten should use scientific notation rather than decimal literals, for readability
#### Recommendation
Scientific notation should be used for better code readability.
Use scientific notation (e.g. 1e18, 1e6) rather than exponentiation (e.g. 10**18, 100000).

*Find (15) instance(s) in contracts*:
```solidity
File: RubiconMarket.sol

74:     uint256 constant WAD = 10 ** 18;

75:     uint256 constant RAY = 10 ** 27;

1057:                     mul(pay_amt, 10 ** 9),

1059:                 ) / 10 ** 9;

1099:                         mul(buy_amt, 10 ** 9),

1101:                     ) / 10 ** 9

1142:                 mul(pay_amt, 10 ** 9),

1144:             ) / 10 ** 9

1175:                 mul(buy_amt, 10 ** 9),

1177:             ) / 10 ** 9

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

```solidity
File: utilities/poolsUtility/Position.sol

317:         _max = (_liq.mul(10 ** 18)).div(_price);

331:         ).div(10 ** 18);

459:         uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);

481:         uint256 _fee = _minFill.mul(_feeBPS).div(10000);

490:         _fee = _payAmount.mul(_feeBPS).div(10000);

```
[utilities/poolsUtility/Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)


### [N-3] Unused imports

*Find (4) instance(s) in contracts*:
```solidity
File: BathHouseV2.sol

8: import "./compound-v2-fork/Unitroller.sol";

```
[BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol)

```solidity
File: V2Migrator.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

```
[V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol)

```solidity
File: utilities/FeeWrapper.sol

5: import "./RubiconRouter.sol";

```
[utilities/FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/FeeWrapper.sol)


### [N-4] Constant values such as a call to keccak256(), should used to immutable rather than constant
There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand.
  
Constants should be used for literal values written into the code, and immutable variables should be used for expressions, or values calculated in, or passed into the constructor.

*Find (3) instance(s) in contracts*:
```solidity
File: RubiconMarket.sol

74:     uint256 constant WAD = 10 ** 18;

75:     uint256 constant RAY = 10 ** 27;

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

### [N-5] For modern and more readable code; update import usages

Our Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct polluted the source code with an unnecessary object we were not using because we did not need it. This was breaking the rule of modularity and modular programming: only import what you need Specific imports with curly braces allow us to apply this rule better.

#### Recommendation: 
import {contract1 , contract2} from "filename.sol";


### [N-06] Assembly Codes Specific – Should Have Comments
Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does

This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Aseembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.


```solidity
File: RubiconMarket.sol

648:      assembly {
649:             foo := calldataload(4)
650:             bar := calldataload(36)
651:             wad := callvalue()
652:         }

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

```solidity
File: utilities/poolsUtility/Position.sol

367:      assembly {
368:             switch _initBathTokenAmount
369:             case 0 {
370:                _bathTokenAmount := _currentBathTokenAmount
371:             }
372:             default {
373:                 _bathTokenAmount := sub(
374:  _currentBathTokenAmount,
375:                    _initBathTokenAmount
376:                )
377:            }
378:       }

```
[utilities/poolsUtility/Position.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol)

### [N-07] Lack of validation for the _feeTo address in the _chargeFeePayable() function
Not checking whether _feeTo is the zero address or a trusted address may result in sending out ETH in _chargeFeePayable(), leading to incorrect values of _msgValue.
```solidity
File: utilities/FeeWrapper.sol

108:    function _chargeFeePayable(
109:         FeeParams memory _feeParams
110:     ) internal returns (uint256 _msgValue) {
111:         // _feeToken is ETH
112:         uint256 _totalAmount = _feeParams.totalAmount;
113:         uint256 _feeAmount = _feeParams.feeAmount;
114:         address _feeTo = _feeParams.feeTo;
115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
116: 
117:         // transfer fee to the 3rd party protocol
118:         (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
119: 		require(OK, "ETH transfer failed");
120:         _msgValue = msg.value - _feeAmount;
121:     }
```
[utilities/FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol)
#### Recommended Mitigation Steps
Add validation for the _feeTo address to ensure that _msgValue is properly recorded.

### [N-08] Simplify code

Consider changing [this block](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41) of code into:
```
function isAuthorized(address src) internal view returns (bool) {
    return src == owner;
}
```
### [N-09] Check the length of the address list in bathTokensV1 and bathTokensV2 is the same
```solidity
File: V2Migrator.sol

30:      constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
31:        for (uint256 i = 0; i < bathTokensV1.length; ++i) {
32:            // set v1 to v2 bathTokens
33:            v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
34:        }
35:    }

```
[V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol)

#### Recommended Mitigation Steps
Consider adding:
```
require(bathTokensV1.length == bathTokensV2.length, "V1 and V2 tokens count mismatch");
```




### [L-1] Owner can renounce ownership
Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.
    
Ownable used in this project contract implements renounceOwnership. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.
    
#### Recommendation
We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.

*Find (6) instance(s) in contracts*:
```solidity
File: utilities/poolsUtility/Position.sol

7: import "@openzeppelin/contracts/access/Ownable.sol";

98:     ) external onlyOwner {

112:     ) external onlyOwner {

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {

```
[utilities/poolsUtility/Position.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)


### [L-2] Non-library/interface files should use fixed compiler versions, not floating ones 

*Find (2) instance(s) in contracts*:
```solidity
File: RubiconMarket.sol

2: pragma solidity ^0.8.9;

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

```solidity
File: periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```
[periphery/BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol)

### [L-3] Check if _feeAmount is less than or equal to _totalAmount
In _chargeFeePayable(), it should be ensured that _feeAmount <= _totalAmount, otherwise the transaction will fail.
#### Recommended Mitigation Steps
Consider adding:
```
require(_feeAmount <= _totalAmount, "FeeWrapper: fee exceeds total amount");
```

### [L-4] Not using OpenZeppelin contracts
[OpenZeppelin maintains a library of standard, audited, community-reviewed, and battle-tested smart contracts.](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/v2.5.0)


#### Recommended Mitigation Steps
Consider replacing the DSMath library with OpenZeppelin's SafeMath library.


### [L-5] isClosed() always returns false
The function isClosed() always returns false, which means that the market is always open, even after the designated closing time (close_time) has been reached, trading can still continue.
```solidity
File: RubiconMarket.sol

620:      function isClosed() public pure returns (bool closed) {
621:          return false;
622:      }

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

#### Recommended Mitigation Steps
Add a variable called close_time to record the time when the market closes, and track and compare the changes in close_time within the isClosed() function.

### [L-6] Function Calls in Loop Could Lead to Denial of Service
Function calls made in unbounded loop are error-prone with potential resource exhaustion as it can trap the contract due to the gas limitations or failed transactions.
*Find (7) instance(s) in contracts*:
```solidity
File: RubiconMarket.sol

899:      for (uint i = 0; i < payAmts.length; i++) { returns (bool closed) {

911:      for (uint i = 0; i < ids.length; i++) {

924:      for (uint i = 0; i < ids.length; i++) {

```
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
File: BathHouseV2.sol

122:  for (uint256 i = 0; i < buddies.length; ++i) {  
```
[BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol)

```solidity
File: V2Migrator.sol

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```
[V2Migrator.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol)


```solidity
File: utilities/poolsUtility/Position.sol

158:         for (uint256 i = 0; i < vars.limit; ++i) {

```
[utilities/poolsUtility/Position.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol)

```solidity
File: utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```
[utilities/FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol)
#### Recommended Mitigation Steps
Consider bounding the loop where possible to avoid unnecessary gas wastage and denial of service.



