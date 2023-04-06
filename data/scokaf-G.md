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
