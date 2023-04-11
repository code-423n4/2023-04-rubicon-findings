# Report  

## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use multiple require instead of && | 3 |
| [GAS-2](#GAS-2) | Require could be called earlier | 1 |
### <a name="GAS-1"></a>[GAS-1] Use multiple require instead of &&
Instead of chaining the conditions in one require (&&),split into multiple require will save gas (saving 3 gas per &).  
Though it costs more gas on deployment, it will end being cheaper with enough runtime calls.

*Instances (3)*:
```solidity
File: contracts/RubiconMarket.sol

893:         require(
                 payAmts.length == payGems.length &&
                     payAmts.length == buyAmts.length &&

1412:         require(
                  _rank[id].delb == 0 && //assert id is in the sorted list
                      isOfferSorted(id)

```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol)

```solidity
File: contracts/utilities/poolsUtility/Position.sol

591:             ? require(
                     _leverage > _wad && _leverage <= _leverageMax,
                     "_leverageCheck{Long}: INVLAID LEVERAGE"

```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)

### <a name="GAS-2"></a>[GAS-2] Require could be called earlier

Always try reverting transactions as early as possible when using require statements. In case a transaction revert occurs, the user will pay the gas up until the revert was executed (not afterwards).

Here it could be called under _totalAmount assignment:
```solidity
File: contracts/utilities/FeeWrapper.sol
108:     function _chargeFeePayable(
109:         FeeParams memory _feeParams
110:     ) internal returns (uint256 _msgValue) {
111:         // _feeToken is ETH
112:         uint256 _totalAmount = _feeParams.totalAmount;
113:         uint256 _feeAmount = _feeParams.feeAmount;
114:         address _feeTo = _feeParams.feeTo;
115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
```
