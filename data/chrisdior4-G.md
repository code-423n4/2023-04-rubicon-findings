## Gas Optimization

| G-O    |Issue|
|:------:|:----|
| [G&#x2011;01] | Prefer Solidity Custom Errors over require statements with strings | 2 |
| [G&#x2011;02] | Use x != 0 instead of x > 0 for uint types | 6 |
| [G&#x2011;03] | x = x - y costs less gas than x -= y, same for addition | 11 |
| [G&#x2011;04] | Require()/Revert() strings longer than 32 bytes cost extra gas | 19 |
| [G&#x2011;05] | Using `storage` instead of `memory` for structs/arrays saves gas | 31 |
| [G&#x2011;06] | Cache Array Length Outside of Loop | 2 |
| [G&#x2011;07] | ++i costs 5 gas less than i++ | 1 |
| [G&#x2011;08] | Splitting require() statements that use && saves gas | 1 |

Total: 8 issues

### [G-01] Prefer Solidity Custom Errors over require statements with strings

Using Solidity Custom Errors has the benefits of less gas spent in reverted transactions, better interoperability of the protocol as clients of it can catch the errors easily on-chain, as well as you can give descriptive names of the errors without having a bigger bytecode or transaction gas spending, which will result in a better UX as well. 

Remove all require statements and use Custom Errors instead.

### [G-02] Use x != 0 instead of x > 0 for uint types

The != operator costs less gas than > and for uint types you can use it to check for non-zero values to save gas

There are 17 instances of this issue. Here are some examples:

```solidity
return offers[id].timestamp > 0;
```
```solidity
if (pay_amt > 0) {
```
```solidity
if (buy_amt > 0) {
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L277
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1133
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1166


### [G-03] x = x - y costs less gas than x -= y, same for addition  

```solidity
_amount -= mul(amount, feeBPS) / 100_000;
```

```solidity
_amount -= mul(amount, makerFee()) / 100_000;
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586


### [G-04] Require()/Revert() strings longer than 32 bytes cost extra gas

```solidity
require(
        _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
        "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
```

```solidity
require(
        _offer.pay_gem.transfer(msg.sender, quantity),
        "_offer.pay_gem.transfer(msg.sender, quantity) failed"
        );
```

```solidity
require(
            isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );
        _;
```


Lines of code: 

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L374
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L379
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L721



### [G-05] Using `storage` instead of `memory` for structs/arrays saves gas 

 When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct.

```solidity
OfferInfo memory _offer = offers[id];
```
```solidity
 OfferInfo memory info;
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L291
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L527


### [G-06] Cache Array Length Outside of Loop

Caching the array length outside a loop saves reading it on each iteration, as long as the array's length is not changed during the loop. The project has many instances of this issue. Cache the array outside the loop. Couple of examples:

```solidity
for (uint i = 0; i < payAmts.length; i++) {
```
```solidity
for (uint i = 0; i < ids.length; i++) {
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L899
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L911
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L924


### [G-07] ++i costs 5 gas less than i++

```solidity
for (uint i = 0; i < payAmts.length; i++) {
```
```solidity
for (uint i = 0; i < ids.length; i++) {
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L899
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L911
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L924

### [G-08] Splitting require() statements that use && saves gas

See this issue which describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by 3 gas

```solidity
require(
            !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
				);
```
```solidity
require(
            (msg.sender == getOwner(id)) ||
                isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );
```
```solidity
require(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );
```

Lines of code:

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L940
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612
- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L893
