## Summary

### Low risk issues
| |Issue|Instances| |
|-|:-|:-:|:-:|
| [L&#x2011;01] | onlyOwner/auth privileges can cause damage of the project in a possible privateKey exploit |13|
| [L&#x2011;02] | Keccak256 Constant values should used to immutable rather than constant keyword | 1 |


### Non-Critical issues
| |Issue|Instances| |
|-|:-|:-:|:-:|
| [N&#x2011;01] | Solidity compiler version should be exactly same in all smart contracts | 6 |
| [N&#x2011;02] | Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18) | 12 |
| [N&#x2011;03] | Default visibility of state variables is internal | 3 |
| [N&#x2011;04] | function returns incorrect naming convention | 1 |
| [N&#x2011;05] | Code can be simplified and more readable | 1 |
| [N&#x2011;06] | Use a more recent version of solidity | 6 |
| [N&#x2011;07] | Do not use floating solidity compiler version. Lock the solidity compiler version | 2 |
| [N&#x2011;08] | Use nested if and, avoid multiple check combinations | 2 |
| [N&#x2011;09] | Function ordering does not follow the Solidity style guide | 4 |
| [N&#x2011;10] | Contract does not follow the Solidity style guide’s suggested layout ordering | 1 |
| [N&#x2011;11] | Events Associated With Setter Functions | 3 |
| [N&#x2011;12] | Missing event and or timelock for critical parameter change | 3 |
| [N&#x2011;13] | For internal functions, follow Solidity standard naming conventions | 13 |
| [N&#x2011;14] | Remove commented out code | 1 |
| [N&#x2011;15] | Lack of address(0) checks in the constructor | 1 |
| [N&#x2011;16] | Use named parameters for mapping type declarations | 17 |

### Low risk issues
### [L&#x2011;01]  onlyOwner/auth privileges can cause damage of the project in a possible privateKey exploit
The contract owner account is the one who deploys the smart contract. Therefore the owner is able to perform certain privileged activities like updating functions, etc as per smart contract functioanlities.
In the Ethos-Core smart contracts, the owner has number of privileges like updating collateral ratios, set yeilding percentage/claim/distribution, etc. but there is no timelock and Multisign structure while taking care of these functions. Mostly, the Owner is an Externally Owned Account(EOA).
With the increase in private key theft, which may in case of project owners, this vulnerability has been termed as medium vulnerability.

Similar vulnerabilities: Private keys theft/stolen.

Hackers have stolen cryptocurrency worth around €540 million from a blockchain project linked to the popular online game Axie Infinity, in one of the largest cryptocurrency heists on record.
Security issue : PrivateKey of the project officer was stolen: [News article reference](https://www.euronews.com/next/2022/03/30/blockchain-network-ronin-hit-by-552-million-crypto-heist)

### Proof of concept
There are 13 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

25    function setOwner(address owner_) external auth {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25)

```solidity
File: contracts/RubiconMarket.sol

628    function stop() external auth {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628)

```solidity
File: contracts/RubiconMarket.sol

955    function setMinSell(
956        ERC20 pay_gem, //token to assign minimum sell amount to
957        uint256 dust //maker (ask) minimum sell amount
958    ) external auth note returns (bool) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L958)

```solidity
File: contracts/RubiconMarket.sol

1466    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
---

1471    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
---

1476    function setFeeTo(address newFeeTo) external auth returns (bool) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1476)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

98    ) external onlyOwner {
---

112    ) external onlyOwner {
---

210    function closePosition(uint256 posId) external onlyOwner {
---

226    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
---

242    function withdraw(address token, uint256 amount) external onlyOwner {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L98-L242)

```solidity
File: contracts/periphery/BathBuddy.sol

194    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L194)

```solidity
File: contracts/periphery/BathBuddy.sol

235    ) external onlyOwner {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L235)

### Recommended Mitigation Steps
1- A timelock contract should be added to use onlyOwner privileges. In this way, users can be warned in case of a possible security weakness. It will also create transparency.
2- onlyOwner can be a Multisign wallet.


### [L&#x2011;02]  Keccak256 Constant values should used with immutable rather than constant
There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.
While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand.
There is 1 instance of this issue.

```solidity
File: contracts/RubiconMarket.sol

232    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232)

### Non-Critical issues
### [N&#x2011;01]  Solidity compiler version should be exactly same in all smart contracts
Solidity compiler version should be exactly same in all smart contracts. RubiconMarket.sol and BathBuddy.sol solidity version differs.
There are 6 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

2   pragma solidity ^0.8.9;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)

```solidity
File: contracts/BathHouseV2.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L2)

```solidity
File: contracts/V2Migrator.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L2)

```solidity
File: contracts/periphery/BathBuddy.sol

2   pragma solidity ^0.8.0;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)

```solidity
File: contracts/utilities/FeeWrapper.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L2)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L2)

### [N&#x2011;02]  Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18) 
While the compiler knows to optimize away the exponentiation, it’s still better coding practice to use idioms that do not require compiler optimization, if they exist
There are 12 instances of this issue:

```solidity
File: contracts/RubiconMarket.sol

74    uint256 constant WAD = 10 ** 18;
75    uint256 constant RAY = 10 ** 27;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75)

```solidity
File: contracts/RubiconMarket.sol

1057                    mul(pay_amt, 10 ** 9),
---

1059                    ) / 10 ** 9;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1057-L1059)

```solidity
File: contracts/RubiconMarket.sol

1099                        mul(buy_amt, 10 ** 9),
---

1101                    ) / 10 ** 9
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1099-L1101)

```solidity
File: contracts/RubiconMarket.sol

1142                mul(pay_amt, 10 ** 9),
---

1144            ) / 10 ** 9
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1142-L1144)

```solidity
File: contracts/RubiconMarket.sol

1175                mul(buy_amt, 10 ** 9),
---

1177            ) / 10 ** 9
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1175-L1177)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

317        _max = (_liq.mul(10 ** 18)).div(_price);
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

331        ).div(10 ** 18);
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L331)

### [N&#x2011;03]  Default visibility of state variables is internal 
The default visibility of state variables is internal, there is no need to mention it specifically. 
[Link to Reference](https://docs.soliditylang.org/en/v0.8.17/contracts.html)

There are 2 instances of this issue. 

```solidity
File: contracts/RubiconMarket.sol

227    uint256 internal feeBPS;
---
230    address internal feeTo;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L227-L230)

### [N&#x2011;04]  function returns incorrect naming convention
In RubiconMarket.sol, getRecipient() returns incorrect naming which creates confusion in code readability.
There is 1 instance of this issue.

```solidity
File: contracts/RubiconMarket.sol

284    function getRecipient(uint256 id) public view returns (address owner) {
285        return offers[id].recipient;
286    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284-L286)

### Recommended Mitigation steps
In RubiconMarket.sol, getRecipient() returns name should be recipient instead of owner.

```solidity
File: contracts/RubiconMarket.sol

+    function getRecipient(uint256 id) public view returns (address recipient) {
        return offers[id].recipient;
    }
```

### [N&#x2011;05]  Code/Contract can be simplified and more readable
In RubiconMarket.sol, DSAuth contract can be simplied, more readable and will save some gas. 
There is 1 instance of this issue. 

```solidity
File: contracts/RubiconMarket.sol

22 contract DSAuth is DSAuthEvents {
23    address public owner;
24
25   function setOwner(address owner_) external auth {
26        owner = owner_;
27        emit LogSetOwner(owner);
28    }
29
30    modifier auth() {
31        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
32        _;
33    }
34
35    function isAuthorized(address src) internal view returns (bool) {
36        if (src == owner) {
37            return true;
38        } else {
39            return false;
40        }
41    }
42 }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L22-L42)

### Recommended Mitigation steps

```solidity
File: contracts/RubiconMarket.sol

 contract DSAuth is DSAuthEvents {
    address public owner;

   function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }

-    modifier auth() {
-        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
-        _;
-    }

+    modifier auth() {
+        require(owner == msg.sender), "ds-auth-unauthorized");
+        _;
+    }

-    function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
-    }
 }
```
### [N&#x2011;06]  Use a more recent version of solidity
Recommend to use latest solidity compiler version(0.8.18 or 0.8.19). This will safeguard from underflow and overflow conditions. This modification will make the contracts more gas optimized. safeMath library wont be explicitely required as ^0.8.0 version has already taken  the underflow and overflow conditions by default.
There are 6 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

2   pragma solidity ^0.8.9;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)

```solidity
File: contracts/BathHouseV2.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L2)

```solidity
File: contracts/V2Migrator.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L2)

```solidity
File: contracts/periphery/BathBuddy.sol

2   pragma solidity ^0.8.0;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)

```solidity
File: contracts/utilities/FeeWrapper.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L2)


```solidity
File: contracts/utilities/poolsUtility/Position.sol

2   pragma solidity 0.8.17;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L2)

### [N&#x2011;07]  Do not use floating solidity compiler version. Lock the solidity compiler version
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
There are 2 instances of this issue.

```solidity
File: contracts/RubiconMarket.sol

2   pragma solidity ^0.8.9;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)

```solidity
File: contracts/periphery/BathBuddy.sol

2   pragma solidity ^0.8.0;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)



### [N&#x2011;08]  Use nested if and, avoid multiple check combinations
Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

```solidity
File: contracts/RubiconMarket.sol

349            if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L349)

```solidity
File: contracts/RubiconMarket.sol

1197        if (
1198            isActive(id) &&
1199            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
1200        ) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1197-L1200)

```solidity
File: contracts/RubiconMarket.sol

1324        if (
1325            t_buy_amt > 0 &&
1326            t_pay_amt > 0 &&
1327            t_pay_amt >= _dust[address(t_pay_gem)]
1328        ) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1324-L1328)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

176            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L176)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

391        if (
392            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
393            borrowBalance(_bathTokenAsset) == 0
394        ) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L391-L394)

### [N&#x2011;09]  Function ordering does not follow the Solidity style guide
According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :constructor(), receive(), fallback(), external, public, internal, private, but the cases below do not follow this pattern.
There are 4 instances of this issue.

Below contracts does not follow,
1) RubiconMarket.sol
2) BathHouseV2.sol
3) Position.sol
4) BathBuddy.sol

### [N&#x2011;10]  Contract does not follow the Solidity style guide’s suggested layout ordering
The [Solidity style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout), says that within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Modifiers, and 5) Functions, but the contract(s) below do not follow this ordering.
There are 1 instances of this issue.
BathBuddy.sol contract does not follow,

### [N&#x2011;11]  Events Associated With Setter Functions
Consider having events associated with setter functions emit both the new and old values instead of just the new value.
There are 4 instances of this issue.

```solidity
File: contracts/periphery/BathBuddy.sol

232    function setRewardsDuration(
233        uint256 _rewardsDuration,
234        address token
235    ) external onlyOwner {
236        require(
237            block.timestamp > periodFinish[token],
238            "Previous rewards period must be complete before changing the duration for the new period"
239        );
240        rewardsDuration[token] = _rewardsDuration;
241        emit RewardsDurationUpdated(rewardsDuration[token]);
242    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232-L242)

```solidity
File: concontracts/utilities/poolsUtility/Position.sol

426    function _updateMargin(
427        uint256 _positionId,
428        uint256 _bathTokenAmount,
429        uint256 _marginAmount
430    ) internal {
431        positions[_positionId].bathTokenAmount += _bathTokenAmount;
432
433        emit MarginIncreased(_positionId, _marginAmount);
434    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L426-L434)

```solidity
File: contracts/RubiconMarket.sol

25    function setOwner(address owner_) external auth {
26        owner = owner_;
27        emit LogSetOwner(owner);
28    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)

```solidity
File: contracts/RubiconMarket.sol

955    function setMinSell(
956        ERC20 pay_gem, //token to assign minimum sell amount to
957        uint256 dust //maker (ask) minimum sell amount
958    ) external auth note returns (bool) {
959        _dust[address(pay_gem)] = dust;
960        emit LogMinSell(address(pay_gem), dust);
961        return true;
962    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L962)

### [N&#x2011;12]  Missing event and or timelock for critical parameter change
Events help non-contract tools to track changes, and events prevent users from being surprised by changes
There are 3 instances of this issue:

```solidity
File: contracts/RubiconMarket.sol

1466    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1467        feeBPS = _newFeeBPS;
1468        return true;
1469    }
1470
1471    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1472        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
1473        return true;
1474    }
1475
1476    function setFeeTo(address newFeeTo) external auth returns (bool) {
1477        require(newFeeTo != address(0));
1478        feeTo = newFeeTo;
1479        return true;
1480    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1480)

### [N&#x2011;13]  For functions, follow Solidity standard naming conventions
Proper use of _ as a function name prefix should be taken care and a common pattern is to prefix internal and private function names with _.
There are 13 instances of this issue.

Below internal functions does not follow this pattern which leads to confusion while reading code and affects overall readability of the code.

```solidity
File: contracts/RubiconMarket.sol

35    function isAuthorized(address src) internal view returns (bool) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L35)

```solidity
File: contracts/RubiconMarket.sol

46    function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

50    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

54    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

58    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

62    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

66    function imin(int256 x, int256 y) internal pure returns (int256 z) {
---

70    function imax(int256 x, int256 y) internal pure returns (int256 z) {
---

77    function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

81    function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

85    function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
---

89    function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L46-L89)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

125    function openPosition(
126        address asset,
127        address quote,
128        uint256 initMargin,
129        uint256 leverage
130    ) internal returns (bool OK) {```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L125-L130)

### [N&#x2011;14]  Remove commented out code
Below commented code should be removed for clean and better code readability. 

```solidity
File: contracts/RubiconMarket.sol

    // // Make a new offer without putting it in the sorted list.
    // // Takes funds from the caller into market escrow.
    // // Keepers should call insert(id,pos) to put offer in the sorted list.
    // function _offeru(
    //     uint256 pay_amt, //maker (ask) sell how much
    //     ERC20 pay_gem, //maker (ask) sell which token
    //     uint256 buy_amt, //maker (ask) buy how much
    //     ERC20 buy_gem, //maker (ask) buy which token
    //     address owner,
    //     address recipient
    // ) internal returns (uint256 id) {
    //     require(_dust[address(pay_gem)] <= pay_amt);
    //     id = super.offer(pay_amt, pay_gem, buy_amt, buy_gem, owner, recipient);
    //     _near[id] = _head;
    //     _head = id;
    //     emit LogUnsortedOffer(id);
    // }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1343-L1359)

### [N&#x2011;15]  Lack of address(0) checks in the constructor
Zero-address check should be used in the constructors, to avoid the risk of setting address(0) at deployment time.
There is 1 instance of this issue.

```solidity
File: contracts/utilities/poolsUtility/Position.sol

54    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
55        oracle = PriceOracle(_oracle);
56        rubiconMarket = RubiconMarket(_rubiconMarket);
57        bathHouseV2 = BathHouseV2(_bathHouseV2);
58        comptroller = bathHouseV2.comptroller();
59    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54-L59)


### [N&#x2011;16]  Use named parameters for mapping type declarations
Consider using named parameters in mappings (e.g. mapping(address account => uint256 balance)) to improve readability. This feature is present since Solidity 0.8.18.
There are 17 instances of this issue.

```solidity
File: contracts/periphery/BathBuddy.sol

52    mapping(address => uint256) public periodFinish; // Token specific
53    mapping(address => uint256) public rewardRates; // Token specific reward rates
54    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55    mapping(address => uint256) public lastUpdateTime; //Token specific
56    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
57
58    // Token then user always
59    mapping(address => mapping(address => uint256))
60        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L52-L61)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

42    mapping(uint256 => Position) public positions;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L42)

```solidity
File: contracts/BathHouseV2.sol

20    mapping(address => address) private tokenToBathToken;
21    mapping(address => address) private bathTokenToBuddy;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L20-L21)


```solidity
File: contracts/RubiconMarket.sol

222    mapping(uint256 => OfferInfo) public offers;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L222)

```solidity
File: contracts/RubiconMarket.sol

691    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in 
       sorted orderbook
694    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695    mapping(uint256 => uint256) public _near; //next unsorted offer id
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L691-L695)

```solidity
File: contracts/RubiconMarket.sol

19    mapping(address => address) public v1ToV2Pools;
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L19)
