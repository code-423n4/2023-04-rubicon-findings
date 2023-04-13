# Report


## Gas Optimizations

### GAS-1 RubiconMarket_#326_Redundant check in `if` statement 
There is a redundant check of `quantity` in the line [#326](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L326). If `quantity` equals `0`, the `spend` will be `0` too: 
[`uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L319). I suggest removing the check for `quantity`.

### GAS-2 RubiconMarket_#1444_Dubble check `isOfferSorted` 
There are the second check `isOfferSorted` in the [line #1444](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1444) in the `_hide` function and in the [line #1414](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1414) in the `_unsort` function after this parametr was firstly checked in the functions with, which calls these functions: [`cancel`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L876-L880) and [`_buys`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1186-L1191). I suggest removing the second checks `isOfferSorted`.

### GAS-3 BathHouseV2_#144_Dubble check of `underlying` 
There is the second check of the `underlying` parametr in the [line #144](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L144) in the `_bathify` function after this parametr was firstly checked in the [line #73](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L73) in the `createBathToken` function - the only function which calls `_bathify`. I suggest removing the second check.

