Use double require instead of using &&

Using double require instead of operator && can save more gas.
When having a require statement with 2 or more expressions needed, place the expression that cost less gas first.

in line 255 we have 
```
(msg.sender == getRecipient(id) && getOwner(id) == address(0))
```

use 2 require conditions instead to save more gas

similar report: https://code4rena.com/reports/2023-01-biconomy/#g-17-use-double-require-instead-of-using-