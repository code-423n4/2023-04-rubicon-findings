# Rubicon QA report

# Low Issues

## L-01 `SimpleMarket.can_offer()` modifier does not provide any  layer of protection
whatsoever
### Proof of Concept

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260-L262

### Recommendation

Advisably complete the modifier's code block or remove it completely, and if this is intended behaviour (i.e meant to be inherited), include it in the natspec comments cause the modifier is used in the `SimpleMarket.offer()` function, but doesn't inherently provide any layer of protection

## L-02 Missing zero address checks

### Proof of Concept

`DSAuth.setOwner()` is a function for a single stepped address change if it mistakenly gets set to the zero address there's no way of retrieving the auth modifier functionality

[DSAuth.setOwner()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)

### Recommendation

Lack of zero address checks procedure for critical operations leaves them error-prone. Consider adding zero address checks on the critical functions.

## L-03 Lack of events emission

### Proof of Concept

the `bump()` function has the `LogBump()` event commented out and no replacement is made for this event, which means that after the function is executed no events are emitted

### Recommendation

Uncomment the event or if this is intended behaviour include it in the comments

## L-04 Inconsistency with the bool var `matchingEnabled`

### Proof of Concept

According to the natspec comment of the `RubiconMarket.offer()` function, matching is perma-enabled, which is why the conditional check in [`RubiconMarket.offer()`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L838) was commented out, but in multiple other functions this is not the case and the bool var is queried, which pops up the question of why the incosistency?

### Recommendation

Querying of the `matchingEnabled` bool should be consistent through out code

## L-05 No zero address checks in `BathHouseV2.initialize()`

### Proof of Concept

[`BathHouseV2.initialize()`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32-L39) as the name suggests can only be called once and if proxyAdmin address is set to a wrong address (or the zeroth address) it can't be called again.

### Recommendation

Requiring that the address passed is not 0x0 should solve this

## L-06 The `Position.sol` constructor lacks zero address checks

### Proof of Concept

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54-L59

### Recommendation

Add zero address checks

## L-07 Usage of `safeApprove()`

### Proof of Concept

`safeApprove()` function has been deprecated by the OpenZeppelin's team due to it's similar approve like issues and also the fact that it can only be used when setting an initial allowance or when resetting it to zero.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L358

https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/081776bf5fae2122bfda8a86d5369496adfdf959/contracts/token/ERC20/utils/SafeERC20Upgradeable.sol#L37-L57

### Recommendation

According to OZ one should use {safeIncreaseAllowance} and {safeDecreaseAllowance} instead

## L-08 `Position.sol`: transfer/transferFrom instead of safeTransferFrom/safeTransfer?

### Proof of Concept

[`using SafeERC20 for IERC20`; was declared](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L17)

### Recommendation

Change functions to their "safe" counterparts

## L-09 No zero address checks in `Bathbuddy.spawnBuddy()`

### Proof of Concept

[ `Bathbuddy.spawnBuddy()`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71-L85) acts as a proxy safe constructor and should have all the minimal checks to ensure that either of _owner || newBud || _bathHouse don't get set to the zero address

### Recommendation

Requiring that the address passed is not 0x0 should solve this

## L-10 Unused essential modifier in `Bathbuddy.sol`

### Proof of Concept

The [`BathBuddy.onlyBuddy()`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94-L102) modifier is meant to enforce that only soul-bound Bath Token has calling rights for whichever function gets to be called, this is an essential conditional check but sadly is used no where in the code.

### Recommendation

Since unused code can hint at programming or architectural errors, use the modifier or remove it.

## L-11 `FeeWrapper.sol`: transfer/transferFrom instead of safeTransferFrom/safeTransfer?

### Proof of Concept

[`using SafeERC20 for IERC20`; was declared](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L10)

The `FeeWrapper._chargeFee()` function uses transfer, transferFrom:

```
    function _chargeFee(FeeParams memory _feeParams, address _target) internal {
        address _feeToken = _feeParams.feeToken;
        uint256 _totalAmount = _feeParams.totalAmount;
        uint256 _feeAmount = _feeParams.feeAmount;
        address _feeTo = _feeParams.feeTo;


        // transfer total amount to the FeeWrapper
        IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);
        // transfer fee to the 3rd party protocol
        IERC20(_feeToken).transfer(_feeTo, _feeAmount);


        // approve tokens to the `_target`
        IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
    }
```

### Recommendation

Change functions to their "safe" counterparts

## L-12 Usage of `approve()` instead of `safeIncreaseAllowance()` / `safeDecreaseAllowance()` or `safeApprove()`

### Proof of Concept

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L105

ERC20 tokes always have the theoretical issue with approve, best thing is to use increase/decrease allowance and though some tokens (like USDT) do not work when changing the allowance from an existing non-zero allowance value, so a double step with an incorporation of safeApprove might be the best option.

### Recommendation

Advisably change the function

# Non-critical Issues

## NC-01 Multiple Events commented out

### Proof of Concept

Below are events commented out only from `RubiconMarket.sol` multiple still exist in other contracts in spec.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L16
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L100
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L107
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L129
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L140
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L163
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L185
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L187
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L207
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L661
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L663
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L666

### Recommendation

Verify that this is intended if not, uncomment them, though as understood from close TODOs these are similar events with uncommented ones and should be removed once decided upon

## NC-02 More than a handful of open TODOs left in code

### Proof of Concept

Below are Open Todos only from `RubiconMarket.sol` multiple still exist in other contracts in spec.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L16
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L99
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L128
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L184
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L197
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L299
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L428
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L661
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L663
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L666
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L675

### Recommendation

All code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.

## NC-03 Cancelled out Natspec comment

### Proof of Concept

[SimpleMarket.can_cancel()] (https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L251-L258)

### Recommendation

If the intention of canceling out the comment is cause the explanation no longer stands, then advisably comment gets changed to current correct explanation, if that's not the case then natspec should get uncommented

## NC-04 Missing natspec comments

### Proof of Concept

All around code, though this is more common among multiple modifiers and a few functions

### Recommendation

Add comments, little change but this can go a long way of improving readability of code.
