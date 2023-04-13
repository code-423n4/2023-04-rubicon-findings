## RubiconMarket

### Add helper function to calculate fee amount

Fees are calculated on 4 different occasions:
- [L338](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338)
- [L346](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L346)
- [L583](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583)
- [L586](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586)

The code would be cleaner and less error prone if the following function was re-used to calculate fees:
```
function _calcFee(uint256 _amount, uint256 _feeBPS) internal returns (uint256) {
  return mul(_amount, _feeBPS) / 100_000;
}
```
