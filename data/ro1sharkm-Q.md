## [QA-01] Use the complete name of types
Replace uint with uint256 everywhere in the codebase. Subtle bugs can slip if you just use uint, for example if you are hashing the signature of a method and use uint instead of uint256 for any parameter type.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L888-L890
## [QA-02] Comments with no functional or informative value
There exists some comments that provide little to no value in terms of describing the functionality. 
Here are example instances.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L11
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L136
## [QA-3] Open TODOs
Code architecture and error handling/reporting questions/issues should be resolved before deployment. There are open TODOs throughout the entire codebase. An example below.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L99
There are 13 instances of open TODOs in the codebase
## [QA-4] Contract does not follow the Solidity style guide’s suggested layout ordering
The style guide says that, within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Modifiers, and 5) Functions, but the contract below do not follow this ordering.
In the "periphery/bathBuddy.sol" contract the events appear at the end of the contract 
## [QA-5] Use a more recent version of solidity
Using the latest version of Solidity can help ensure compatibility with other contracts
The contract "periphery/bathBuddy.sol" makes use of solidity v 0.8.0 while others make use of version 0.8.17
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2 
## [QA-6] The Checks-Effects-Interactions pattern is not followed
It is recommended to follow the CEI pattern for security reasons even though they have no reentrancy vulnerability  at the moment.
The code below doesn't follow CEI
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L108-L122


## [QA-7] Import declarations should import specific identifiers, rather than the whole file
Using import declarations of the form import {<identifier_name>} from "some/file.sol" avoids polluting the symbol namespace making flattened files smaller and speeds up compilation.
Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L4-L7
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L4
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L4
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L11-L12
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L4-L6
## [QA-8] SafeMath usage in contract using Solidity version =>0.8.0
The "BathBuddy.sol contract currently uses the SafeMath library for arithmetic operations which is a recommended practice in Solidity versions prior to 0.8.0 to prevent overflows. However, Solidity version 0.8.0 and above offer built-in overflow checks, making the SafeMath library unnecessary.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2
## [QA-9] Inconsistent SPDX License Identifier in Contract Code
The contracts should be updated to have the same SPDX license identifier for all of its code. This will ensure that the licensing terms are clear and consistent. Some contracts make use of "MIT" while others use "AGPL-3.0"
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L1
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1
## [QA-9] Event is missing indexed fields
Each event should use three indexed fields if there are three or more fields, If there are fewer than three fields, all of the fields should be indexed
Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L23-L24
## [QA-10] Lines are too long
Usually lines in source code are limited to 80 characters.
Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L9
## [QA-11] Use Scientific Notation (e.g. 1e18) Instead of Exponentiation (e.g. 10**18).
it’s better coding practice to use idioms that do not require compiler optimization.
There are some instances;
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74
## [QA-12] Contract implements interface without extending the interface
Not extending the interface may lead to the wrong function signature being used leading to unexpected behavior.
The contract "V2Migrator.sol" implements interface "IBathToken" but does not extend it.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L17
## [QA-13] Remove commented out code
There exist several instances of commented code throughout the entire codebase. The code should be formatted
to remove commented code. Below are some instances.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L129-L150
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L185-L195
## [QA-14] Functions Missing Natspec and Comments
Missing Natspec comments or code comments makes it hard for external developers to easily understand the intended functionality. In the contract "BathHouseV2.sol",the function "_bathify()" is missing comments. 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L136-L150
## [QA-15] Use of Natspec Comments (///) for Regular Comments
Natspec comments (using the /// notation) have been used to create regular comments in the code. The code should be refactored to ensure comments use (//).
Example instance:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L666
## [QA-16] Partial Utilization of OpenZeppelin Contracts 
Consider importing the OpenZeppelin re-entrancy guard contract instead of re-implementing . Here is an example.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L264-L269
## [QA-17] Potential Gas Limit Exceedance for Function with Multiple Array Inputs
There is concern that as the size of the arrays increases, the function may exceed the block gas limit and become unable to execute. Here is an instance where by a function takes in four arrays as input parameters and loops through all during execution.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L887-L907

The code should be refactored to ensure its scalable to potential increase in array size.
## [QA-18] Possible Array Length Mismatch in Function
functions that takes in two or more arrays as input without validating their lengths may result in an array length mismatch error during execution. This error could potentially impact the functionality of the function.
Here are some instances.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30-L35
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L128
Recommendations: Always use "require()" statement before the loop to ensure arrays of equal length.
## [QA-19] Floating Pragma
Locking the pragma helps to ensure that contracts do not accidentally get deployed using another pragma,
for example, either an outdated pragma version that might introduce bugs or a recently released pragma
version which has not been extensively tested.
Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2
## [QA-20] Shorthand way to write if / else statement
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L48-L56

contracts/utilities/FeeWrapper.sol contains the code:
```
 
    function rubicall(
        CallParams memory params
    ) external payable returns (bytes memory) {
        if (msg.value == 0) {
            return _rubicall(params);
        } else {
            return _rubicallPayable(params);
        }
    }
```
This can be refactored to 
```
return (msg.value == 0) ? _rubicall(params) : _rubicallPayable(params);
```
This Increases readability and Shortens the overall SLOC.

## [QA-21] Missing Minimum Time Requirement for Reward Collection
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232-L242

The contract BathBuddy.sol implements a **setRewardDuration()** function as follows:

```
    function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );
        rewardsDuration[token] = _rewardsDuration;
        emit RewardsDurationUpdated(rewardsDuration[token]);
    }
```
The function ensures that previous rewards period  are complete before setting new ones but it does not
check for minimum time requirements.
Missing minimum time requirement for reward collection could  allow a malicious owner to set a very short time period for reward collection leading to unfair rewards distribution and loss of trust in the system
Precautions should be taken to implement a minimum time requirement for reward collection to prevent a malicious owner from setting a very short time period.
## [QA-21] Lack of isContract() Check in Functions Passing Contract Address
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L60-L84

```
    function createBathToken(
        address underlying,
        InterestRateModel interestRateModel,
        uint256 initialExchangeRateMantissa,
        address implementation,
        bytes memory becomeImplementationData
```

The contract BathHouseV2.sol takes a contract addresses (Implementation address) as parameter without checking if the address is actually a contract address or an externally owned account (EOA). This can potentially lead to unexpected behavior since EOAs do not have the same functionality as contract addresses. 
The function should include **isContract()** assembly code to verify contract addresses.
```
function isContract(address addr) internal view returns (bool) {
    uint256 size;
    assembly {
        size := extcodesize(addr)
    }
    return size > 0;
}
```







