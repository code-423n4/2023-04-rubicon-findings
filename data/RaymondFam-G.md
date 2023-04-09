## Unused functions
Unused functions may be removed to reduce contract size and save gas on deployment.

Here is a specific instance entailed:

[File:https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
594:    bool public stopped;

628:    function stop() external auth {
629:        stopped = true;
630:    }
```
## Private function with embedded modifier reduces contract size
Consider having the logic of a modifier embedded through a private function to reduce contract size if need be. A `private` visibility that saves more gas on function calls than the `internal` visibility is adopted because the modifier will only be making this call inside the contract.

For example, the modifier instance below may be refactored as follows:

[File: RubiconMarket.sol#L30-L33](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30-L33)

```diff
+    function _auth() private view {
+        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
+        }
+    }

    modifier auth() {
-        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
-        }
+        _auth();
        _;
    }
```
## Unneeded initialization
`matchingEnabled` and `buyEnabled` have been [separately assigned `true`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675-L676) in their state declarations. As such, having them initialized to true can be omitted to save gas on calling `initialize()`:

[File: RubiconMarket.sol#L714-L715](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714-L715) 

```diff
-        matchingEnabled = true;
-        buyEnabled = true;
```
## Use smaller uint128 or smaller type and pack structs variables to use lesser amount of storage slots
As the solidity EVM works with 32 bytes, most variables work fine with less than 32 bytes and may be packed inside a struct when sitting next to each other so that they can be stored in the same slot, this saves gas when writing to storage ~2000 gas.

For instance, struct `PosVars` of Position.sol may be refactored as follows:

[File: Position.sol#L23-L31](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L23-L31)

```diff
    struct PosVars {
-        uint256 borrowedAmount; // how much to borrow in current borrow loop
+        uint128 borrowedAmount; // how much to borrow in current borrow loop
-        uint256 initAssetBalance; // initial balance of asset
+        uint128 initAssetBalance; // initial balance of asset
-        uint256 limit; // limit of borrowing loops
+        uint128 limit; // limit of borrowing loops
-        uint256 lastBorrow; // how much borrow on the last loop (check _borrowLimit)
+        uint128 lastBorrow; // how much borrow on the last loop (check _borrowLimit)
-        uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution
+        uint128 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution
-        uint256 currentAssetBalance; // balance of asset in the moment of execution
+        uint128 currentAssetBalance; // balance of asset in the moment of execution
        uint256 toBorrow; // certain perc. to borrow from max amount available to borrow
    }
```
This alone will save ~6000 gas with 7 slots of storage reduced to 4 slot. Additionally, the respective max value of [`uint128`](https://velvetshark.com/articles/max-int-values-in-solidity), which is commonly adopted for token amounts in Uniswap V2 and V3, is more than sufficient catering to the limits of the struct variables as can be evidenced from the data table below:

```
uint            Digits          Max value
-----------------------------
uint128         39              340,282,366,920,938,463,463,374,607,431,768,211,455
```