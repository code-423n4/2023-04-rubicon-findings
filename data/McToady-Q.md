**‘buy’ should revert if conditionals not met instead of returning false**

**Severity**: INFO

Line 331

```solidity
        if (
            quantity == 0 ||
            spend == 0 ||
            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt
        ) {
            return false;
        }
```

By reverting makes the user not submit a transaction that leads to no buy action.

**Recommendation**:
Should use revert instead of return false.

----------

**Possible incorrect emitFee Event argument**

**Severity**: Low

Lines365-272
The `emitFee` event takes an argument `feeTo` , this event is emitted in `buy` with the argument `_offer.owner` but to handle v1 to v2 migration it’s possible that `_offer.owner` is the zero address and in this case `feeTo` should be `_offer.recipient`.

**Recommendation:**
Add logic to check who is the correct `feeTo`before calling the `emitFee` event.
