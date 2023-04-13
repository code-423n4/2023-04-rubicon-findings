# Report

## Low Issues

### L-1 RubiconMarket_#1020_Incorrect answer possible
The [`isOfferSorted` function](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1020-L1026) returns `true` for offer `id`s after executing the [`_unsort` function](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1405-L1430) to them. Possible use in attack has not been determined. I suggest checking `_rank[id].delb == 0` in the `isOfferSorted` function if it is important.

### L-2 RubiconMarket_#297_Useless function
There is the [`bump` function](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297-L310), which does nothing. I suggest removing the function.

### L-3 RubiconMarket_`batchOffer` and `batchRequote` don't return offers id's 
The [`batchOffer`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L887-L907) and [`batchRequote`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933) could return arrays of `offer`s `id`s but they don't. I suggest adding this functionality. 

### L-4 RubiconMarket_`batchRequote` doesn't have check for arrays length 
The [`batchRequote` function](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933) doesn't have check for arrays length like the check from the [`batchOffer`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L893-L898). I suggest adding this check or calling functions `batchCancel` and `batchOffer` in the `batchRequote` function.

### L-5 V2Migrator_`v1ToV2Pools` mapping can contain unmatched addresses or zero addresses 
The [`V2Migrator` contract constractor](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30-L35) doesn't check parameters for the `v1ToV2Pools` mapping. It is not possible to use unmatched addresses for asset steal, but migration in `bathTokenV2` tokens will be also blocked. I suggest checking that `bathTokenV1` and `bathTokenV2` `underlying` token addresses match.