[G]No need to explicitly initialize variables with default values
If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for `address`…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.
I suggest removing explicit initializations for default values.
```
contracts/BathHouseV2.sol
122:        for (uint256 i = 0; i < buddies.length; ++i) {
```