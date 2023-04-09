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

[G-02] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables `(-= too)`
=====================================================================================

Using the addition operator instead of plus-equals saves `113 gas`. Subtractions act the same way.

There are two instances found in the contract `RubiconMarket.sol`:

* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586

There are three instances found in the contract `Position.sol`:

* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L184
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L560

[G-03] Splitting `require()` statements that use `&&` saves gas - (saves 8 gas per &&)
==========================================================================================
Instead of using the `&&` operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per `&&`.
The gas difference would only be realized if the revert condition is `realized(met)`.

## Proof Of Concept

The following tests were carried out in remix with both optimization turned on and off
```solidity
function multiple (uint a) public pure returns (uint){
	require ( a > 1 && a < 5, "Initialized");
	return  a + 2;
}
```
Execution cost
21617 with optimization and using &&
21976 without optimization and using &&

After splitting the require statement
```solidity
function multiple(uint a) public pure returns (uint){
	require (a > 1 ,"Initialized");
	require (a < 5 , "Initialized");
	return a + 2;
}
```
Execution cost
21609 with optimization and split require
21968 without optimization and using split require

There is one instance in the contract `RubiconMarket.sol`:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893-#L898

There is one instance in the contract `BathBuddy.sol`:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95-#L100

There is two instance in the contract `Position.sol`:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L591-#L594
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L595-#L598

[G-04] `Public` functions not called by the contract should be declared `external` instead
========================================================================================
Contracts are allowed to override their parents’ functions and change the visibility from external to public and can save gas by doing so.

There are six instances in the contracr `RubiconMarket.sol`:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L998
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1149

There is one instance in the contract `BathBuddy.sol`:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L139
