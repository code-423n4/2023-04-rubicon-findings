#### `feeBPS` is a misnomer

##### Severity: Low

##### Context: 

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583

##### Description

`feeBPS` is used in `RubiconMarket.sol` to account for fees. The problem with this variable name is that the actual fee logic considers the `feeBPS` out of a total of 100,000, rather than 10,000 which is implied by the term `BPS` (basis points). As a result, this can lead user to believe that their fee is 10x greater than it actually is.


#### `_isPricedLtOrEq` actually only returns true if priced strictly less than

##### Severity: Low

##### Context: 

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1261

##### Description

Contrary to the implication of its name, `_isPricedLtOrEq` only returns true if `low` offer price is strictly less than `high` offer price.