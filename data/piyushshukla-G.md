## <array>.length should not be looked up in every loop of a for-loop


### The overheads outlined below are PER LOOP, excluding the first loop

 1. storage arrays incur a Gwarmaccess (100 gas)

 2. memory arrays use MLOAD (3 gas)

 3. calldata arrays use CALLDATALOAD (3 gas)

Caching the length changes each of these to a DUP<N> (3 gas), and gets rid of the extra DUP<N> needed to store the stack offset

There is 1 instance of this issue:

file : contracts/utilities/FeeWrapper.sol

for (uint256 i = 0; i < tokenAmounts.length; ++i) {


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L40