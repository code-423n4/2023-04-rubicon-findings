| Number |Details|
|:--:|:-------|
|[L-01]| Incorrect practice on CToken metadata |
|[L-02]| `Position`: Suggest supporting adding margin on non-specific positions |
|[L-03]| `V2Migrator.sol`: Suggest batching migrating multiple pools at a time |
|[N-01]| Incorrect Compound `initialExchangeRateMantissa_` on Forge testing |
|[N-02]| Typo |

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

## [N-01] Incorrect Compound `initialExchangeRateMantissa_` on Forge testing

In the following line of the testing system:

- https://github.com/code-423n4/2023-04-rubicon/blob/main/test/foundry-tests/ProtocolDeployment.t.sol#L71

The `initialExchangeRateMantissa_` of `bathTUSDC` is incorrectly set as `2e15` on a stablecoin with $6$ decimals. The correct value should be `2e14`.

## [N-02] Typo

- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L593
- https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L597

`INVLAID` $\rightarrow$ `INVALID`
