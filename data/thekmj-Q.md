| Number |Details|
|:--:|:-------|
|[L-01]| Incorrect practice on CToken metadata |
|[L-02]| `Position`: Suggest supporting adding margin on non-specific positions |
|[L-03]| `V2Migrator.sol`: Suggest batching migrating multiple pools at a time |
|[L-04]| Leveraged positions in `Positions` are calculated without taking fees into account |
|[L-05]| Leveraged positions in `Positions` can swing without limit, depending on orderbook available offers (lack of slippage control) |
|[N-02]| Incorrect Compound `initialExchangeRateMantissa_` on Forge testing |
|[N-03]| Typo |

## [L-01] Incorrect practice on CToken metadata

In `BathHouseV2`, the BathToken/CToken is initialized with the same decimals as the underlying token:
```solidity
// get bathToken metadata that semantically reflects underlying ERC20
(string memory name, string memory symbol, uint8 decimals) = _bathify(
    underlying
);

// BathTokenDelegator
address bathToken = address(
    new CErc20Delegator(
        underlying,
        comptroller,
        interestRateModel,
        initialExchangeRateMantissa,
        name,
        symbol,
        decimals,
        payable(proxyAdmin),
        implementation,
        becomeImplementationData
    )
);
```

```solidity
function _bathify(
    address _underlying
)
    internal
    view
    returns (string memory _name, string memory _symbol, uint8 _decimals)
{
    require(_underlying != address(0), "_bathify: ADDRESS ZERO");

    _name = string.concat("bath", ERC20(_underlying).symbol());
    _symbol = string.concat(_name, "v2");
    _decimals = ERC20(_underlying).decimals();
}
```

However, this is not the correct practice for deploying CToken markets. The usual practice is to set the CToken decimal to exactly $8$, and the `initialExchangeRateMantissa` to be $2e8 * 10^{decimals}$, where $decimals$ is the decimal of the underlying token. 

[CToken contracts](https://docs.compound.finance/v2/) info for reference.

## [L-02] `Position`: Suggest supporting adding margin on non-specific positions

Since an instance of a `Position` contract is cross-margin by nature (it is mentioned on the README), it is reasonable to think a user will want to add margins as assets they do not have a position in. 

We suggest adding functionality that allows entering market in any market and adding margin in said asset (e.g. a user can open a ETH/USDC long or short position, but still add WBTC as margin when they need to).

## [L-03] `V2Migrator`: Suggest batching migrating multiple pools at a time

Currently, `V2Migrator` contract only allows for migrating one pool per transaction. That means to migrate one pool, a user has to sign two transactions: To approve the pool, and to actually migrate.

Compared to migrating by themselves, a user should sign three transactions: withdraw from V1, approve V2, deposit to V2. Meaning the migrator only saves one tx by batching approve V2 and deposit to V2.

Then there is not much point in using the migrator to save one tx per pool, because the user has to approve the V2 pool again anyway, unless the user only plans on withdrawing afterwards.

We suggest allowing batching multiple pool migrations in one tx.

## [L-04] Leveraged positions in `Positions` are calculated without taking fees into account

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526-L583

When creating a new leveraged position, the function `_borrowLimit()` calculates the number of loops needed. However, the function assumes the `quote` is able to sell for the exact amount of `asset` at market price, **without taking into account Rubicon Market fee**. Hence, the calculation will come with higher inaccuracy than intended.

This will not completely address the inaccuracy problem, as the result also depends on the orderbook status itself. However we think that as this addresses one of the sponsor's concerns in the contest details:

> Ensuring that users can easily perform levered long/short operations, with accuracy, using the combination of the new `Pools v2` - COMP v2 fork - system and `RubiconMarket`

Then the importance of addressing the calculation flaw is justified.

## [L-05] Leveraged positions in `Positions` can swing without limit, depending on orderbook available offers (lack of slippage control)

This finding extends upon **[L-04]**, but highlights a different issue.

As was mentioned in [L-04], the resulting leverage itself depends on the orderbook status, as buying `asset` from `quote` is done using the RubiconMarket market order. Hence, the actual amount of `asset` bought will be different than calculated, and the resulting leverage will be different.

This is a risk that is to be associated with making market orders. However, the issue we wish to highlight here is that there is no limit to how far down below or above the actual leverage may swing (the only theoretical limits are within 1x to infinity).

The impacts are as follow:
- Loss of leverage: the user may profit less than expected.
- Gain of leverage: for the same margin amount, a higher leverage will expose the user at more risk of liquidation.

The more liquid the orderbook is, the less impact there is. It is also worth noting that, there is a MEV attack pointed out in another of our submission, that has a side effect of also swinging the user's leverage further.

While this is a risk that is to be associated with market trading, due to its potential impact, we believe this should be acknowledged.

**Recommended mitigation steps:**
- Consider adding a slippage control to the `Positions` itself, providing a desired leverage, as well as a minimum and maximum acceptable leverage, to account for extreme market conditions, as well as potential MEVs.
- Consider warning the users about additional risks of trading beforehand (however this is already a must for margin traders in general).

## [N-01] Incorrect Compound `initialExchangeRateMantissa_` on Forge testing

In the following line of the testing system:

- https://github.com/code-423n4/2023-04-rubicon/blob/main/test/foundry-tests/ProtocolDeployment.t.sol#L71

The `initialExchangeRateMantissa_` of `bathTUSDC` is incorrectly set as `2e15` on a stablecoin with $6$ decimals. The correct value should be `2e14`.

## [N-02] Typo

- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L593
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L597

`INVLAID` $\rightarrow$ `INVALID`

