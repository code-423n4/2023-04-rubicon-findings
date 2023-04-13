### Issues Template
| Letter | Name | Description |
|:--:|:-------:|:-------:|
| L  | Low risk | Potential risk |
| NC |  Non-critical | Non risky findings |

| Total Found Issues | 22 |
|:--:|:--:|

### Low-Risk Template
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [L-01] | Failed Function Call Could Occur Without Contract Existence Check | 2 |
| [L-02] | Descriptive and Verbose Options | 4 |
| [L-03] | Missing ReEntrancy Guard to _chargeFeePayable function | 1 |
| [L-04] | Missing Event for critical parameters init and change | 4 |
| [L-05] | Add a timelock to critical functions | 2 |
| [L-06] | Critical Address Changes Should Use Two-step Procedure | 1 |
| [L-07] | Front-runnable initializer `spawnBuddy` | 1 |

| Total Low-Risk Issues | 7 |
|:--:|:--:|

### Non-Critical Template
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [N-01] | Netspec comment format error | 1 |
| [N-02] | String constants used in multiple places should be defined as constants | 6 |
| [N-03] | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 2 |
| [N-04] | Import declarations should import specific identifiers, rather than the whole file | 1 |
| [N-05] | Not using well known and safe contracts | 5 |
| [N-06] | constants should be defined rather than using magic numbers | 4 |
| [N-07] | Events that mark critical parameter changes should contain both the old and the new value | 2 |
| [N-08] | Missing Netspec comments for some functions | 4 |
| [N-09] | Not using the named return variables anywhere in the function is confusing | 5 |
| [N-10] | Floating pragma | 2 |
| [N-11] | Assembly Codes Specific – Should Have Comments | 1 |
| [N-12] | Lines Too Long | - |
| [N-13] | Contracts should have full test coverage | - |
| [N-14] | Function ordering does not follow the Solidity style guide | - |
| [N-15] | Tokens accidentally sent to the contract cannot be recovered | - |
| [N-16] | Use SMTChecker | - |
| Total Non-Critical Issues | 16 |
|:--:|:--:|

## Low-Risk issues
### [L-01] Failed Function Call Could Occur Without Contract Existence Check.
Performing low-level calls without confirming the contract’s existence (not yet deployed or has been destructed) could return success even though no function call was executed.


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L118
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#83


### [L-02] Descriptive and Verbose Options
Consider making the naming of local and state variables more verbose and descriptive so all other peer developers would better be able to comprehend the intended statement logic, significantly enhancing the code readability.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L227
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L235-238
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L346

### [L-03] Missing ReEntrancy Guard to `_chargeFeePayable` function
When users try to call the Rubicon Protocol using the `rubicall()`:

```solidity
File: \contracts\utilities\FeeWrapper.sol
48:     function rubicall(
49:         CallParams memory params
50:     ) external payable returns (bytes memory) {
51:         if (msg.value == 0) {
52:             return _rubicall(params);
53:         } else {
54:             return _rubicallPayable(params);
55:         }
56:     }
```
performs a low-level call in one of its subcalls using a `.call` instruction. Precisely, when trying to call either the `_rubicallPayable`:

```solidity
File: contracts\utilities\FeeWrapper.sol
76:     function _rubicallPayable(
77:         CallParams memory _params
78:     ) internal returns (bytes memory) {
79:         // charge fee from feeParams
80:         uint256 _msgValue = _chargeFeePayable(_params.feeParams);
81: 
82:         (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
83:             bytes.concat(_params.selector, _params.args)
84:         );
85: 
86:         require(_OK, "low-level call to the router failed");
87: 
88:         return _data;
89:     }
```
or the `_chargeFeePayable` function:

```solidity
File: contracts\utilities\FeeWrapper.sol
108:     function _chargeFeePayable(
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
However, both of them are not protected against reentrancy and could lead to user funds draining

### [L-04] Missing Event for critical parameters init and change
Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476



### [L-05] Add a timelock to critical functions
It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing the risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471



### [L-06] Critical Address Changes Should Use Two-step Procedure
The critical procedures should be a two-step process.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1478

### [L-07] Front-runnable initializer `spawnBuddy`
There is nothing preventing another account from calling the initializer `spawnBuddy` before the contract owner.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L1478


### [N-01] Netspec comment format error 
The syntax of some Netspec comments is not well written for example in line 250 of the RuiconMarket.sol file, we found that the line starts with 2 slashes followed by 3 slashes which have disabled the @notice identifier

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L250

## Non-Critical Issues
### [N-02] String constants used in multiple places should be defined as constants

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L341
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L356
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L361
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L834
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L874


### [N-03] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions

See this [link](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

### [N-04] Import declarations should import specific identifiers, rather than the whole file 

Using import declarations of the form import {<identifier_name>} from "some/file.sol" avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation.

Here is an example of such an issue in your code:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L11


### [N-05] Not using well-known and safe contracts
Reenetrancy and Initializer protection are built from scratch and not using openzeppelin contracts.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L834
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L874


### [N-06] constants should be defined rather than using magic numbers

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L338
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L346
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586


### [N-07] Events that mark critical parameter changes should contain both the old and the new value
This should especially be done if the new value is not required to be different from the old value.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471


### [N-08] Missing Netspec comments for some functions
Multiple defined function does not have clear Netspec documentation which makes their comprehension more difficult

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1069
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1157
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1183


### [N-09] Not using the named return variables anywhere in the function is confusing
Consider changing the variable to an unnamed one.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L280
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L284
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L873


### [N-10] Floating pragma
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

### [N-11] Assembly Codes Specific – Should Have Comments

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L367

### [N-12] Lines Too Long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens these days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible. Here are some of the instances entailed:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L376
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733


### [N-13] Contracts should have full test coverage
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.


### [N-14] Function ordering does not follow the Solidity style guide
According to the Solidity style guide, functions should be laid out in the following order :constructor(), receive(), fallback(), external, public, internal, private, but the cases below do not follow this pattern.


### [N-15] Tokens accidentally sent to the contract cannot be recovered
It can’t be recovered if tokens accidentally arrive at the contract address, which has happened to many popular projects, so I recommend adding a recovery code to your critical contracts.


### [N-16] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.
