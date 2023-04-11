## LOW FINDINGS 

##

## [L-1] MIXING AND OUTDATED COMPILER

### Impact 
Mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues

The minimum required version must be 0.8.19 otherwise, contracts will be affected by the following important bug fixes

- 0.8.14:
ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against calldatasize() in all cases.
Override Checker: Allow changing data location for parameters only when overriding external functions.
- 0.8.15
Code Generation: Avoid writing dirty bytes to storage when copying bytes arrays.
Yul Optimizer: Keep all memory side-effects of inline assembly blocks.
- 0.8.16
Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.
- 0.8.17
Yul Optimizer: Prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call.
Apart from these, there are several minor bug fixes and improvements

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;
```
```solidity
FILE: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

2: pragma solidity 0.8.17;
```
```solidity
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;
```
## Recommended Mitigation:
Use More recent version of solidity 

##

## [L-2] Sanity/Threshold/Limit Checks

Devoid of sanity/threshold/limit checks, critical parameters can be configured to invalid values, causing a variety of issues and breaking expected interactions within/between contracts. Consider adding proper uint256 validation as well as zero address checks for critical changes. A worst case scenario would render the contract needing to be re-deployed in the event of human/accidental errors that involve value assignments to immutable variables. If the validation procedure is unclear or too complex to implement on-chain, document the potential issues that could produce invalid values

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
}

function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }

```
[RubiconMarket.sol#L25-L28](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28),[RubiconMarket.sol#L35-L41](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L35-L41)

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

  function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }

    function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }

```
[RubiconMarket.sol#L1466-L1480](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1480)

##

## [L-3] Unsafe typecasting method is used to cast bytes32 to uint256 

To convert a bytes32 variable to a uint256 variable in Solidity, you can use the uint256 cast with the abi.decode function

### EXAMPLE :

```solidity
bytes32 id_ = 0x1234567890123456789012345678901234567890123456789012345678901234;
uint256 id = uint256(abi.decode(bytes(id_), (uint256)));
```
```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

> id_ is type casted from unsafe way from bytes32 to uint256

297: function bump(bytes32 id_) external can_buy(uint256(id_)) {
298: uint256 id = uint256(id_);

565: require(buy(uint256(id), maxTakeAmount));

753: require(buy(uint256(id), maxTakeAmount));

757: require(cancel(uint256(id)));

```

[RubiconMarket.sol#L565](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L565),[RubiconMarket.sol#L753](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L753)

### Recommended Mitigation

Use safe casting way instead incompatible way 

##

## [L-4] Consider using OpenZeppelin’s SafeCast library to prevent unexpected overflows when casting from uint256

In the RubiconMarket contract, the buy() function variable spend of type uint256.

Now, in the function, the value of spend is downcasted to uint128.

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

321: require(uint128(spend) == spend, "spend is not an int");
322: require(uint128(quantity) == quantity, "quantity is not an int");

405: uint128(quantity),
406: uint128(spend)

480: uint128(_offer.pay_amt),
481: uint128(_offer.buy_amt)

519: require(uint128(pay_amt) == pay_amt);
520: require(uint128(buy_amt) == buy_amt);

534: info.timestamp = uint64(block.timestamp);

559: uint128(pay_amt),
560: uint128(buy_amt)

625:  return uint64(block.timestamp);

1053: take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
```
[RubiconMarket.sol#L321-L322](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L321-L322),[RubiconMarket.sol#L405-L406](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L405-L406),[RubiconMarket.sol#L480-L481](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L480-L481),[RubiconMarket.sol#L519-L520](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L519-L520),[RubiconMarket.sol#L534](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L534),[RubiconMarket.sol#L1053](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1053)

Recommended Mitigation Steps:
Consider using OpenZeppelin’s SafeCast library to prevent unexpected overflows when casting from uint256.

##

## [L-5] Prevent division by 0

On several locations in the code precautions are not being taken for not dividing by 0, this will revert the code.
These functions can be called with 0 value in the input, this value is not checked for being bigger than 0, that means in some scenarios this can potentially trigger a division by zero.

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

319: uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;
1317: t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt_old;

```
[RubiconMarket.sol#L319](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L319)

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L41


##

## [L-6] Function may run out of gas

The for loop in the Solidity code you provided, for (uint i = 0; i < payAmts.length; i++) { ... }, has the potential to consume a lot of gas, especially if the payAmts array is large. This is because each iteration of the loop requires the execution of the code inside the loop, which can result in a significant amount of computational work.

If the gas limit for the transaction executing this loop is not set high enough to cover the gas cost of each iteration of the loop, the transaction may run out of gas and fail. When a transaction runs out of gas, all state changes made up to that point are reverted and any ether sent with the transaction is lost

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

  for (uint i = 0; i < payAmts.length; i++) {
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }

```
[RubiconMarket.sol#L899-L906](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L899-L906)

[RubiconMarket.sol#L911-L913](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L911-L913)

[RubiconMarket.sol#L924-L933)](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L924-L933)

##

## [L-7] Array lengths not checked before batch operations

The batchRequote function provided takes in several arrays as input parameters. It is important to note that if the lengths of these arrays do not match, the function may behave unexpectedly or throw an error 

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

   function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }
    }


```

[RubiconMarket.sol#L917-L933](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933)


Recommended Mitigation:

require(
            ids.length == payAmts.length &&
                ids.length == payGems.length &&
                ids.length == buyAmts.length && ids.length == buyGems.length
            "Array lengths do not match"
        );

##

## [L-8] Missing event and or timelock for critical parameter change

Events help non-contract tools to track changes, and events prevent users from being surprised by changes

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

  function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }

    function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }

```
[RubiconMarket.sol#L1466-L1480](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1480)

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700-L716)

##

## [L-9] Don’t use payable.call()

This means the following cases can cause the transfer to fail:

- The contract does not have a payable callback
- The contract’s payable callback spends more than 2300 gas (which is only enough to emit something)
- The contract is called through a proxy which itself uses up the 2300 gas Use OpenZeppelin’s Address.sendValue() instead

```solidity
FILE: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

118: (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");

```
[FeeWrapper.sol#L118](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L118)

##

## [L-10] Inconsistent spacing in comments

Some lines use // x and some use //x. The instances below point out the usages that don’t follow the majority, within each file

[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol) contract should use // x instead //x 

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

854:  //Transfers funds from caller to offer maker, and from market to caller.
882:  return super.cancel(id); //delete the offer.
935:  //deletes _rank [id]
950:  //set the minimum sell amount for a token
956:  ERC20 pay_gem, //token to assign minimum sell amount to
957:  uint256 dust //maker (ask) minimum sell amount
964:  //returns the minimum sell amount for an offer
966:  ERC20 pay_gem //token for which minimum sell amount is queried
971: //return the best offer for a token pair
981: //return the next worse offer in the sorted list
989: //return the next better offer in the sorted list
997: //return the amount of better offers for a token pair
1005://get the first unsorted offer that was inserted by a contract
1014:  //get the next unsorted offer
1038:  //while there is amount to sell
1060: fill_amt = add(fill_amt, baux); //Add amount bought to acumulator
1061: take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
1078: //Meanwhile there is amount to buy
1079: offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
1087:  break; //We consider that all amount is sold
1090: //If amount to buy is higher or equal than current offer amount to sell
1091: fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
1092: buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
1093: take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
1095: //if lower
1102: ); //Add amount sold to acumulator
1103: take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need
1104: buy_amt = 0; //All amount is bought
1129: uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair
1131: fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator
1132: pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay
1134: //If we still need more offers
1135: offerId = getWorseOffer(offerId); //We look for the next best offer
1136: require(offerId != 0); //Fails if there are not enough offers to complete
1145: ); //Add proportional amount of last offer to buy accumulator
```

[RubiconMarket.sol#L854](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L854)

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1162-L1169)

##

## [L-11] A single point of failure

Impact
The onlyOwner or auth roles has a single point of failure and onlyOwner can use critical a few functions.

Even if protocol admins/developers are not malicious there is still a chance for Owner keys to be stolen. In such a case, the attacker can cause serious damage to the project due to important functions. In such a case, users who have invested in project will suffer high financial losses.

> onlyOwner and auth functions 

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

25:   function setOwner(address owner_) external auth {
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1476: function setFeeTo(address newFeeTo) external auth returns (bool) {

```
[RubiconMarket.sol#L25](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25),[RubiconMarket.sol#L1466](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466)

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L958)

```solidity
FILE: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

   function buyAllAmountWithLeverage(
        address quote,
        address asset,
        uint256 quotePayAmount,
        uint256 leverage
    ) external onlyOwner {

function sellAllAmountWithLeverage(
        address asset,
        address quote,
        uint256 assetPayAmount,
        uint256 leverage
    ) external onlyOwner {

210: function closePosition(uint256 posId) external onlyOwner {
226: function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
242: function withdraw(address token, uint256 amount) external onlyOwner {

```
[Position.sol#L93-L98](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L93-L98),[Position.sol#L107-L112](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L107-L112),[Position.sol#L210](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L210),[Position.sol#L226](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L226),[Position.sol#L242](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L242)

```solidity
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

function getReward(
        IERC20 rewardsToken,
        address holderRecipient
    )
        external
        override
        nonReentrant
        whenNotPaused
        updateReward(holderRecipient, address(rewardsToken))
        onlyBathHouse

function notifyRewardAmount(
        uint256 reward,
        IERC20 rewardsToken
    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {

```
[BathBuddy.sol#L168-L177](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L168-L177)

##

## [L-12] Loss of precision due to rounding

Add scalars so roundings are negligible.

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

338:  uint256 fee = mul(spend, feeBPS) / 100_000;
346:  uint256 mFee = mul(spend, makerFee()) / 100_000;
583:  _amount -= mul(amount, feeBPS) / 100_000;
586:  _amount -= mul(amount, makerFee()) / 100_000;
1317: t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt_old;

```
[RubiconMarket.sol#L338](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338)

##

## NON CRITICAL FINDINGS

##

## [NC-1] Named imports can be used

#### CONTEXT

ALL CONTRACTS 

It’s possible to name the imports to improve code readability. 

E.g. import "@openzeppelin/contracts/token/ERC20/IERC20.sol"; can be rewritten as import {IERC20} from “import “@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol”;

> Example 

FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

```solidity 
FILE : 2023-04-rubicon/contracts/BathHouseV2.sol

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "./compound-v2-fork/InterestRateModel.sol";
import "./compound-v2-fork/CErc20Delegator.sol";
import "./compound-v2-fork/Comptroller.sol";
import "./compound-v2-fork/Unitroller.sol";
import "./periphery/BathBuddy.sol";
```
[BathHouseV2.sol#L4-L9](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L4-L9)

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/StorageSlot.sol";

```
[RubiconMarket.sol#L11-L12](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L11-L12)

```solidity
FILE: 2023-04-rubicon/contracts/V2Migrator.sol

import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "./compound-v2-fork/CTokenInterfaces.sol";

```
[V2Migrator.sol#L4-L6](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L4-L6)

```solidity
FILE: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";
import "@openzeppelin/contracts/security/Pausable.sol";

```
[BathBuddy.sol#L4-L7](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L4-L7)

```solidity
FILE: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "../../compound-v2-fork/Comptroller.sol";
import "../../compound-v2-fork/PriceOracle.sol";
import "../../BathHouseV2.sol";
import "../../RubiconMarket.sol";

```
[Position.sol#L4-L11](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L4-L11)

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L4-L5)

### Recommended Mitigation

Name the imports to all contract scopes to improve code readability

##

## [NC-2] Remove commented out code

CONTEXT
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

it's generally considered good practice to remove commented out code from a codebase once it's determined that it's no longer needed or relevant. This can help keep the codebase clean, readable, and maintainable, and can help prevent confusion or errors in the future

##

## [NC-3] Test environment comments and codes should not be in the main version

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

5: // import "hardhat/console.sol";

```
##

## [NC-4] Add a timelock to critical functions

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user). Consider adding a timelock to

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
}

```
[RubiconMarket.sol#L25-L28](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

  function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }

    function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }

```
[RubiconMarket.sol#L1466-L1480](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1480)

##

## [NC-5] No same value control 

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
}

```
[RubiconMarket.sol#L25-L28](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

  function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }

    function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo;
        return true;
    }

```
[RubiconMarket.sol#L1466-L1480](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1480)

##

## [NC-6] Critical changes should use two-step procedure

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two-step procedure on the critical functions.

Consider adding a two-steps pattern on critical changes to avoid mistakenly transferring ownership of roles or critical functionalities to the wrong address

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
}

```
[RubiconMarket.sol#L25-L28](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)



##

## [NC-7] Emit both old and new values in critical changes 

Emitting old and new values when critical changes are made can help you improve the reliability, accuracy, and maintainability of your systems

[RubiconMarket.sol#L25-L28](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28)

##

## [NC-8] Missing NATSPEC

Consider adding NATSPEC on all public/external functions to improve documentation

[RubiconMarket.sol#L25-L27](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L27)
[RubiconMarket.sol#L288-L293](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L288-L293)
[RubiconMarket.sol#L1028-L1034](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1028-L1034)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L760-L766)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L780-L787)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L801-L810)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L824-L833)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L885-L892)
()
()
()

##

## [NC-9] For functions, follow Solidity standard naming conventions (internal function style rule)

Description
The bellow codes don’t follow Solidity’s standard naming convention,

internal and private functions and variables : the mixedCase format starting with an underscore (_mixedCase starting with an underscore)

```solidity
FILE : FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

35: function isAuthorized(address src) internal view returns (bool) {
46: function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
50: function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
54: function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
58: function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
62: function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
66: function imin(int256 x, int256 y) internal pure returns (int256 z) {
70: function imax(int256 x, int256 y) internal pure returns (int256 z) {
77: function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
81: function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
85: function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
89: function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

227:  uint256 internal feeBPS;
230:  address internal feeTo;
```
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L125-L130)

##

## [NC-10] FUNCTIONS,PARAMETERS,MODIFIERS AND VARIABLES IN SNAKE CASE

Use camel case for all functions, parameters and variables and snake case for constants

Snake case examples
The following variable names follow the snake case naming convention:

this_is_snake_case
build_docker_image

Camel case examples
The following are examples of variables that use the camel case naming convention:

FileNotFoundException
toString

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

219: uint256 public last_offer_id;
245: modifier can_buy(uint256 id) virtual {
251: modifier can_cancel(uint256 id) virtual {
719: modifier can_cancel(uint256 id) override {
```

##

## [NC-11] Considered good practice to include a descriptive error message

A good error message should be clear and concise, providing enough information to help the user or developer understand what went wrong and how to fix it. It should also be specific to the error that occurred, rather than a generic or vague message that doesn't provide any useful information

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

246: require(isActive(id));
252: require(isActive(id));
253: require(
            (msg.sender == getOwner(id)) ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );
265: require(!locked);

460: ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt)): 
461: require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

488: require(cancel(uint256(id)));

538: require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

565: require(buy(uint256(id), maxTakeAmount));

581: require(amount > 0);

598: require(!isClosed());

604: require(isActive(id));
605: require(!isClosed());

612: require(
            (msg.sender == getOwner(id)) ||
                isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );

625:  return uint64(block.timestamp);

753: require(buy(uint256(id), maxTakeAmount));

757: require(cancel(uint256(id)));

938: require(!locked);

940: require(
            !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );
```
##

## [NC-12] NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
(https://docs.soliditylang.org/en/v0.8.15/natspec-format.html)

### Recommendation
NatSpec comments should be increased in Contracts

##

## [NC-13] NOT USING THE NAMED RETURN VARIABLES ANYWHERE IN THE FUNCTION IS CONFUSING

Consider changing the variable to be an unnamed one

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L280)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L452-L454)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L491-L496)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L511-L518)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L578-L580)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620-L622)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1028-L1034)
()
()
()
()
()
()

##

## [NC-14] Mark visibility of initialize(…) functions as external

Description
External instead of public would give more the sense of the initialize(…) functions to behave like a constructor (only called on deployment, so should only be called externally).

Security point of view, it might be safer so that it cannot be called internally by accident in the child contract.

It might cost a bit less gas to use external over public.

It is possible to override a function from external to public (= “opening it up”) ✅
but it is not possible to override a function from public to external (= “narrow it down”). ❌

For above reasons you can change initialize(…) to external

(https://github.com/OpenZeppelin/openzeppelin-contracts/issues/3750)

```solidity
FILE : 2023-04-rubicon/contracts/RubiconMarket.sol

700:  function initialize(address _feeTo) public {

```
[RubiconMarket.sol#L700](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700)

##

## [NC-15] Contract layout and order of functions

The Solidity style guide recommends declaring state variables before all functions. 

(https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout)

CONTEXT
ALL CONTRACT

### Layout contract elements in the following order:

- Pragma statements

- Import statements

Interfaces

- Libraries

- Contracts

### Inside each contract, library or interface, use the following order:

- Type declarations

- State variables

- Events

- Modifiers

- Functions

### Recommendations 

All contracts should follow the solidity style guide 

##

## [NC-16] Pragma float

[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol), [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol) both contracts using the floating pragma 

### Recommendation
Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version.

Note that pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or a package

##

## [NC-17] Use solidity naming conventions for state variables 

State variables name not starting with "_". The "_name" is reserved for internal/private functions and variables 

(https://docs.soliditylang.org/en/v0.8.17/style-guide.html)

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

691: mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids
692: mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair
693: mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook
694: mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers
695: mapping(uint256 => uint256) public _near; //next unsorted offer id
696: uint256 public _head; //first unsorted offer id

```
[RubiconMarket.sol#L691-L696](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L691-L696)

##

## [NC-18] Interchangeable usage of uint and uint256

Consider using only one approach throughout the codebase, e.g. only uint or only uint256

(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L781-L785)
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L918-L921)
()
()

##

## [NC-19] TYPOS

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

/// @audit multuple => multiple
886:  /// @notice Batch offer functionality - multuple offers in a single transaction

/// @audit acumulator=> accumulator
1051: fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

/// @audit acumulator=> accumulator
1060: fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

/// @audit acumulator=> accumulator
1091: fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

```
## [NC-20] public functions not called by the contract should be declared external instead

Contracts are [allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents’ functions and change the visibility from external to public.

```solidity
FILE: 2023-04-rubicon/contracts/BathHouseV2.sol

45: function getBathTokenFromAsset(
        address asset
    ) public view returns (address) {

```
[BathHouseV2.sol#L45-L47](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L45-L47)

```solidity
FILE: 2023-04-rubicon/contracts/RubiconMarket.sol

574:  function getFeeBPS() public view returns (uint256) {
1010: function getFirstUnsortedOffer() public view returns (uint256) {
1016: function getNextUnsortedOffer(uint256 id) public view returns (uint256) {
1020: function isOfferSorted(uint256 id) public view returns (bool) {

1165: function getPayAmountWithFee(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint256 buy_amt
    ) public view returns (uint256 fill_amt) {


```
[RubiconMarket.sol#L574](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L574)

##

## [Nc-21] Use scientific notations rather than exponential notations

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1175-L1177
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1142-L1144
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1099-L1101
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1057-L1059
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L331
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317

##

## [NC-22] Use underscores for number literals

```solidity
FILE: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

459: uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
481: uint256 _fee = _minFill.mul(_feeBPS).div(10000);
490:  _fee = _payAmount.mul(_feeBPS).div(10000);

```
### Recommended Mitigation

```solidity
459: uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10_000);


```




















Lack of pricirison 


NC-1	Missing checks for address(0) when assigning values to address state variables	5
NC-2	require() / revert() statements should have descriptive reason strings	45
NC-3	Return values of approve() not checked	4
NC-4	TODO Left in the code	14
NC-5	Event is missing indexed fields	17
NC-6	Constants should be defined rather than using magic numbers	1
NC-7	Functions not used internally could be marked external	10

L-1	abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()	6
L-2	Do not use deprecated library functions	1
L-3	Initializers could be front-run	2
L-4	Unsafe ERC20 operation(s)	20