## Named returns unutilized
The functions below in contract DSMath have the named returns `z` unutilized.

Consider having the affected functions refactored as follows:

[File: RubiconMarket.sol#L58-L72](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58-L72)

```diff
    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
-        return x <= y ? x : y;
+        z = x <= y ? x : y;
    }

    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
-        return x >= y ? x : y;
+        z = x >= y ? x : y;
    }

    function imin(int256 x, int256 y) internal pure returns (int256 z) {
-        return x <= y ? x : y;
+        z = x <= y ? x : y;
    }

    function imax(int256 x, int256 y) internal pure returns (int256 z) {
-        return x >= y ? x : y;
+        z = x >= y ? x : y;
    }
```
## Inexpedient returned output of sellAllAmount() and buyAllAmount()
On the last code line of [`sellAllAmount()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1066) and [`buyAllAmount()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1111), the returned value `fill_amt` is assigned by casting itself into [`calcAmountAfterFee()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L578-L589). This does not make much sense since the fees has separately been paid by the takers and could possibly lead to miscalculation if the returned output is used elsewhere for accounting or other arithmetic operations. Consider looking into the codes and see whether or not the code line may need to be removed.

## Typo mistakes
[File: RubiconMarket.sol#L250](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L250)

```diff
-    // /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it
+    // /// @notice Modifier that checks the user to make sure they own the offer and it is valid before they attempt to cancel it
```
[File: Position.sol#L28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L28)

```diff
-        uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution
+        uint256 currentBathTokenAmount; // amount of bathTokenAsset in the moment of execution
```
## Incorrect use of returned variable name
[File: RubiconMarket.sol#L284-L286](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L284-L286)

```diff
-    function getRecipient(uint256 id) public view returns (address owner) {
+    function getRecipient(uint256 id) public view returns (address recipient) {
        return offers[id].recipient;
    }
```
Note: Comment the asociated V1 to V2 Changeset where possible.

## Hardcoded `isClosed()`
`isClosed()` has been hardcoded to `false`, making the market always open. There is no option to close it.

Consider making the function toggling as follows, and have the function [initialized](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700) in contract `RubiconMarket` where possible:

[File: RubiconMarket.sol#L620-L622](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L622)

```diff
-    function isClosed() public pure returns (bool closed) {
+    function isClosed(bool status) public pure returns (bool closed) {
-        return false;
+        closed = status;
    }
```
## Missing dust amount check when making new offer
The dust amount check is only implemented prior to creating a new offer when there is left over [`pay_amt`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1327) after a maker has taken up an offer in `_matcho()`.

Consider having the same check implemented too when a user is making a new offer via `make()`:

[File: RubiconMarket.sol#L733-L750](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733-L750)

```diff
    function make(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    ) public override returns (bytes32) {
        require(pay_amt >= _dust[address(pay_gem)]
        return
            bytes32(
                offer(
                    pay_amt,
                    pay_gem,
                    buy_amt,
                    buy_gem,
                    msg.sender,
                    msg.sender
                )
            );
    }
```
## Include descriptive variables in mappings
Where possible, implement mappings with descriptive variables included for better readability.

Here are some of the instances entailed:

[File: RubiconMarket.sol#L691-L695](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691-L695)
  
```solidity
    mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
    mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
    mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
    mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
    mapping(uint256 => uint256) public _near; //next unsorted offer id
```
Additionally, group them into a struct for better organization where possible.

## `synchronized()` over `require()`
In contract `RubiconMarket`, there are six instances where modifier [`synchronized()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269), a check and effect mutex, could have been used in the function visibility instead of just implementing `require(!locked)` that is devoid of re-entrancy protection if it is ever needed. 
 
[File: RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
834:        require(!locked, "Reentrancy attempt");

859:        require(!locked, "Reentrancy attempt");

874:        require(!locked, "Reentrancy attempt");

938:        require(!locked);

1034:        require(!locked);
```
## Use a more recent version of solidity
The protocol adopts version 0.8.9 & 0.8.17 on writing contracts. For better security, it is best practice to use the latest Solidity version, 0.8.19.

Security fix list in the versions can be found in the link below:

https://github.com/ethereum/solidity/blob/develop/Changelog.md

## Inadequate NatSpec
Solidity contracts can use a special form of comments, i.e., the Ethereum Natural Language Specification Format (NatSpec) to provide rich documentation for functions, return variables and more. Please visit the following link for further details:

https://docs.soliditylang.org/en/v0.8.16/natspec-format.html

Consider fully equipping all contracts with complete set of NatSpec to better facilitate users/developers interacting with the protocol's smart contracts.

## Unspecific compiler version pragma
For some source-units the compiler version pragma is very unspecific, i.e. ^0.8.9. While this often makes sense for libraries to allow them to be included with multiple different versions of an application, it may be a security risk for the actual application implementation itself. A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up actually checking a different EVM compilation that is ultimately deployed on the blockchain.

Avoid floating pragmas where possible. It is highly recommend pinning a concrete compiler version (latest without security issues) in at least the top-level “deployed” contracts to make it unambiguous which compiler version is being used. Rule of thumb: a flattened source-unit should have at least one non-floating concrete solidity compiler version pragma.

## Modularity on import usages
For cleaner Solidity code in conjunction with the rule of modularity and modular programming, use named imports with curly braces instead of adopting the global import approach.

For example, the import instances below could be refactored conforming to the suggested standards as follows:

[File: FeeWrapper.sol#L4-L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L4-L5)

```diff
- import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
+ import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
- import "./RubiconRouter.sol";
+ import {RubiconRouter} from "./RubiconRouter.sol";
```
## Non-compliant contract layout with Solidity's Style Guide
According to Solidity's Style Guide below:

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

In order to help readers identify which functions they can call, and find the constructor and fallback definitions more easily, functions should be grouped according to their visibility and ordered in the following manner:

constructor, receive function (if exists), fallback function (if exists), external, public, internal, private

And, within a grouping, place the `view` and `pure` functions last.

Additionally, inside each contract, library or interface, use the following order:

type declarations, state variables, events, modifiers, functions

Consider adhering to the above guidelines for all contract instances entailed.

## Gas griefing/theft is possible on unsafe external call
`return` data (bool success,) has to be stored due to EVM architecture, if in a usage like below, ‘out’ and ‘outsize’ values are given (0,0). Thus, this storage disappears and may come from external contracts a possible gas grieving/theft problem is avoided as denoted in the link below:

https://twitter.com/pashovkrum/status/1607024043718316032?t=xs30iD6ORWtE2bTTYsCFIQ&s=19

Here are the two instances entailed:

[File: FeeWrapper.sol#L82-L84](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L82-L84)

```solidity
        (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
            bytes.concat(_params.selector, _params.args)
        );
```
[File: FeeWrapper.sol#L118-L121](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L118-L121)

```solidity
        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
		require(OK, "ETH transfer failed");
        _msgValue = msg.value - _feeAmount;
    }
```
## Lines too long
Lines in source code are typically limited to 80 characters, but it’s reasonable to stretch beyond this limit when need be as monitor screens theses days are comparatively larger. Considering the files will most likely reside in GitHub that will have a scroll bar automatically kick in when the length is over 164 characters, all code lines and comments should be split when/before hitting this length. Keep line width to max 120 characters for better readability where possible.

Here are some of the instances entailed:

[File: BathBuddy.sol#L28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L28)
[File: BathBuddy.sol#L31](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L31)

## Gas griefing/theft is possible on unsafe external call
`return` data (bool success,) has to be stored due to EVM architecture, if in a usage like below, ‘out’ and ‘outsize’ values are given (0,0). Thus, this storage disappears and may come from external contracts a possible gas grieving/theft problem is avoided as denoted in the link below:

https://twitter.com/pashovkrum/status/1607024043718316032?t=xs30iD6ORWtE2bTTYsCFIQ&s=19

Here are the instances entailed:

[File: FeeWrapper.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol)

```solidity
66:        (bool _OK, bytes memory _data) = _params.target.call(
67:            bytes.concat(_params.selector, _params.args)
68:        );

82:        (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
83:            bytes.concat(_params.selector, _params.args)
84:        );

118:        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
119:		require(OK, "ETH transfer failed");
```
## V1 bathTokens might not be wiped clean
V2 bathTokens sent into V2Migrator will be all swept to the next mighrator. The same approach adopting contract balance instead of pre post balance difference should be implemented on V1 bathTokens too. Consider refactoring the affected code block as follows. Otherwise, the donated V1 bathTokens will never be able to get out of the contract:

[File: V2Migrator.sol#L39-L57](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L39-L57)

```diff
        //////////////// V1 WITHDRAWAL ////////////////
        uint256 bathBalance = bathTokenV1.balanceOf(msg.sender);
        require(bathBalance > 0, "migrate: ZERO AMOUNT");

        /// @dev approve first
        bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);

        // withdraw all tokens from the pool
-        uint256 amountWithdrawn = bathTokenV1.withdraw(bathBalance);
+        uint256 amountWithdrawn = bathTokenV1.withdraw(bathTokenV1.balanceOf(address(this)));

        //////////////// V2 DEPOSIT ////////////////
        IERC20 underlying = bathTokenV1.underlyingToken();
        address bathTokenV2 = v1ToV2Pools[address(bathTokenV1)];

        underlying.approve(bathTokenV2, amountWithdrawn);
        require(
            CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
            "migrate: MINT FAILED"
        );
```
## Incorrect concatenation in _bathify()
[File: BathHouseV2.sol#L137-L149](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L137-L149)

```diff
    function _bathify(
        address _underlying
    )
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)
    {
        require(_underlying != address(0), "_bathify: ADDRESS ZERO");

-        _name = string.concat("bath", ERC20(_underlying).symbol());
+        _name = string.concat("bath", ERC20(_underlying).name());
-        _symbol = string.concat(_name, "v2");
-        _symbol = string.concat(ERC20(_underlying).symbol(), "v2");
        _decimals = ERC20(_underlying).decimals();
    }
```