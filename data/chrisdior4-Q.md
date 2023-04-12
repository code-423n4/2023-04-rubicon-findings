# QA report

## Low Risk
| L-N    |Issue|
|:------:|:----|
| [L&#x2011;01] | Open TODO in the code | 9 |
| [L&#x2011;02] | Use of one step ownership transfer in `RubiconMarket.sol` | 1 |
| [L&#x2011;03] | Missing array length check | 2 |
| [L&#x2011;04] | 4 different functions with the same name | 2 |
| [L&#x2011;05] | No upper bound for  `setFeeBPS` function | 2 |
| [L&#x2011;06] | Missing zero address check | 2 |

Total: 6 issues

## Non-critical

| N-N    |Issue|
|:------:|:----|
| [N&#x2011;01] | Constants should be defined rather than using magic numbers  | 2 |
| [N&#x2011;02] | 10 ** 9 can be changed to 1e9 for readability | 6 |
| [N&#x2011;03] | Using SafeMath when compiler is ^0.8.0 | 11 |
| [N&#x2011;04] | Missing event emission on setter functions | 19 |
| [N&#x2011;05] | NatSpec docs are missing | 31 |
| [N&#x2011;06] | Solidity safe pragma best practices are not used | 2 |
| [N&#x2011;07] | Typos in the comments | 1 |
| [N&#x2011;08] | Contract is not inheriting its interface | 1 |
| [N&#x2011;09] | Interchangeable usage of uint and uint256 in `RubiconMarket.sol` | 1 |
| [N&#x2011;10] | Missing friendly revert strings | 1 |
| [N&#x2011;11] | Unused event | 1 |

Total: 11 issues

## Low Risk

### [L-01]  Open TODO in the code

There are 11 open TODOs in `RubiconMarket.sol` this implies changes that might not be audited. Resolve them or remove them. 

### [L-02] Use of one step ownership transfer in `RubiconMarket.sol`

It is a best practice to use two-step ownership transfer pattern, meaning ownership transfer gets to a "pending" state and the new owner should claim his new rights, otherwise the old owner still has control of the contract. Consider using OpenZeppelin's Ownable2Step contract.

```solidity
address public owner;

    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25

### [L-03]  Missing array length check

In `batchRequote()` there is missing check for the length of hte array that the user will input as parameters. Consider adding a check like the existing one in `batchOffer()`.

```solidity
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

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917


### [L-04] 4 different functions with the same name

File: `RubiconMarket.sol`

There are 4 different functions named `offer()`. This creates a lot of confusion, and can be problematic. Consider giving them different names.  Examples:

```solidity
function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint pos, //position to insert offer, 0 should be used if unknown
        bool rounding
    ) external can_offer returns (uint256)
```

```solidity

    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        bool matching, //match "close enough" orders?
        address owner, // owner of the offer
        address recipient // recipient of the offer's fill
    ) public can_offer returns (uint256) {
```

Lines of code:

761 - 852

### [L-05] No upper bound for  `setFeeBPS` function

The function has the `auth` modifier and it is supposed to the person calling this method will be trusted. But nevertheless if the caller makes a human mistake or is compromised, he will be able to set the `FeeBPS` to a huge amount and this can be problematic. Add a `maxFeeBPS` variable in the function and require that the `_newFeeBPS` is lower or equal to the max one.

```solidity
   function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466


### [L-06] Missing zero address checks

In the following function, a zero address check is missing for `owner` and `recipient` which values are then assigned in the struct `OfferInfo`:

```solidity
 function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L511

Consider adding a zero address check for bot `owner` and `recipient`.


## Non-critical


### \[NC-01\] Constants should be defined rather than using magic numbers 

File: `RubiconMarket.sol`

```solidity
uint256 fee = mul(spend, feeBPS) / 100_000;
```

```solidity
_rank[id].delb < block.number - 10
```

Lines of code: 

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L346
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586

### [NC-02] 10 ** 9 can be changed to 1e9 for readability 

File: `RubiconMarket.sol`

```solidity
mul(pay_amt, 10 ** 9)
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1057
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1059
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1099
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1101
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1142
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1144
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1175
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1177


### [NC-03] Using SafeMath when compiler is ^0.8.0

There is no need to use SafeMath in this case  when compiler is ^0.8.0 because it has built-in under/overflow checks.

```solidity
/// @notice DSMath library for safe math without integer overflow/underflow
contract DSMath {
```

Lines of code:
-  https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L45

### [NC-04] Missing event emission on setter functions

Some setter functions in the contract do not emit an event which might not be good for off-chain monitoring. Emit an event on state change.

```solidity
    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }

    function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }

    function getFeeTo() external view returns (address) {
        return feeTo;
    }
```

Lines of code:
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1476
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1482


### [NC-05]  NatSpec docs are missing

File: `RubiconMarket.sol`

In almost all methods the NatSpec documentation is incomplete as it is missing parameters and return variables in it. NatSpec documentation is essential for better understanding of the code by developers and auditors and is strongly recommended. Please refer to the [NatSpec format](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html) and follow the guidelines outlined there.

### [NC-06] Solidity safe pragma best practices are not used

Always use a stable pragma to be certain that you deterministically compile the Solidity code to the same bytecode every time. All the contracts are currently using floatable 0.8.9 version.

### [NC-07] Typos in the comments

File: `RubiconMarket.sol`

`multuple` -> `multiple`

`acumulator` -> `accumulator`


### [NC-08] Contract is not inheriting its interface
 
`RubiconMarket.sol` is not inheriting its interface `IRubiconMarket.sol` while this is a best practice for the contract implementations to inherit their interface definition. Doing so would improve the contract's clarity, and force the implementation to comply with the defined interface. Also do not forget to include the `override` keyword whenever you use a method inherited from the interface.

### [NC-09] Interchangeable usage of uint and uint256 in `RubiconMarket.sol` 

Consider using only one approach throughout the codebase, e.g. only uint or only uint256.

### [NC-10] Missing friendly revert strings

Here, a friendly message should exist for users to understand what went wrong:

```solidity
 require(cancel(uint256(id)));
```

```solidity
require(uint128(pay_amt) == pay_amt);
require(uint128(buy_amt) == buy_amt);
require(pay_amt > 0);
require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
require(buy_amt > 0);
require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
require(pay_gem != buy_gem);
```

```solidity
require(buy(uint256(id), maxTakeAmount));
```

```solidity
require(amount > 0);
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L488
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L519
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L520
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L521
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L522
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L523
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L524
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L525
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L565
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L581

### [NC-11]  Unused event

The event `LogUnsortedOffer` is not emitted so it can be removed.

Lines of code:
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L664