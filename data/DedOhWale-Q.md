## [N-01] `buyAllAmount` function in `RubiconMarket.sol` returns a `fill_amount` that may be greater than the `max_fill_amount` with fees.

If a user asks for a max_fill_amount in transaction, they will actually have a fill_amount that is higher considering the fees.

### Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1107-L1111

### Recommendation: 

Move the `require` underneath the `calcAmountAfterFee`.

## [N-02] `buy` function in `RubiconMarket.sol` could potentially lose funds. 

If `_offer.owner == address(0)` and `getRecipient(d) == address(0)`, then we will send funds to the 0 address.

### Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349-L363
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L374-L382

### Recommendation: 

Check with a require: ‘require(_offer.recipient != address(0))’

## [N-03] Related to N-02, `offer` within `RubiconMarket.sol` does not do a 0 address check for `recipient` & `owner`.

We should check to make sure that we are not registering the 0 address as owner, in addition, it is public so any user may lose funds this way.

## [N-04] ‘matchingEnabled’ and ‘buyEnabled’ have no setter functions. 

As there is logic embedded that uses the false parameter, you should either get rid of the logic or have the ability to change those parameters. Make sure this is locked by an owner so that users cannot change the functionality of the contract.

###Recommendation: 

Create a setMatchingEnabled & setBuyEnabled function.

```
function enableMatching() public onlyOwner{
        matchingEnabled = true;
    }

function enableBuying() public onlyOwner{
        buyingEnabled = true;
    }
```