## modifier `synchronized` can be optimized to save gas

In code https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

```solidity
bool locked;

modifier synchronized() {
    require(!locked);
    locked = true;
    _;
    locked = false;
}
```
Booleans are more expensive than uint256 or any type that takes up a full word because each write operation emits an extra SLOAD to first read the slot's contents, replace the bits taken up by the boolean, and then write back. This is the compiler's defense against contract upgrades and pointer aliasing, and it cannot be disabled.

The values being non-zero value makes deployment a bit more expensive, but in exchange the refund on every call to nonReentrant will be lower in amount. Since refunds are capped to a percentage of the total transaction's gas, it is best to keep them low in cases like this one, to increase the likelihood of the full refund coming into effect.

## Recommendation
Use a private uint256 to define lock. Set it to 1 when open and 2 when not open, just like  https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol

```solidity
uint256 private _locked;

modifier synchronized() {
    require(_locked != 2);
    _locked = 2;
    _;
    _locked = 1;
}
```