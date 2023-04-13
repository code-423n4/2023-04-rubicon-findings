https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1129-L1130

Add a check to see if the ID exists.
```
require(offerId != 0, "offerId == 0");
```