## Summary

|        | Issues                                                                                                     | Contexts |
| ------ | ---------------------------------------------------------------------------------------------------------- | -------- |
| [G-01] | S`<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables                                                                      | 5       |
| [G-02] | Setting the constructor to `payable`                                                           | 2        |
| [G-03] | Expression for constant values such as a call to `keccak256()`, should use immutable rather than constant                                                                         | 1       |
| [G-04] | Use nested if and, avoid multiple check combinations | 4       |
| [G-05] | Splitting `require()` statements that use `&&` saves gas                                                      | 12       |
| [G-06] | Upgrade Solidity’s optimizer                                                       | 1       |
| [G-07] | Use 1 and 2 instead of `true` and `false`                                                       | 1       |
| [G-08] | Delete unused variables                                                       | 2       |
| [G-09] | Change function visibility from public to external                                                       | 7       |
| [G-10] | Unused events                                                       | 3       |
| [G-11] | Redundant imports                                                       | 3       |

## [G-01] `<x> += <y>` Costs More Gas Than `<x> = <x> + <y>` For State Variables

    3 results - 1 file

    contracts/utilities/poolsUtility/Position.sol:
    184:             vars.currentBathTokenAmount += _borrowLoop(

    431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;

    560:             _assetAmount += _loopBorrowed;

    2 results - 1 file

    contracts/RubiconMarket.sol:

    583:         _amount -= mul(amount, feeBPS) / 100_000;
    584  
    586:             _amount -= mul(amount, makerFee()) / 100_000;

## [G-02] Setting the constructor to `payable`

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor `payable` eliminates the need for an initial check of `msg.value == 0` and saves 13 gas on deployment with no security risks.

    2 results - 2 files

    contracts/V2Migrator.sol:
    30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

    contracts/utilities/poolsUtility/Position.sol:
    54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

## [G-03] Expression for constant values such as a call to `keccak256()`, should use immutable rather than constant

    1 result - 1 file

    contracts/RubiconMarket.sol:
    232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

## [G-04] Use nested if and, avoid multiple check combinations

Using nested is cheaper than using && multiple check combinations.

    6 results - 2 files

    contracts/RubiconMarket.sol:
    349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
    1198:             isActive(id) &&
    1325:             t_buy_amt > 0 &&
    1326:             t_pay_amt > 0 &&

    contracts/utilities/poolsUtility/Position.sol:
    176:             if (i.add(1) == vars.limit && vars.lastBorrow != 0) {
    392:             IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&

## [G-05] Splitting `require()` statements that use `&&` saves gas

Instead of using the && operator in a single require statement to check multiple conditions,using multiple require statements with 1 condition per require statement will save 8 GAS per &&.
The gas difference would only be realized if the revert condition is realized(met).

    12 results - 3 files

    contracts/RubiconMarket.sol:
    255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
    615:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
    725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
    894:             payAmts.length == payGems.length &&
    895:                 payAmts.length == buyAmts.length &&
    941:             !isActive(id) &&
    942:                 _rank[id].delb != 0 &&
    1413:             _rank[id].delb == 0 && //assert id is in the sorted list

    contracts/periphery/BathBuddy.sol:
    96:             msg.sender == myBathTokenBuddy &&
    97:                 msg.sender != address(0) &&

    contracts/utilities/poolsUtility/Position.sol:
    592:                 _leverage > _wad && _leverage <= _leverageMax,
    596:                 _leverage >= _wad && _leverage <= _leverageMax,

## [G-06] Upgrade Solidity’s optimizer

Make sure Solidity’s optimizer is enabled. It reduces gas costs. If you want to gas optimize for contract deployment (costs less to deploy a contract) then set the Solidity optimizer at a low number. If you want to optimize for run-time gas costs (when functions are called on a contract) then set the optimizer to a high number.

Set the optimization value higher than 800 in your hardhat.config.ts file as it is currently set to 5 which is too low.

        version: "0.8.17",
        settings: {
          optimizer: {
            enabled: true,
            runs: 5,
          },

## [G-07] Use 1 and 2 instead of `true` and `false`

Using true and false will trigger gas-refunds, which after London are 1/5 of what they used to be, meaning using 1 and 2 (keeping the slot non-zero), will cost 5k per change (5k + 5k) vs 20k + 5k, saving you 15k gas per function which uses the `synchronized` modifier.

    modifier synchronized() {
        require(!locked);
        locked = true;
        _;
        locked = false;
    }

The modifier is used in 3 key functions - `buy`, `cancel` and `offer`. See [solmate](https://github.com/transmissions11/solmate/blob/main/src/utils/ReentrancyGuard.sol) implementation or directly inherit their library and use the `nonReentrant` as it is gas optimized and well tested.

## [G-08] Delete unused variables

The below variables are stored in storage and are not used: 

    /// @dev unused deprecated variable for applying a token distribution on top of a trade
    bool public AqueductDistributionLive;
    /// @dev unused deprecated variable for applying a token distribution of this token on top of a trade
    address public AqueductAddress;

As we can see from the comments the variables are deprecated and not used anymore. Initializing a storage variable costs 20,000 gas which also applies for addresses and booleans.

## [G-09] Change function visibility from public to external

Various functions across contracts are never called from within contracts but yet declared public. Their visibility can be made external to save gas. As described [here](https://mudit.blog/solidity-gas-optimization-tips/) - "If your function is only called externally, then you should explicitly mark it as external. External function’s parameters are not copied into memory but are read from calldata directly. This small optimization in your solidity code can save you a lot of gas when the function input parameters are huge."

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L288-L290
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L998-L1001
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L574
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L624
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L733-L738
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L1010
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L1016

## [G-10] Unused events

Unused events increase contract size at deployment and can be removed.

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/periphery/BathBuddy.sol#L262
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/periphery/BathBuddy.sol#L263
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/periphery/BathBuddy.sol#L266

## [G-11] Redundant imports

The `Position` and `BathBuddy` contracts import the `SafeMath` library which is not necessary as the solidity compiler version is above 0.8.0. This version prevents arithmetic operations from underflow/overflow. Also, there is `DSMath` contract inside `RubiconMarket` which can also me removed because of the same reason.

    /// @notice DSMath library for safe math without integer overflow/underflow
    contract DSMath {