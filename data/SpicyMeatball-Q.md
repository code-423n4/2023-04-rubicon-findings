Predefined values in the upgradable contract `RubiconMarket`
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675-L676

```diff
    - bool public buyEnabled = true; //buy enabled
    + bool public buyEnabled;
    - bool public matchingEnabled = true; //true: enable matching,
    + bool public matchingEnabled
```

https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable#avoid-initial-values-in-field-declarations
