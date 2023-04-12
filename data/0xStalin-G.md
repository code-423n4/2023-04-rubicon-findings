## [G‑01] Unnecessary checks on `matchingEnabled` bool variable because its value is set to true since its declaration and it never changes - RubiconMarket contract
Some checks and code can be removed since the `matchingEnabled` bool variable is set to true, thus, any code depending on the value of the variable will always end up executing the path of the `true` value

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L863-L867
```
function(uint256, uint256) returns (bool) fn = matchingEnabled
   ? _buys
   : super.buy;

return fn(id, amount);
```
The above portion of code can be removed and in the `return` statement call directly the `_buys()` function


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L875-L881
```
if (matchingEnabled) {
    if (isOfferSorted(id)) {
        require(_unsort(id));
    } else {
        require(_hide(id), "can't hide");
    }
}
```
In the above code, the if() checking the value of `matchingEnabled` can be removed, leaving the code as follows:
```
if (isOfferSorted(id)) {
    require(_unsort(id));
} else {
    require(_hide(id), "can't hide");
}
```
