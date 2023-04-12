# Gas Optimizations

## [GAS-01] Unnecessary if statement in isAuthorized()
This code can be simplified. Most likely leftover from a more complex control structure in V1.
```diff
-if (src == owner) {
-    return true;
-} else {
-    return false;
-}
+ return (src == owner);
```

## [GAS-02] DSMath add/mul functions are depreceated with solidity 0.8
There are many calls in the code to DSMath.add() and DSMath.mul(), which are functions that check for overflows. However, given this code is compiled with solidity >= 0.8, this adds unnecessary overhead as the compiler will add these checks to the bytecode. Additionally these functions add to deployment cost which can be elimated by removing them.

SUGGESTIONS:
1. Remove add(), sub(), mul() from DSMath
2. Replace all calls to the functions in 1. with their normal operative counterparts (+,-,*)

## [GAS-03] Check if an offer will be deleted before writing new values in SimpleMarket.buy()
While performing a buy of an offer, the offer's struct values of ``pay_amt`` and ``buy_amt`` are written to storage with the updated amounts. At the end of the function, if ``pay_amt`` is zero, the struct is deleted from the mapping. To avoid unnecessary writes, they should only be performed if the ``id`` will not be deleted.

This is possible because the ``pay_amt`` and ``buy_amt`` of ``_offer`` are not accessed after calculating ``spend`` and the zero-check if statement.

```diff
-...
-offers[id].pay_amt = sub(_offer.pay_amt, quantity);
-offers[id].buy_amt = sub(_offer.buy_amt, spend);
-...
-if (offers[id].pay_amt == 0) {
-  delete offers[id];
-  ...
-}

+...
+if (_offer.pay_amt == 0) {
+  delete offers[id];
+  ...
+} else {
+  offers[id].pay_amt = _offer.pay_amt - quantity;
+  offers[id].buy_amt = _offer.buy_amt - spend;
+}
```

## [GAS-04] only call makerFee() once in SimpleMarket.buy()
In order to avoid accessing the makerFee slot twice, either cache the ``makerFee()`` value, or calculate ``mFee`` and check if that value is 0.

```diff
- if (makerFee() > 0) {
-  uint256 mFee = mul(spend, makerFee()) / 100_000;
-  ...
-}
+ uint256 mFee = mul(spend, makerFee()) / 100_000;
+ if (mFee > 0) {
+  ...
+}
```

## [GAS-05] Avoid double ERC20 transfers in SimpleMarket.buy()
Instead of sending the ``mFee`` and ``spend`` in two separate transfers, totals should be calculated and then a single transfer is sent (if ``_offer.owner != 0``).

```diff
-if (mFee > 0) {
-    if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
-        require(
-            _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, mFee),
-            "Insufficient funds to cover fee"
-        );
-    } else {
-        require(
-            _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee), "Insufficient funds to cover fee"
-        );
-    }
-    ...
-}

-require(
-    _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
-    "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
-);

+uint256 recipientAmount = spend;
+if (mFee > 0) {
+  if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
+    recipientAmount += mFee;
+  } else {
+     require(
+      _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),
+      "Insufficient funds to cover fee"
+    );
+  }
+  ...
+}

+require(
+    _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, recipientAmount),
+    "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
+);
```

## [GAS-06] Remove isClosed() from ExpiringMarket and RubiconMarket
The function ``isClosed()`` in ExpiringMarket is pure and always returns false. All calls to this function increase gas costs, and most external functions eventually call this call.

If the intention is to upgrade the market proxy at a later point and return isClosed() to true, this is not advised.