# [G-1] Use custom errors rather than revert()/require() strings to save gas

## Comments

Custom errors are available from solidity version 0.8.4. Custom errors save [~50 gas](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they’re hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L27
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L33
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L68
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L144

# [G-2] ++i/i++ should be unchecked {++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for- and while-loops

## Comments

In Solidity, the unchecked keyword can be used to disable overflow and underflow checking for a particular operation or expression. This can improve the gas efficiency of the smart contract and simplify the code.

## Instances 

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L122

# [G-3] Use nested if and, avoid multiple check combinations

## Comments

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L325
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1197

# [G-4] State variables should be cached in stack variables rather than re-reading them from storage

## Comments

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.


## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L334

# [G-5] Division by two should use bit shifting 

## Comments

Example: x / 2 is the same as x >> 1. 
While the compiler uses the SHR opcode to accomplish both, the version that uses division incurs an overhead of [20 gas](https://gist.github.com/IllIllI000/ec0e4e6c4f52a6bca158f137a3afd4ff) due to JUMPs to and from a compiler utility function that introduces checks which can be avoided by using unchecked {} around the division by two.

## Instances

https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L77
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L81
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L85
https://github.com/juniormp/audit-2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L89