# Issues found

## 1. Don't Initialize Variables with Default Value

### Description
Uninitialized variables are assigned with the types default value.
Explicitly initializing a variable with it's default value costs unnecessary gas.

### Background Information
https://mudit.blog/solidity-tips-and-tricks-to-save-gas-and-reduce-bytecode-size/

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L122


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L40

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L31

## 2. Cache Array Length Outside of Loop

### Description
Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop.

### Background Information
https://github.com/code-423n4/2021-11-badgerzaps-findings/issues/36

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L122

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L37

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L38

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L40

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L894

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L895

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L896

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L897

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L31

## 3. Use != 0 instead of > 0 for Unsigned Integer Comparison

### Description
When dealing with unsigned integer types, comparisons with != 0 are cheaper than with > 0.

#### Findings:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L277

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L521

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L523

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L581

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L585

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1037

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1077

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1133

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1166

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1209

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1226

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1289

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1077

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1325

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1326

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1410

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1452

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L41

## 4. Long Revert Strings

### Description
Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

The contracts in scope allow using Solidity >=0.8.4, so consider using **[Custom Errors](https://blog.soliditylang.org/2021/04/21/custom-errors/)** as they are more gas efficient while allowing developers to describe the error in detail using **[NatSpec](https://docs.soliditylang.org/en/latest/natspec-format.html)**.

#### Findings:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L4

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L5

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L7

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L105

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#122

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L143

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L238

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L4

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L5

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L6

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L7

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L8

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L70

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L74

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/BathHouseV2.sol#L78

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/utilities/FeeWrapper.sol#L70

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/contracts/utilities/FeeWrapper.sol#L86

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L11

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L376

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L381

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L720

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L726

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L4

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L5

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L6

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L65

### Background Information
https://docs.uniswap.org/contracts/v3/reference/error-codes
https://github.com/code-423n4/2021-09-sushimiso-findings/issues/134

## 5. Use Shift Right/Left instead of Division/Multiplication if possible

### Description
A division/multiplication by any number ``` x ``` being a power of 2 can be calculated by shifting ``` log2(x) ``` to the right/left.

While the ```DIV``` opcode uses 5 gas, the ```SHR``` opcode only uses 3 gas. Furthermore, Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting.

### Background Information
https://www.evm.codes/?fork=merge

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L75

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L78

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L82

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L86

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L90

## 6. Empty Modifier

### Description
An Empty virtual modifier has been placed in the contract, even if you want to override later there is no need of adding an empty modifier in the contract, this just takes up storage space while uploading the contract.

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260-L262

### Recommendation
Conisder removing the modiifer from the contract.

## 7. Make Functions External

### Description
Function that is not called from inside the contract can be made ```external``` instead of ```public``` to save some gas.

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574

## 8. Make StateVariables Internal

### Description
Consider making the ```bool``` state variables internal RubiconMarket contract is not getting inherited further so there is no need for the variables to be ```public```.

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L679
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682

## 9. Memory To Calldata

### Description

In the function ```calculateFee```,  the function parameter ```tokenAmounts``` is set as memory but it can be changed to ```calldata``` as the parameter is only used to view the data. 

```calldata``` is very similar to ```memory``` in that it is a data location where items are stored. It is a special data location that contains the function arguments, only available for external function call parameters.

It is recommended to try to use ```calldata``` because it avoids unnecessary copies and ensures that the data is unaltered. Arrays and structs with ```calldata``` data location can also be returned from functions. 

### Impact
Is the cheapest location to use.

### Findings

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28-L32

## Recommendation
You can change to:
```solidity
   function calculateFee(
        uint256[] calldata tokenAmounts,
        uint256 feeType,
        uint256 feeValue
    )
```

### Another Findings 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L49
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L61
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L109
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30

## 10. Unncessary Else

### Description
In the ```isAuthorized```, the use of ```if``` and ```else``` can be omitted and the ```bool``` value can be directly returned from the function.

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L42

### Recommendation
You can change to:
```
   function isAuthorized2(address src) internal  view returns (bool) {
        return src == owner;
    }
```

## 11. Unncessary Is AuthCall

### Description
the ```isAuthorized``` function is an internal function and is only called once and that too in a modifier. Consider addding the check directly in the modifier itself to prevent unnecessarily gas cost.

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L40

## 12. Using Xor And Or

### Description

The ```XOR``` operator ```(^ and ^=)``` is essentially a boolean operator.
```
function compare1(uint a , uint b
     , uint c , uint d , uint e , uint f) internal pure returns(bool){
         return (a != b || c != d || e != f);
    }
```
```
    function compare2(uint a , uint b
     , uint c , uint d , uint e , uint f) internal pure returns(bool){
         return ((a ^ b) | (c ^ d) | (e ^ f)) != 0;
    }
```

In the given functions, the following are logical equavalent but still don't consume the same amount of gas.

1) (a != b || c != d || e != f);
2) ((a ^ b) | (c ^ d) | (e ^ f)) != 0;

Given 4 variables a, b, c and d represented as such:

0 0 0 0 0 1 1 0 <- a
0 1 1 0 0 1 1 0 <- b
0 0 0 0 0 0 0 0 <- c
1 1 1 1 1 1 1 1 <- d

To have ```a == b``` means that every 0 and 1 match on both variables. Meaning that a ```XOR (operator ^)``` would evaluate to 0 ```((a ^ b) == 0)```, as it excludes by definition any equalities.

Now, if a ```!= b```, this means that there’s at least somewhere a 1 and a 0 not matching between a and b, making ```(a ^ b) != 0```.

Both formulas are logically equivalent and using the ```XOR``` bitwise operator costs actually the same amount of gas:
```
      function xOrEquivalence(uint a, uint b) external returns (bool) {
        //return a != b; //370
        //return a ^ b != 0; //370
```
However, it is much cheaper to use the bitwise OR operator ```(|)``` than comparing the truthy or falsy values:
```
    function xOrOrEquivalence(uint a, uint b, uint c, uint d) external returns (bool) {
        //return (a != b || c != d); // 495
        //return (a ^ b | c ^ d) != 0; // 442
    }
```
These are logically equivalent too, as the OR bitwise operator ```(|)``` would result in a 1 somewhere if any value is not 0 between the ```XOR (^)``` statements, meaning if any ```XOR (^)``` statement verifies that its arguments are different.

#### Findings:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1362-L1377

### Recommendation
You can change to:
```
    pos = (((pos^ 0) |
           (offers[pos].pay_gem ^ pay_gem) |
            (offers[pos].buy_gem ^ buy_gem) ) ||
            !isOfferSorted(pos)  != 0)
            ? _find(id)
            : _findpos(id, pos);
```