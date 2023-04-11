### Missing check for the length
The `batchRequote()` function is used to update outstanding offers to new offers. The function lacks a check to see if the lengths of the arrays match.
```
function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }
    }
```
## Recommended Mitigation Steps
Check the length of an array as in the `batchOffer()` function.
```solidity
    function batchOffer(
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
        require(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );

```

