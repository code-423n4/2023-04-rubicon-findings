## Gas Optimization Report

[G-01] Functions guaranteed to revert when called by normal users can be marked `payable`
===========================================================================================

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided costs an average of about 53 gas per call to the function, in addition to the extra deployment cost.

There are two instances found in the contract `BathBuddy.sol`:
Gas saved per instance: 53 gas

* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L194
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L235

There are five instances found in the contract `Position.sol`:
Gas saved per instance: 53 gas

* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L98
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L112
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L226
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L242

Proof Of Concept:
```solidity

    //27107 gas used
    function test1() external onlyOwner returns(uint256){
        return 200;
    }

    //27054 gas used
    function test2() external payable onlyOwner returns(uint256){
        return 200;
    }
    

```
