# Event not emitted in fee logic for taker trades

In `RubiconMarket.sol` the fee event is not emitted when a fee is taken.

```solidity
    uint256 fee = mul(spend, feeBPS) / 100_000;
    require(
        _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
        "Insufficient funds to cover fee"
    );
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L339-L342

# Matching cannot be disabled in `RubiconMarket`

There is functionality in the contract that handles `matchingEnabled` conditionals. However `matchingEnabled` is hardcoded to be `true`.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L714

# Buys cannot be disabled in `RubiconMarket`

There is functionality in the contract that handles `buyEnabled` conditionals. However `buyEnabled` is hardcoded to be `true`.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L715