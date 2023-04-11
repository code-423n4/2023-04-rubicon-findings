Non-critical01 -  Function isClosed() in RubiconMarket.sol is actually useless,it will always return false, just like a bool variable
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620

Non-critical02 - Function bump(bytes32 id_) in  RubiconMarket.sol is actually useless, it doesn't have a return value
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297