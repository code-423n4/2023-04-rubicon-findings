**[G1]No need to explicitly initialize variables with default values**
If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for `address`…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.
I suggest removing explicit initializations for default values.
```
contracts/BathHouseV2.sol
122:        for (uint256 i = 0; i < buddies.length; ++i) {
```

**[G2]use `uint` instead of `bool` for `locked`**
Change this:
```
contracts/RubiconMarket.sol
224:    bool locked;

264:    modifier synchronized() {
265:      require(!locked);
266:      locked = true;
267:      _;
268:     locked = false;
269:    }
```
To:
```
contracts/RubiconMarket.sol
    uint256 locked = 1;

    modifier synchronized() {
      require(locked == 1);
      locked = 2;
      _;
     locked = 1;
    }
```