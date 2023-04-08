## RubiconMarket

1- Consider simplifying the code in `_findpos` to make it more readable by removing `else` from L1236 and L1251
2- Comment in L1270 looks redundant
3- There are 2 references for `close_time` in the comments but no actual variable has this name

```console
contracts/RubiconMarket.sol
592:/// @dev When the close_time has been reached, offers can only be cancelled (offer and buy will throw).
596:    /// @dev After close_time has been reached, no new offers are allowed.
```

4- `public bool stopped` in `ExpiringMarket` does not look like it is used anywhere in the contracts other than its setter function. Obviously it cannot be removed, otherwise it will cause a storage collision since it's already in place since the last upgrade but you may at least want to consider removing `stop()` since setting `stopped` to `true` does not have any meaning at the moment and the function can be re-introduced in the future once `stopped` is utilized elsewhere in the contract.
5- `isClosed` is used in 4 different places but it only returns `false` so it is a redundant check

```console
contracts/RubiconMarket.sol
598:        require(!isClosed());
605:        require(!isClosed());
614:                isClosed() ||
722:            isClosed() ||
```

6- Switch visibility for deprecated values to `private` and potentially prefix with `DEPRECATED_` to make it clearer that these values are deprecated and not to be used in code. Current deprecated values are `AqueductDistributionLive` and `AqueductAddress`
7- Fee calculations should use `10_000` instead of `100_000` to perform divisions and multiplications in order for the fee basis points values (`feeBPS` and `makerFee()`) to be semantically correct.

## Migrator

The constructor of the migrator could check that the `underlyingToken` of v1 and v2 bath tokens match to ensure the right pairs have been instantiated.

## BathHouseV2

Remove `admin` in favor of inheriting from `DSAuth` in order to enable rotating the admin w/o the need for a contract upgade.

## Position

1- Typos in `_leverageCheck` at L593 and L597 (`INVLAID` -> `INVALID`)
2- `delete positions[_positionId]` in L422 only deletes the map index and not the actual position data. The way `_positionId` currently work in the contract this is not an issue since it's monotonically increasing but you may want to consider properly cleaning up all position data from storage.
3- Opening levered positions where both `quote` and `asset` are the same asset is invalid but currently there is a revert that has no string and it's not really clear from where does it come from (potentially from `_rubiconswap`). It may be best to require that `quote != asset` in both `buyAllAmountWithLeverage` and `sellAllAmountWithLeverage` to make it clearer to clients that this is an invalid combo.

## All

For upgradeable contracts, it's a common approach to disable running initializers in implementations to avoid social attacks where an attacker can trick a user to use the implementation instead of the proxy. OpenZeppelin has [`_disableInitializers()`](https://docs.openzeppelin.com/contracts/4.x/api/proxy#Initializable-_disableInitializers--) but since initialization in the Rubicon contracts is done via custom code, you will need to re-implement `_disableInitializers()` in your own library and pull in all upgradeable contracts.
