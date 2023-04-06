# Use safeTransferOwnership instead of transferOwnership function

`transferOwnership` function is used to change Ownership on `Position.sol`. It is safer to use a 2 step method to change owners like `safeTransferOwnership`.

## Code Location
[Position.sol#L15](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L15)

## Recomendation
It is recommended to use the `ownable2step.sol` contract from oppenzeppelin. [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)