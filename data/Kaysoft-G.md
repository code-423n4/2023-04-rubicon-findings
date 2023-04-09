## [GAS-01] REFACTOR THE `auth` modifier to reduce gas cost
The auth modifier uses an isAuth function which will expend more gas. The logic to check owner can be written directly in the `auth` modifier.
File: [RubiconMarket.sol#L30-L41](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30-L41)
Update:

```diff
modifier auth() {
-        require(isAuthorized(msg.sender), "ds-auth-unauthorized"); //@audit rewrite modifier to save gas.
+        require(msg.sender == owner, "ds-auth-unauthorized");
      _;
    }

-    function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
-    }
```
The refactored modifier will check owner directly this way

```solidity
modifier auth() {
      require(msg.sender == owner, "ds-auth-unauthorized");
      _;
    }
```

## [GAS-02] REMOVING REDUNDANT CODE CAN SAVE GAS

FILES: 
- [RubiconMarket.sol#L299-L309](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L299-L309)
- [RubiconMarket.sol#L1343-L1359](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1343-L1359)

## [GAS-03] REPLACE `10 ** 18` WITH `1e18` TO SAVE GAS

FILE: [Position.sol#L317](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317)
```diff
function _maxBorrow(address _bathToken) internal view returns (uint256 _max) {
        (uint256 _err, uint256 _liq, uint256 _shortfall) = comptroller.getAccountLiquidity(address(this));

        require(_err == 0, "_maxBorrow: ERROR");
        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
        require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

        uint256 _price = oracle.getUnderlyingPrice(CToken(_bathToken));
-        _max = (_liq.mul(10 ** 18)).div(_price);
+        _max = (_liq.mul(1e18)).div(_price);
        require(_max > 0, "_maxBorrow: can't borrow 0");
    }
```


