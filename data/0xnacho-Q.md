### 3rd party protocol fees can go to `address(0)`

In `_chargeFeePayable` and `_chargeFee` is recommended to check if the `_feeTo` value is different than `address(0)`. In some cases, the protocol can set the value for this field to `address(0)` by mistake, which will cause them to lose the fees.

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L97
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L114

### `calculateFee` can be bypassed


A 3rd party protocol can call the `rubicall` with different fees than the ones calculated by `calculateFee`, this way bypassing the recommended way of calculating the fees. 

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L48-L56

