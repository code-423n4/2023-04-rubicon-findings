## Unused functions
Unused functions may be removed to reduce contract size and save gas on deployment. Apparently, 

Here is a specific instance entailed:

[File:https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
594:    bool public stopped;

628:    function stop() external auth {
629:        stopped = true;
630:    }
```
## Unneeded zero address check
The zero address check for `underlying` in the internal function `_bathify()` is a double job since it has already been executed by [`createBathToken()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L72-L75) calling `_bathify()`. 

Consider having the unneeded require statement removed to save gas both on function call and contract deployment:

[File: BathHouseV2.sol#L137-L149](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L137-L149)

```diff
    function _bathify(
        address _underlying
    )
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)
    {
-        require(_underlying != address(0), "_bathify: ADDRESS ZERO");

        _name = string.concat("bath", ERC20(_underlying).symbol());
        _symbol = string.concat(_name, "v2");
        _decimals = ERC20(_underlying).decimals();
    }
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
## Unneeded second condition
In `matcho()` of contract `RubiconMarket`, `t_pay_amt == 0` because of `t_buy_amt == 0`. as such, the second condition may be removed to save gas both on function call and contract deployment. The following refactoring is fully justifiable considering `t_pay_amt` could be zero due to a numerator product smaller than the denominator under edge cases:

[File: RubiconMarket.sol#L1313-L1321](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1313-L1321)

```diff
            buy(best_maker_id, min(m_pay_amt, t_buy_amt));
            emit LogMatch(id, min(m_pay_amt, t_buy_amt));
            t_buy_amt_old = t_buy_amt;
            t_buy_amt = sub(t_buy_amt, min(m_pay_amt, t_buy_amt));
            t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt_old;

-            if (t_pay_amt == 0 || t_buy_amt == 0) {
+            if (t_pay_amt == 0) {
                break;
            }
```
## += and -= cost more gas
`+=` and `-=` generally cost 22 more gas than writing out the assigned equation explicitly. The amount of gas wasted can be quite sizable when repeatedly operated in a loop.

For example, the `+=` instance below may be refactored as follows:

[File: Position.sol#L431](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431)

```diff
-        positions[_positionId].bathTokenAmount += _bathTokenAmount;
+        positions[_positionId].bathTokenAmount = _positions[_positionId].bathTokenAmount + bathTokenAmount;
```
## Non-strict inequalities are cheaper than strict ones
In the EVM, there is no opcode for non-strict inequalities (>=, <=) and two operations are performed (> + = or < + =).

As an example, consider replacing `>=` with the strict counterpart `>` in the following inequality instance:

[File: RubiconMarket.sol#L1049](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1049)

```diff
-            if (pay_amt >= offers[offerId].buy_amt) {
// Rationale for subtracting 1 on the right side of the inequality:
// x >= 10; [10, 11, 12, ...]
// x > 10 - 1 is the same as x > 9; [10, 11, 12 ...]
+            if (pay_amt > offers[offerId].buy_amt - 1) {
```
## Payable access control functions costs less gas
Consider marking functions with access control as `payable`. This will save 20 gas on each call by their respective permissible callers for not needing to have the compiler check for `msg.value`.

For instance, the code line below may be refactored as follows:

[File: BathHouseV2.sol#L60-L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60-L66)

```diff
    function createBathToken(
        address underlying,
        InterestRateModel interestRateModel,
        uint256 initialExchangeRateMantissa,
        address implementation,
        bytes memory becomeImplementationData
-    ) external onlyAdmin {
+    ) external payable onlyAdmin {
```
## `||` costs less gas than its equivalent `&&`
Rule of thumb: `(x && y)` is `(!(!x || !y))`

Even with the 10k Optimizer enabled: `||`, OR conditions cost less than their equivalent `&&`, AND conditions.

As an example, the instance below may be refactored as follows:

[File: RubiconMarket.sol#L349](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349)

```diff
-            if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
+            if (!(_offer.owner != address(0) || getRecipient(id) == address(0))) {
```
## Function order affects gas consumption
The order of function will also have an impact on gas consumption. Because in smart contracts, there is a difference in the order of the functions. Each position will have an extra 22 gas. The order is dependent on method ID. So, if you rename the frequently accessed function to more early method ID, you can save gas cost. Please visit the following site for further information:

https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

## Activate the optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
solidity: {
version: "0.8.17",
settings: {
  optimizer: {
    enabled: true,
    runs: 1000,
  },
},
},
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```
Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.
