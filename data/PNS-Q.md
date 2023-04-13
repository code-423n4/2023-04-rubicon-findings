# Low Risk

## `buyEnabled` is redundant

The variable is always `true`. In addition, it is unnecessarily set to `true` again after contract initialization and checked only once (if `true`), but it is not possible to change its value to `false`.

```solidity
File: contracts/RubiconMarket.sol
675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!
...
715:         buyEnabled = true;
...
1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {
1184:         require(buyEnabled);
```

## `matchingEnabled` is redundant

The variable is always `true`. In addition, it is unnecessarily set to `true` again after contract initialization.

```solidity
File: contracts/RubiconMarket.sol
676:     bool public matchingEnabled = true; //true: enable matching,
...
714:         matchingEnabled = true; //audit:info already set to true
```
## Confusing error message
The message suggests that all conditions are met, but one is enough to cause an error.

```solidity
File: contracts/RubiconMarket.sol
721:         require(
722:             isClosed() ||
723:                 msg.sender == getOwner(id) ||
724:                 id == dustId ||
725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
726:             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
727:         );
```

## Missing event for critical parameter change

```solidity
File: contracts/RubiconMarket.sol
628:     function stop() external auth {
629:         stopped = true;
630:     }
```

# Non-Critical

## Inconsistent used of named return params
When a function has unused named returns and used return statements, these named returns become redundant. To improve readability and maintainability, these variables for the named returns can be removed.
Named returns is a style preference, however in a project, it’s nice to be consistent.

```solidity
File: contracts/RubiconMarket.sol
276:     function isActive(uint256 id) public view returns (bool active) {
277:         return offers[id].timestamp > 0;
278:     }
279: 
280:     function getOwner(uint256 id) public view returns (address owner) {
281:         return offers[id].owner;
282:     }
283: 
284:     function getRecipient(uint256 id) public view returns (address owner) {
285:         return offers[id].recipient;
286:     }
```
```solidity
File: contracts/RubiconMarket.sol
620:     function isClosed() public pure returns (bool closed) {
621:         return false;
622:     }
```
```solidity
File: contracts/RubiconMarket.sol
871:     function cancel(
872:         uint256 id
873:     ) public override can_cancel(id) returns (bool success) {
```
## Redundant modifier
The modifier below does nothing, and is widely used, it should be removed.

```solidity
File: contracts/RubiconMarket.sol
260:     modifier can_offer() virtual {
261:         _;
262:     }
```
### Example:
```solidity
File: contracts/RubiconMarket.sol
761:     function offer(
762:         uint256 pay_amt, //maker (ask) sell how much
763:         ERC20 pay_gem, //maker (ask) sell which token
764:         uint256 buy_amt, //maker (ask) buy how much
765:         ERC20 buy_gem //maker (ask) buy which token
766:     ) external can_offer returns (uint256) {
```
## Dead code should be removed
```solidity
File: contracts/RubiconMarket.sol
297:     function bump(bytes32 id_) external can_buy(uint256(id_)) { 
298:         uint256 id = uint256(id_);
299:         /// TODO: double check it is sound logic to kill this event
300:         /// emit LogBump(
301:         ///     id_,
302:         ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
303:         ///     offers[id].owner,
304:         ///     offers[id].pay_gem,
305:         ///     offers[id].buy_gem,
306:         ///     uint128(offers[id].pay_amt),
307:         ///     uint128(offers[id].buy_amt),
308:         ///     offers[id].timestamp
309:         /// );
310:     }
```
## `public` functions not called by the contract should be declared `external` instead

```solidity
File: contracts/BathHouseV2.sol
45:     function getBathTokenFromAsset(
46:         address asset
47:     ) public view returns (address) {
48:         return tokenToBathToken[asset];
49:     }
```

## Constants in comparisons should appear on the left side
[It's a mechanism to avoid mistakes like this](https://stackoverflow.com/a/370373)

Examples:

```solidity
File: contracts/RubiconMarket.sol
1233:         if (pos == 0) {
```
```solidity
File: contracts/RubiconMarket.sol
1319:             if (t_pay_amt == 0 || t_buy_amt == 0) {
```
## Use descriptive variable names
Variable names describing its operation improve understanding of the code.

`delb` => `delBlock` or `deletedBlock` for example.

```solidity
File: contracts/RubiconMarket.sol
689:         uint256 delb; //the blocknumber where this entry was marked for delete
```
## Function ordering does not follow the Solidity style guide
According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.13/style-guide.html#order-of-functions), functions should be laid out in the following order:
- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private

None of the contracts meet the guidelines.

## Spellcheck
Consider using the [VSCode plugin](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) or similar to help catch spelling errors during development.

Example: `multuple` => `multiple`
```solidity
File: contracts/RubiconMarket.sol
886:     /// @notice Batch offer functionality - multuple offers in a single transaction
```