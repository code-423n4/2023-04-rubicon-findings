DSMath is not required because solidity version 0.8.x is used. Remove it and change code where its functions are used to save some gas. Same thing for OZ SafeMath in Position.sol and ButhBuddy.sol.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L44-L92

Use `buy` function internally in the contract instead of using [this](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L752-L754). We save two cast from uint to bytes32 and from bytes32 to uint.
Example: https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1053

