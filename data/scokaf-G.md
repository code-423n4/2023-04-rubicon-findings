# 1: USE A MORE RECENT VERSION OF SOLIDITY

Optimization details

## Context:

Use a solidity version of at least 0.8.2 to get simple compiler automatic inlining

Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

## Proof of Concept

There is 1 instance of this issue:

File: BathBuddy.sol

2: pragma solidity ^0.8.0;

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2

## Tools Used

Manual Analysis


# 2: OPEN THE OPTIMIZER

Optimization details

## Context:

Always use the Solidity Optimizer to optimize gas costs. It’s good practice to set the optimizer as high as possible until it no longer helps reduce gas costs in function calls. This is advisable since function calls are intended to be executed many times than contract deployment, which only happens once.

## Proof of Concept

All Contracts

## Tools Used

Manual Analysis

### Recommended Mitigation Steps

In light of this information, We suggest you open the optimizer for v2-library.


# 3: SPLITTING require() STATEMENTS THAT USE && SAVES GAS

Optimization details

## Context:


More expensive gas usage


## Proof of Concept

> ***Results: 4 | Files: 3***

Instead of using operator && on a single require check. using double require check can save more gas:


> ***File: BathBuddy.sol***

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L95



> ***File: RubiconMarket.sol*** 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L253


> ***File: Position.sol***

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L591


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L595


## Tools Used

Manual Analysis


### Recommended Mitigation Steps

> ***Example***

> ***File: BathBuddy.sol***

Consider changing 

require(msg.sender == myBathTokenBuddy && msg.sender != address(0) && friendshipStarted, "You are not my buddy!);

To

require(msg.sender. == myBathTokenBuddy, "You are not my buddy!);
require(msg.sender != address(0) "You are not my buddy!);
require(friendshipStarted, "You are not my buddy!);

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L95
