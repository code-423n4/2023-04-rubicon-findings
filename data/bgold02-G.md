### Remove creation of intermediate `uint256` in every loop iteration in `_borrowLimit()`
Assigning memory to temporary variables, when not necessary, results in extra gas being used in every `_borrowLimit()` function call. Remove these intermediary variables to save gas.

This will save gas on every iteration (~45 gas per iteration)

There are two instances of this.

__Replace the following:__
```solidity
File: contracts/utilities/poolsUtility/Position.sol
Lines 545-550:      uint256 _max = _maxBorrow(_bathToken);

            // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );

Lines 567-570:      uint256 _borrowDelta = _desiredAmount.sub(
                        _assetAmount.sub(_loopBorrowed)
                    );
                    _lastBorrow = _borrowDelta.mul(WAD).div(_loopBorrowed);
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol)

__with the following respectively:__
```solidity
_loopBorrowed = wmul(_assetAmount, _collateralFactor).add(_maxBorrow());

_lastBorrow = _desiredAmount.sub(
                  _assetAmount.sub(_loopBorrowed)
              ).mul(WAD).div(_loopBorrowed);
```