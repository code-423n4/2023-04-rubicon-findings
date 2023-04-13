 1. Remove [kill](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L487) function  as it just just calls another external function with same paremeters.
```
function kill(bytes32 id) external override {

require(cancel(uint256(id)));

}
```
2. On `del_rank` function comment is written that it need to be called by keeper but no such logic in side del_rank function.
```solidity

// Function should be called by keepers.
function del_rank(uint256 id) external returns (bool) {

require(!locked);

  

require(

!isActive(id) &&

_rank[id].delb != 0 &&

_rank[id].delb < block.number - 10

);

delete _rank[id];

emit LogDelete(msg.sender, id);

return true;

}
```