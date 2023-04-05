# Issues found

## Don't Initialize Variables with Default Value

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

## Cache Array Length Outside of Loop

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

## Use != 0 instead of > 0 for Unsigned Integer Comparison

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

## Long Revert Strings

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

## Use Shift Right/Left instead of Division/Multiplication if possible

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

