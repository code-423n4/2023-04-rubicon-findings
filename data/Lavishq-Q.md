## 1. The versions of solidity are different in all the files

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L2

version ^0.8.9, 0.8.17, ^0.8.0 are used in the files the versions are not consistent and 2 files have floating pragma. It is the best practice to use same solidity versions over all the files. Also, taking compiler version bugs in consideration.

## 2. array length are equal check
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30

It is necesary to check array length in cases where there is use of for loop on arrays so that it doesn't result in unwanted consequence

## 3. integer overflows and underflow checks are done by default since v0.8.0
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45

In that case, it is not necessary to use safeMath like library as it will cause more gas for checking and computing the same operation, including deploy bytecode size, so it is recommended to remove the functions that are not necessary.

## 4. libraries can be in different files for better reading Rubicon Market for better reading

[DSAuthEvents](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L15),
[DSAuth](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L22),
[DSMath](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L45),
[EventfulMarket](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L96),
[SimpleMarket](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L218),
[ExpiringMarket](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L593),
[DSNote](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L633),
[MatchingEvents](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L660)

are all in single file `RubiconMarket.sol` which is not a good practice since it affects readability and diverts focus while making the file too big (in terms of lines). So, it is recommended to separate them and it will enhance readability as well as developers/auditors can focus on writing functionality of one `contract`.


## 5. presence of dead/commented code

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L99
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L128
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1346

This are only few instances that I recorded but there are many instance of dead/commented code in the RubiconMarket.sol.

## 6. The solidity function can be written in ternary
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35

It will be better for readability and very easy to understand
`return src == owner ? true : false;`

instead of 
```
function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }
```

as well as here is another instance https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L48 can be just `return msg.value == 0? _rubicall(params) : _rubicallPayable(params);`

## 7. There is no access specifier for the variable

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224
It is best practice to explicitly define the access specifier of the variable, if it is not required to be read by users then it can be specified private. 

## 8. Missing address(0) check and anyone can call the function
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

address 0 checks for all the parameters is missing as well as the function can be called by anyone or frontrun -ned when admin tries to call [spawnBuddy()](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L71) function. And will lead the contract `BathBuddy` not usable.


## 9. Modifier declared but only used once and never can be removed and inlined

[onlyBathHouse](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L104) modifier is only used once and so it can be inlined and removed from the contract on top of that it will save some gas.
[]`onlyBuddy`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94) modifier is used never used so should be removed if there is no use of this.
As well as [`onlyAdmin`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L26)

## 10. The modifier is not supposed update state variables
[updateReward](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247) this can be defined inside an internal function or private function instead of a modifier. The reason is because it is really bad practice to update state variables in modifier, modifiers cost more gas than internal or private functions and often times this can result in hidden vulnerabilities which have been missed in past.

## 11. Modifier has no code in it but is called by a function

modifier [can_offer](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260) has no code in it.
It is called by https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L518