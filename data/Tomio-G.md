
Title: Using SafeMath for solidity >0.8

Proof of Concept:
[Position.sol#L18](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L18)

Recommended Mitigation Steps:
it's better to remove `using SafeMath for uint256` for solidity >0.8

reference: https://github.com/OpenZeppelin/openzeppelin-contracts/issues/2465
________________________________________________________________________

Title: Expression for `constant` values such as a call to `keccak256()`, should use `immutable` rather than `constant`

Proof of Concept:
[RubiconMarket.sol#L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232)

Recommended Mitigation Steps:
Change from `constant` to `immutable`
reference: [here](https://github.com/ethereum/solidity/issues/9232)
________________________________________________________________________

Title: Using multiple `require` instead `&&` can save gas

Proof of Concept:
[RubiconMarket.sol#L893-L898](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893-L898)
[RubiconMarket.sol#L940-L944](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L940-L944)

Recommended Mitigation Steps:
Change to:

```
require(payAmts.length == payGems.length, "Array lengths do not match");
require(payAmts.length == buyAmts.length, "Array lengths do not match");
require(payAmts.length == buyGems.length, "Array lengths do not match");
```
________________________________________________________________________

Title: function _find() gas improvement on returning value

Proof of Concept:
[RubiconMarket.sol#L1214](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1214)

Recommended Mitigation Steps:
by set `old_top` in returns L#1208 and delete L#1214 can save gas

```
function _find(uint256 id) internal view returns (uint256 old_top) { //@audit-info: set here
```
________________________________________________________________________

Title: Gas savings for using solidity 0.8.10

impact:
Use a solidity version of at least 0.8 to get default underflow/overflow checks, use a solidity version of at least 0.8.2 to get simple compiler automatic inlining Use a solidity version of at least 0.8.3 to get better struct packing and cheaper multiple storage reads Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

Proof of Concept:
[BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2)

Recommended Mitigation Steps:
Consider to upgrade pragma to at least 0.8.10.

Solidity 0.8.10 has a useful change which reduced gas costs of external calls
Reference: [here](https://blog.soliditylang.org/2021/11/09/solidity-0.8.10-release-announcement/)
______________________________________________________________________