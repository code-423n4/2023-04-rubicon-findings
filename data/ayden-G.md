https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1432
At the beginning of the function, it is required that the value for _span[pay_gem][buy_gem] be greater than 0
```solidity
  - _span[pay_gem][buy_gem]--;
  + unchecked{ _span[pay_gem][buy_gem]--;}
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1444
The require statement is best placed at the beginning of the function for optimal performance
```solidity
    function _hide(
        uint256 id //id of maker offer to remove from unsorted list
    ) internal returns (bool) {
      + require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list
        uint256 uid = _head; //id of an offer in unsorted offers list
        uint256 pre = uid; //id of previous offer in unsorted offers list

      - require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list
```