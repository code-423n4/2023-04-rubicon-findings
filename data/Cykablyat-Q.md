Warning: Function state mutability can be restricted to view
   --> rubicon.sol:297:5:
    |
297 |     function bump(bytes32 id_) external can_buy(uint256(id_)) {
    |     ^ (Relevant source part starts here and spans across multiple lines).