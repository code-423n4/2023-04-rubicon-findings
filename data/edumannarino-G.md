## [G-1] Inefficient/Less Clear Boolean Evaluation

It's inefficient to use IF/ELSE statement in this case. Prefer returning an inline evaluation.
Also is a code easier and clearer to read.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35L41

Replace
```
    function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }
```
For 
```
    function isAuthorized(address src) internal view returns (bool) {
        return (src == owner);
    }
```
A benchmark evaluation throws this gas amounts:
```
No Optimized - Evaluating TRUE: 24017
No Optimized - Evaluating FALSE: 24018
Optimized - Evaluating TRUE: 23930
Optimized - Evaluating FALSE: 23930
```