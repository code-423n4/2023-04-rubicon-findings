Title: Use `safeTransfer`/`safeTransferFrom` consistently instead of `transfer`/`transferFrom`

Impact:
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

Proof of Concept:
[Position.sol#L493](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L493)

Recommended Mitigation Steps
Consider using safeTransfer/safeTransferFrom or require() consistently.

Reference: [here](https://consensys.net/diligence/audits/2021/01/fei-protocol/#unchecked-return-value-for-iweth-transfer-call)
______________________________________________________________________

Title: Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)

Proof of Concept:
[RubiconMarket.sol#L74-L75](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75)
[RubiconMarket.sol#L1057-L1059](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1057-L1059)
______________________________________________________________________