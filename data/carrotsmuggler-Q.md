# 1. Oracles must return in 18 decimals

The oracle is not yet implemented, thus this is classified as low for now. The code expects the oracle to return the price in 18 decimals. This is true for chainlink oracles reporting prices in ETH. But if chainlink oracles reporting prices in USD are used, they come in 6 decimals and must be scaled up to 18 decimals.

The reason this is necessary is because the code makes an assumption [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317) in Position.sol.
```solidity
_max = (_liq.mul(10 ** 18)).div(_price);
```
This function calculates the maximum amount that can be borrowed. _liq is returned in 18 decimals. It is then raised by 18 decimals and divided by price. If the price reported is not in 18 decimals, it will lead to a decimal error and return very high values of max borrow which would revert since the contract actually cannot borrow that much when actually executing the transaction.

Thus care must be taken to make sure all oracles return 18 decimals.

# 2. Position.sol approval handling

The contract Position.sol uses the `safeApprove` function [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L358).
The `safeApprove` function has been deprecated in favour of `safeIncreaseAllowance` and its use should be discouraged.

# 3.  Fee must be >0 for some tokens
In the Rubicon market contract, the fee is calculated and sent to the `feeTo` address during the `buy()` function execution [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338-L342) through the following lines.
```solidity
        uint256 fee = mul(spend, feeBPS) / 100_000;
        require(
            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
            "Insufficient funds to cover fee"
        );
```
Some ERC20 tokens however do not support 0 value transfers. Thus is the fee is calculated to be 0, this function call can revert. Tokens such as these are rare and not in wide circulation, thus this issue is being classified as low, but if the intention of Rubicon is to open a general purpose orderbook for ALL tokens, this issue would be of higher severity.

The fee calculated can be 0 due to multiple reasons: small exchange amounts such that dividing by 10000 gives 0, or the feeBPS is set to 0. Thus if the `feeBPS` is ever set to 0 as a promotional offer, it can brick these types of tokens which cannot do zero value transfers.

The recommended mitigation is to use a similar logic as present in `makerFee` [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L345), where the fee is first checked to be greater than 0, and only then the transfer statement is executed, and is skipped otherwise.
