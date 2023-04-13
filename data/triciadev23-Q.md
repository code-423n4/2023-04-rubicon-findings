# Low/non-critical

## RubiconMarket.sol

### RubiconMarket.sol Line 260 we have a modifier that is unused/empty 

```
modifier can_offer() virtual { 
        _;
    }
```

### RubiconMarket.sol  ```pragma solidity ^0.8.9;``` which is different than the other contracts.

Best practices suggest that all contracts use the same compiler version.

