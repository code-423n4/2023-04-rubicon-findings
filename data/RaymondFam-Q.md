## Named returns unutilized
The functions below in contract DSMath have the named returns `z` unutilized.

Consider having the affected functions refactored as follows:

[File: RubiconMarket.sol#L58-L72](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58-L72)

```diff
    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
-        return x <= y ? x : y;
+        z = x <= y ? x : y;
    }

    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
-        return x >= y ? x : y;
+        z = x >= y ? x : y;
    }

    function imin(int256 x, int256 y) internal pure returns (int256 z) {
-        return x <= y ? x : y;
+        z = x <= y ? x : y;
    }

    function imax(int256 x, int256 y) internal pure returns (int256 z) {
-        return x >= y ? x : y;
+        z = x >= y ? x : y;
    }
```
## Typo mistakes
[File: RubiconMarket.sol#L250](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L250)

```diff
-    // /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it
+    // /// @notice Modifier that checks the user to make sure they own the offer and it is valid before they attempt to cancel it
```
[File: Position.sol#L28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L28)

```diff
-        uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution
+        uint256 currentBathTokenAmount; // amount of bathTokenAsset in the moment of execution
```
## Hardcode `isClosed()`
`isClosed()` has been hardcoded to `false`, making the market always open. There is no option to close it.

Consider making the function toggling as follows, and have the function [initialized](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700) in contract `RubiconMarket` where possible:

[File: RubiconMarket.sol#L620-L622](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L622)

```diff
-    function isClosed() public pure returns (bool closed) {
+    function isClosed(bool status) public pure returns (bool closed) {
-        return false;
+        closed = status;
    }
```
## Missing dust amount check when making new offer
The dust amount check is only implemented prior to creating a new offer when there is left over [`pay_amt`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1327) after a maker has taken up an offer in `_matcho()`.

Consider having the same check implemented too when a user is making a new offer via `make()`:

[File: RubiconMarket.sol#L733-L750](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733-L750)

```diff
    function make(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    ) public override returns (bytes32) {
        require(pay_amt >= _dust[address(pay_gem)]
        return
            bytes32(
                offer(
                    pay_amt,
                    pay_gem,
                    buy_amt,
                    buy_gem,
                    msg.sender,
                    msg.sender
                )
            );
    }
```
## Include descriptive variables in mappings
Where possible, implement mappings with descriptive variables included for better readability.

Here are some of the instances entailed:

[File: RubiconMarket.sol#L691-L695](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691-L695)
  
```solidity
    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
    mapping(uint256 => uint256) public _near; //next unsorted offer id
```
Additionally, group them into a struct for better organization where possible.

## `synchronized()` over `require()`
In contract `RubiconMarket`, there are six instances where modifier [`synchronized()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269), a check and effect mutex, could have been used in the function visibility instead of just implementing `require(!locked)` that is devoid of re-entrancy protection if it is ever needed. 
 
[File: RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
834:        require(!locked, "Reentrancy attempt");

859:        require(!locked, "Reentrancy attempt");

874:        require(!locked, "Reentrancy attempt");

938:        require(!locked);

1034:        require(!locked);
```