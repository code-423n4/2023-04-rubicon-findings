# Transfer ownership two step

# Flatten setOwner, requireOwner transferOwnership

# replace 10\*\*18 with 10e18

# Named import

# Floating pragma

# Replace long parameters with custom struct data types or compact bytes

```js
File: PositionFactory.sol
13:     function createNewPosition(address _owner, address _zchf, address _collateral, uint256 _minCollateral,
14:         uint256 _initialLimit, uint256 initPeriod, uint256 _duration, uint256 _challengePeriod,
15:         uint32 _mintingFeePPM, uint256 _liqPrice, uint32 _reserve)
16:         external returns (address) {
17:         return address(new Position(_owner, msg.sender, _zchf, _collateral,
18:             _minCollateral, _initialLimit, initPeriod, _duration,
19:             _challengePeriod, _mintingFeePPM, _liqPrice, _reserve));
20:     }
```
