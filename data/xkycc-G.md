### Optimize `calcAmountAfterFee()`

*Proposed change*
```solidity
function calcAmountAfterFee6(
        uint256 amount
    ) public view returns (uint256 _amount) {
        require(amount > 0);
        _amount = makerFee() != 0 ? amount - (mul(amount, feeBPS + makerFee())) / 100_000 : amount - mul(amount, feeBPS) / 100_000;  // != 0 can be changed to > 0 if preferred
    }
```

*Saves a max of 752 gas when makerFee() != 0*

*Instances (1)*
```solidity
File: contracts/RubiconMarket.sol

582:    _amount = amount;
583:    _amount -= mul(amount, feeBPS) / 100_000;

585:    if (makerFee() > 0) {
586:        _amount -= mul(amount, makerFee()) / 100_000;
587:    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)



### Replace uint128 cast with with inequality check of to size of uint128

*Proposed change:*
```solidity
require(variable < 340282366920938463463374607431768211456, "variable is not an int");  // 340282366920938463463374607431768211455 (size of uint128) + 1
```

*Saves 50 gas per instance*

*Instances (4)*
```solidity
File: contracts/RubiconMarket.sol

321:             require(uint128(spend) == spend, "spend is not an int");

322:             require(uint128(quantity) == quantity, "quantity is not an int");

519:             require(uint128(pay_amt) == pay_amt);

520:             require(uint128(buy_amt) == buy_amt);

```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

