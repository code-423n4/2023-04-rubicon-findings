# Gas Improvements
#### 1. A on-chain order book can be implement way more gas efficient with segement-trees
The current order book implementation is simple and straight forward in it's design but comes with higher gas-costs.
However, other more gas efficient implementations of on-chain order books are existing by using segement-trees and not storing everything in storage.

The protocol could potentially benefit from an in-depth analysis of these other implementation and implement similar patterns to reduce gas-costs drastically. 

See: https://ethresear.ch/t/enabling-on-chain-order-matching-for-order-book-dexs/14051
See: https://docs.clober.io/concepts/overview


#### 2. Use custom error types instead of error messages
Instead of using long error messages it would be more gas efficient to use custom error types.

### 3. Use public variables instead of getter methods
Instead of using getter methods for variable the method can just be made public.

### 4. Stop using DS-Math for basic arithmetic operations
In Solidity 0.8.0 a undeflow check is performed by default. In the current implementation the underflow check will happen twice. Which is not needed.


### 5. Optimize Struct storage slots
See Position contract.

Not all variables in this struct require a full `uint256`. Storage  costs can be reduced by using `uint128` or smaller and reordering the positions.

```solidity=
    // variables used in openPosition execution
    struct PosVars {
        uint256 borrowedAmount; // how much to borrow in current borrow loop
        uint256 initAssetBalance; // initial balance of asset
        uint256 limit; // limit of borrowing loops
        uint256 lastBorrow; // how much borrow on the last loop (check _borrowLimit)
        uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution
        uint256 currentAssetBalance; // balance of asset in the moment of execution
        uint256 toBorrow; // certain perc. to borrow from max amount available to borrow
    }
```