**[G-01] No need to explicitly initialize variables with default values**
If a variable is not set/initialized, it is assumed to have the default value (`0` for `uint`, `false` for `bool`, `address(0)` for `address`…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.
I suggest removing explicit initializations for default values.
```
File: contracts/BathHouseV2.sol
122:        for (uint256 i = 0; i < buddies.length; ++i) {
```

**[G-02] use `uint` instead of `bool` for `locked`**
Change this:
```
File: contracts/RubiconMarket.sol
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
File: contracts/RubiconMarket.sol
    uint256 locked = 1;

    modifier synchronized() {
      require(locked == 1);
      locked = 2;
      _;
     locked = 1;
    }
```

**[G-03] `++i` costs less gas compared to `i++` or `i += 1`**
++i costs less gas compared to i++ or i += 1 for unsigned integer, as pre-increment is cheaper (about 5 gas per iteration). This statement is true even with the optimizer enabled.
I suggest using ++i instead of i++ to increment the value of an uint variable.
```
File: contracts/RubiconMarket.sol
569:        last_offer_id++;
```

**[G-04] `> 0` is less efficient than `!= 0`**
`> 0` is used in the following location(s):
```
File: contracts/RubiconMarket.sol
277:        return offers[id].timestamp > 0;
```
```
File: contracts/RubiconMarket.sol
345:        if (makerFee() > 0) {
```
```
File: contracts/RubiconMarket.sol
521:        require(pay_amt > 0);
523:        require(buy_amt > 0);
1289:       while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
```