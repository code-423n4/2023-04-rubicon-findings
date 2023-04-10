## Non Critical Issues

|       | Issue                                                                                    |
| ----- | :--------------------------------------------------------------------------------------- |
| NC-1  | ADD TO INDEXED PARAMETER FOR COUNTABLE EVENTS                                            |
| NC-2  | ADD A TIMELOCK TO CRITICAL FUNCTIONS                                                     |
| NC-3  | BE EXPLICIT DECLARING TYPES                                                              |
| NC-4  | USE OF BYTES.CONCAT() INSTEAD OF ABI.ENCODEPACKED(,)                                     |
| NC-5  | GENERATE PERFECT CODE HEADERS EVERY TIME                                                 |
| NC-6  | COMMENTED CODE                                                                           |
| NC-7  | SOLIDITY COMPILER VERSIONS MISMATCH                                                      |
| NC-8  | FUNCTIONS, PARAMETERS AND VARIABLES IN SNAKE CASE                                        |
| NC-9  | FOR MODERN AND MORE READABLE CODE; UPDATE IMPORT USAGES                                  |
| NC-10 | MARK VISIBILITY OF INITIALIZE(…) FUNCTIONS AS EXTERNAL                                   |
| NC-11 | CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT |
| NC-12 | LACK OF EVENT EMISSION AFTER CRITICAL INITIALIZE() FUNCTIONS                             |
| NC-13 | LARGE MULTIPLES OF TEN SHOULD USE SCIENTIFIC NOTATION                                    |
| NC-14 | FOR EXTENDED "USING-FOR" USAGE, USE THE LATEST PRAGMA VERSION                            |
| NC-15 | MISSING EVENT FOR CRITICAL PARAMETER CHANGE                                              |
| NC-16 | NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS                                        |
| NC-17 | INCLUDE RETURN PARAMETERS IN NATSPEC COMMENTS                                            |
| NC-18 | NO NEED TO RE-INHERIT                                                                    |
| NC-19 | NO SAME VALUE INPUT CONTROL                                                              |
| NC-20 | OMISSIONS IN EVENTS                                                                      |
| NC-21 | SOLIDITY COMPILER OPTIMIZATIONS CAN BE PROBLEMATIC                                       |
| NC-22 | FUNCTION WRITING THAT DOES NOT COMPLY WITH THE SOLIDITY STYLE GUIDE                      |
| NC-23 | NO CONFIG.SOL FILE                                                                       |
| NC-24 | DON’T USE PERIODS WITH FRAGMENTS                                                         |
| NC-25 | USE A MORE RECENT VERSION OF SOLIDITY                                                    |
| NC-26 | UNUSED IMPORTS                                                                           |
| NC-27 | FOR FUNCTIONS AND VARIABLES FOLLOW SOLIDITY STANDARD NAMING CONVENTIONS                  |
| NC-28 | LINES ARE TOO LONG                                                                       |

### [NC-1] ADD TO INDEXED PARAMETER FOR COUNTABLE EVENTS

#### Description:

Add to indexed parameter for countable Events

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

664:     event LogUnsortedOffer(uint256 id);

```

```solidity
File: contracts/periphery/BathBuddy.sol

262:     event Staked(address indexed user, uint256 amount);

263:     event Withdrawn(address indexed user, uint256 amount);

266:     event Recovered(address token, uint256 amount);

```

### [NC-2] ADD A TIMELOCK TO CRITICAL FUNCTIONS

#### Description:

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

25:     function setOwner(address owner_) external auth {

955:     function setMinSell(

1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {

```

```solidity
File: contracts/periphery/BathBuddy.sol

232:     function setRewardsDuration(

```

### [NC-3] BE EXPLICIT DECLARING TYPES

#### Description:

Instead of uint use uint256

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

785:         uint pos, //position to insert offer, 0 should be used if unknown

888:         uint[] calldata payAmts,

890:         uint[] calldata buyAmts,

899:         for (uint i = 0; i < payAmts.length; i++) {

910:     function batchCancel(uint[] calldata ids) external {

911:         for (uint i = 0; i < ids.length; i++) {

918:         uint[] calldata ids,

919:         uint[] calldata payAmts,

921:         uint[] calldata buyAmts,

924:         for (uint i = 0; i < ids.length; i++) {

```

### [NC-4] USE OF BYTES.CONCAT() INSTEAD OF ABI.ENCODEPACKED(,)

#### Description:

Rather than using `abi.encodePacked` for appending bytes, since version 0.8.4, `bytes.concat()` is enabled.

Since version 0.8.4 for appending bytes, `bytes.concat()` can be used instead of `abi.encodePacked(,)`.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

369:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

400:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

423:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

442:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

476:             keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),

555:             keccak256(abi.encodePacked(pay_gem, buy_gem)),

```

### [NC-5] GENERATE PERFECT CODE HEADERS EVERY TIME

#### Description:

I recommend using header for Solidity code layout and readability:
https://github.com/transmissions11/headers

```solidity
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
```

### [NC-6] COMMENTED CODE

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

99:         /// TODO: double check it is sound logic to kill this event
            /// event LogTrade(
            ///     uint256 pay_amt,
            ///     address indexed pay_gem,
            ///     uint256 buy_amt,
            ///     address indexed buy_gem
            /// );

            /// event LogMake(
            ///     bytes32 indexed id,
            ///     bytes32 indexed pair,
            ///     address indexed maker,
            ///     ERC20 pay_gem,
            ///     ERC20 buy_gem,
            ///     uint128 pay_amt,
            ///     uint128 buy_amt,
            ///     uint64 timestamp
            /// );

163:    /// event LogKill(
        ///     bytes32 indexed id,
        ///     bytes32 indexed pair,
        ///     address indexed maker,
        ///     ERC20 pay_gem,
        ///     ERC20 buy_gem,
        ///     uint128 pay_amt,
        ///     uint128 buy_amt,
        ///     uint64 timestamp
        /// );


163:    /// event FeeTake(
        ///     bytes32 indexed id,
        ///     bytes32 indexed pair,
        ///     ERC20 asset,
        ///     address indexed taker,
        ///     address feeTo,
        ///     uint256 feeAmt,
        ///     uint64 timestamp
        /// );

300:            /// emit LogBump(
                ///     id_,
                ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
                ///     offers[id].owner,
                ///     offers[id].pay_gem,
                ///     offers[id].buy_gem,
                ///     uint128(offers[id].pay_amt),
                ///     uint128(offers[id].buy_amt),
                ///     offers[id].timestamp
                /// );

386:    /// emit LogTake(
        ///     bytes32(id),
        ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
        ///     _offer.owner,
        ///     _offer.pay_gem,
        ///     _offer.buy_gem,
        ///     msg.sender,
        ///     uint128(quantity),
        ///     uint128(spend),
        ///     uint64(block.timestamp)
        /// );

409:    /// emit FeeTake(
        ///     bytes32(id),
        ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
        ///     _offer.buy_gem,
        ///     msg.sender,
        ///     feeTo,
        ///     fee,
        ///     uint64(block.timestamp)
        /// );

429:    /// emit LogTrade(
        ///     quantity,
        ///     address(_offer.pay_gem),
        ///     spend,
        ///     address(_offer.buy_gem)
        /// );

464:    /// emit LogKill(
        ///     bytes32(id),
        ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
        ///     _offer.owner,
        ///     _offer.pay_gem,
        ///     _offer.buy_gem,
        ///     uint128(_offer.pay_amt),
        ///     uint128(_offer.buy_amt)
        /// );

1346:   // function _offeru(
        //     uint256 pay_amt, //maker (ask) sell how much
        //     ERC20 pay_gem, //maker (ask) sell which token
        //     uint256 buy_amt, //maker (ask) buy how much
        //     ERC20 buy_gem, //maker (ask) buy which token
        //     address owner,
        //     address recipient
        // ) internal returns (uint256 id) {
        //     require(_dust[address(pay_gem)] <= pay_amt);
        //     id = super.offer(pay_amt, pay_gem, buy_amt, buy_gem, owner, recipient);
        //     _near[id] = _head;
        //     _head = id;
        //     emit LogUnsortedOffer(id);
        // }

```

### [NC-7] SOLIDITY COMPILER VERSIONS MISMATCH

#### Description:

The project is compiled with different versions of Solidity, which is not recommended because it can lead to undefined behaviors.

It is better to use one Solidity compiler version across all contracts instead of different versions with different bugs and security checks.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```

```solidity
File: contracts/V2Migrator.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: contracts/utilities/FeeWrapper.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

2: pragma solidity 0.8.17;

```

### [NC-8] FUNCTIONS, PARAMETERS AND VARIABLES IN SNAKE CASE

#### Description:

Use camel case for all functions, parameters and variables and snake case for constants.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

122:         ERC20 pay_gem,

123:         ERC20 buy_gem,

124:         uint128 pay_amt,

125:         uint128 buy_amt

159:         uint128 take_amt,

160:         uint128 give_amt

219:     uint256 public last_offer_id;

245:     modifier can_buy(uint256 id) virtual {

251:     modifier can_cancel(uint256 id) virtual {

260:     modifier can_offer() virtual {

568:     function _next_id() internal returns (uint256) {

597:     modifier can_offer() override {

603:     modifier can_buy(uint256 id) override {

610:     modifier can_cancel(uint256 id) virtual override {

662:     event LogMinSell(address pay_gem, uint256 min_amount);

662:     event LogMinSell(address pay_gem, uint256 min_amount);

719:     modifier can_cancel(uint256 id) override {

937:     function del_rank(uint256 id) external returns (bool) {

960:         emit LogMinSell(address(pay_gem), dust);

1162:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1211:         address buy_gem = address(offers[id].buy_gem);

1212:         address pay_gem = address(offers[id].pay_gem);

1213:         uint256 top = _best[pay_gem][buy_gem];

1214:         uint256 old_top = 0;

1283:         uint256 best_maker_id; //highest maker id

1284:         uint256 t_buy_amt_old; //taker buy how much saved

1285:         uint256 m_buy_amt; //maker offer wants to buy this much token

1286:         uint256 m_pay_amt; //maker offer wants to sell this much token

1368:         ERC20 buy_gem = offers[id].buy_gem;

1369:         ERC20 pay_gem = offers[id].pay_gem;

```

### [NC-9] FOR MODERN AND MORE READABLE CODE; UPDATE IMPORT USAGES

#### Description:

Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct polluted the source code with an unnecessary object we were not using because we did not need it.

This was breaking the rule of modularity and modular programming: only import what you need Specific imports with curly braces allow us to apply this rule better.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

5: import "./compound-v2-fork/InterestRateModel.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

7: import "./compound-v2-fork/Comptroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

9: import "./periphery/BathBuddy.sol";

```

```solidity
File: contracts/V2Migrator.sol

6: import "./compound-v2-fork/CTokenInterfaces.sol";

```

```solidity
File: contracts/utilities/FeeWrapper.sol

5: import "./RubiconRouter.sol";

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

8: import "../../compound-v2-fork/Comptroller.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

10: import "../../BathHouseV2.sol";

11: import "../../RubiconMarket.sol";

```

#### Recommended Mitigation Steps:

`import {contract1 , contract2} from "filename.sol";` OR Use specific imports syntax per solidity docs recommendation.

### [NC-10] MARK VISIBILITY OF INITIALIZE(…) FUNCTIONS AS EXTERNAL

#### Description:

If someone wants to extend via inheritance, it might make more sense that the overridden initialize(...) function calls the internal {...}\_init function, not the parent public initialize(...) function.

External instead of public would give more sense of the initialize(...) functions to behave like a constructor (only called on deployment, so should only be called externally).

From a security point of view, it might be safer so that it cannot be called internally by accident in the child contract.

It might cost a bit less gas to use external over public.

It is possible to override a function from external to public (= “opening it up”) ✅ but it is not possible to override a function from public to external (= “narrow it down”). ❌

For the above reasons, you can change initialize(...) to external:

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

700:     function initialize(address _feeTo) public {

```

### [NC-11] CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

#### Description:

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

### [NC-12] LACK OF EVENT EMISSION AFTER CRITICAL INITIALIZE() FUNCTIONS

#### Description:

To record the init parameters for off-chain monitoring and transparency reasons, please consider emitting an event after the initialize() functions.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

32:     function initialize(address _comptroller, address _pAdmin) external {

```

```solidity
File: contracts/RubiconMarket.sol

700:     function initialize(address _feeTo) public {

```

### [NC-13] LARGE MULTIPLES OF TEN SHOULD USE SCIENTIFIC NOTATION

#### Description:

Use (e.g. 1e6) rather than decimal literals (e.g. 100000), for better code readability.

#### **Proof Of Concept**

```solidity
File: contracts/utilities/poolsUtility/Position.sol

459:         uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);

481:         uint256 _fee = _minFill.mul(_feeBPS).div(10000);

490:         _fee = _payAmount.mul(_feeBPS).div(10000);

```

### [NC-14] FOR EXTENDED "USING-FOR" USAGE, USE THE LATEST PRAGMA VERSION

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```

#### Recommended Mitigation Steps:

Use solidity pragma version min. 0.8.13

### [NC-15] MISSING EVENT FOR CRITICAL PARAMETER CHANGE

#### Description:

Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

https://github.com/crytic/slither/wiki/Detector-Documentation#missing-events-access-control

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {

```

### [NC-16] NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS

#### Context:

All contracts

#### Description:

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html.

#### Recommended Mitigation Steps:

NatSpec comments should be increased in contracts

### [NC-17] INCLUDE RETURN PARAMETERS IN NATSPEC COMMENTS

#### Context:

All contracts

#### Description:

If Return parameters are declared, you must prefix them with ”/// @return”.

Some code analysis programs do analysis by reading NatSpec details, if they can’t see the “@return” tag, they do incomplete analysis.

Reference: https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

#### Recommended Mitigation Steps:

Include return parameters in NatSpec comments

### [NC-18] NO NEED TO RE-INHERIT

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

import "./compound-v2-fork/InterestRateModel.sol";
import "./compound-v2-fork/CErc20Delegator.sol";
import "./compound-v2-fork/Comptroller.sol";
import "./compound-v2-fork/Unitroller.sol";
import "./periphery/BathBuddy.sol";

```

```solidity
File: contracts/V2Migrator.sol

import "./compound-v2-fork/CTokenInterfaces.sol";

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

import "../../compound-v2-fork/Comptroller.sol";
import "../../compound-v2-fork/PriceOracle.sol";
import "../../BathHouseV2.sol";
import "../../RubiconMarket.sol";

```

```solidity
File: contracts/utilities/FeeWrapper.sol

import "./RubiconRouter.sol";

```

### [NC-19] NO SAME VALUE INPUT CONTROL

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

705:         feeTo = _feeTo;

```

```solidity
File: contracts/periphery/BathBuddy.sol

77:         owner = _owner;

79:         bathHouse = _bathHouse;

```

```solidity
File: contracts/utilities/FeeWrapper.sol

94:         address _feeToken = _feeParams.feeToken;

95:         uint256 _totalAmount = _feeParams.totalAmount;

96:         uint256 _feeAmount = _feeParams.feeAmount;

97:         address _feeTo = _feeParams.feeTo;

112:         uint256 _totalAmount = _feeParams.totalAmount;

113:         uint256 _feeAmount = _feeParams.feeAmount;

114:         address _feeTo = _feeParams.feeTo;

```

### [NC-20] OMISSIONS IN EVENTS

#### Description:

Throughout the codebase, events are generally emitted when sensitive changes are made to the contracts. However, some events are missing important parameters.

The events should include the new value and old value where possible.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

27:         emit LogSetOwner(owner);

384:         emit LogItemUpdate(id);

463:         emit LogItemUpdate(id);

540:         emit LogItemUpdate(id);

708:         emit LogSetOwner(msg.sender);

1401:         emit LogSortedOffer(id);

```

```solidity
File: contracts/periphery/BathBuddy.sol

227:         emit RewardAdded(reward);

241:         emit RewardsDurationUpdated(rewardsDuration[token]);

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

418:         emit PositionOpened(lastPositionId, pos);

423:         emit PositionClosed(_positionId);

```

### [NC-21] SOLIDITY COMPILER OPTIMIZATIONS CAN BE PROBLEMATIC

#### **Proof Of Concept**

```solidity
File: hardhat.config.ts

  solidity: {
    compilers: [
      {
        version: "0.8.17",
        settings: {
          optimizer: {
            enabled: true,
            runs: 5,
          },
        },
      },
      {
        version: "0.7.6",
      },
    ],
  },

```

### [NC-22] FUNCTION WRITING THAT DOES NOT COMPLY WITH THE SOLIDITY STYLE GUIDE

#### Context:

contracts/RubiconMarket.sol
contracts/BathHouseV2.sol
contracts/periphery/BathBuddy.sol
contracts/utilities/poolsUtility/Position.sol

#### Description:

Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private, within a grouping, place the view and pure functions last

### [NC-23] NO CONFIG.SOL FILE

Better to have config facet, in case some update is needed in the config.sol.

### [NC-24] DON’T USE PERIODS WITH FRAGMENTS

Throughout the files, most of the comments have fragments that end without periods. They should either be converted to actual sentences with both a noun phrase and a verb phrase that ends with periods, or the periods should be removed.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

312:    /// @notice Accept a given `quantity` of an offer. Transfers funds from caller/taker to offer maker, and from market to caller/taker.
313:    /// @notice The fee for taker trades is paid in this function.

324:        ///@dev For backwards semantic compatibility.

450:    /// @notice Allows the caller to cancel the offer if it is their own.
451:    /// @notice This function refunds the offer to the maker.

510:    /// @notice Key function to make a new offer. Takes funds from the caller into market escrow.

591: /// @notice Expiring market is a Simple Market with a market lifetime.

592: /// @dev When the close_time has been reached, offers can only be cancelled (offer and buy will throw).

596:    /// @dev After close_time has been reached, no new offers are allowed.

602:    /// @dev After close, no new buys are allowed.

609:    /// @dev After close, anyone can cancel an offer.

854:    //Transfers funds from caller to offer maker, and from market to caller.

870:    // Cancel an offer. Refunds offer maker.

936:   //  Function should be called by keepers.

1015:   //      Can be used to cycle through all the unsorted offers.

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

63: /// @return balance - the user’s current borrow balance (with interest) in units of the underlying asset.

```

### [NC-25] USE A MORE RECENT VERSION OF SOLIDITY

#### Description:

For security, it is best practice to use the latest Solidity version. For the security fix list in the versions; https://github.com/ethereum/solidity/blob/develop/Changelog.md

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/RubiconMarket.sol

2: pragma solidity ^0.8.9;

```

```solidity
File: contracts/V2Migrator.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/periphery/BathBuddy.sol

2: pragma solidity ^0.8.0;

```

```solidity
File: contracts/utilities/FeeWrapper.sol

2: pragma solidity 0.8.17;

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

2: pragma solidity 0.8.17;

```

### [NC-26] UNUSED IMPORTS

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

import "./compound-v2-fork/InterestRateModel.sol";
import "./compound-v2-fork/CErc20Delegator.sol";
import "./compound-v2-fork/Comptroller.sol";
import "./compound-v2-fork/Unitroller.sol";
import "./periphery/BathBuddy.sol";

```

```solidity
File: contracts/V2Migrator.sol

import "./compound-v2-fork/CTokenInterfaces.sol";

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

import "../../compound-v2-fork/Comptroller.sol";
import "../../compound-v2-fork/PriceOracle.sol";
import "../../BathHouseV2.sol";
import "../../RubiconMarket.sol";

```

```solidity
File: contracts/utilities/FeeWrapper.sol

import "./RubiconRouter.sol";

```

### [NC-27] FOR FUNCTIONS AND VARIABLES FOLLOW SOLIDITY STANDARD NAMING CONVENTIONS

#### Description:

Solidity’s standard naming convention for internal and private functions and variables (apart from constants): the mixedCase format starting with an underscore (\_mixedCase starting with an underscore)

Solidity’s standard naming convention for internal and private constants variables: the snake_case format starting with an underscore (\_mixedCase starting with an underscore) and use ALL_CAPS for naming them.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

18:     bool private initialized;

20:     mapping(address => address) private tokenToBathToken;

21:     mapping(address => address) private bathTokenToBuddy;

```

```solidity
File: contracts/RubiconMarket.sol

35:     function isAuthorized(address src) internal view returns (bool) {

46:     function add(uint256 x, uint256 y) internal pure returns (uint256 z) {

50:     function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {

54:     function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {

58:     function min(uint256 x, uint256 y) internal pure returns (uint256 z) {

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

77:     function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

81:     function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

85:     function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

89:     function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

227:     uint256 internal feeBPS;

230:     address internal feeTo;

232:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

125:     function openPosition(

```

### [NC-28] LINES ARE TOO LONG

#### Description:

Usually lines in source code are limited to 80 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length.

[Reference](https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length)

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

292:         return (_offer.pay_amt, _offer.pay_gem, _offer.buy_amt, _offer.buy_gem);

360:                     _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),

376:             "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"

460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))

522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

691:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

694:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers

726:             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."

1039:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1040:             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1051:                 fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

1052:                 pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell

1053:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1060:                 fill_amt = add(fill_amt, baux); //Add amount bought to acumulator

1061:                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

1079:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1091:                 fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator

1092:                 buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1093:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1103:                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

1129:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1131:             fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator

1132:             pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay

1135:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1136:                 require(offerId != 0); //Fails if there are not enough offers to complete

1162:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1164:             fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator

1165:             buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1168:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1169:                 require(offerId != 0); //Fails if there are not enough offers to complete

1225:     function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1444:         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

1461:         _near[pre] = _near[id]; //set previous unsorted offer to point to offer after offer id

```

```solidity
File: contracts/periphery/BathBuddy.sol

53:     mapping(address => uint256) public rewardRates; // Token specific reward rates

54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific

61:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

238:             "Previous rewards period must be complete before changing the duration for the new period"

```

```solidity
File: contracts/utilities/FeeWrapper.sol

22:         uint256 feeAmount; // amount of feeToken from which fee should be charged

93:     function _chargeFee(FeeParams memory _feeParams, address _target) internal {

100:         IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

27:         uint256 lastBorrow; // how much borrow on the last loop (check _borrowLimit)

28:         uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution

29:         uint256 currentAssetBalance; // balance of asset in the moment of execution

30:         uint256 toBorrow; // certain perc. to borrow from max amount available to borrow

37:         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied

54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

65:     function borrowBalance(address bathToken) public returns (uint256 balance) {

```

## Low Issues

|      | Issue                                                                          |
| ---- | :----------------------------------------------------------------------------- |
| L-1  | DOS WITH BLOCK GAS LIMIT                                                       |
| L-2  | AVOID `TRANSFER()`/`SEND()` AS REENTRANCY MITIGATIONS                          |
| L-3  | CRITICAL ADDRESS CHANGES SHOULD USE TWO-STEP PROCEDURE                         |
| L-4  | THE CRITICAL PARAMETERS IN INITIALIZE(...) ARE NOT SET SAFELY                  |
| L-5  | DO NOT USE DEPRECATED LIBRARY FUNCTIONS                                        |
| L-6  | DON’T USE OWNER                                                                |
| L-7  | LACK OF INPUT VALIDATION                                                       |
| L-8  | LOSS OF PRECISION DUE TO ROUNDING                                              |
| L-9  | UNIFY RETURN CRITERIA                                                          |
| L-10 | OWNER CAN RENOUNCE OWNERSHIP                                                   |
| L-11 | REQUIRE MESSAGES ARE TOO SHORT AND UNCLEAR                                     |
| L-12 | THE SAFETRANSFER FUNCTION DOES NOT CHECK FOR POTENTIALLY SELF-DESTROYED TOKENS |
| L-13 | MODIFIER SIDE-EFFECTS                                                          |
| L-14 | A SINGLE POINT OF FAILURE                                                      |
| L-15 | BLOCK VALUES AS A PROXY FOR TIME                                               |
| L-16 | ACCOUNT EXISTENCE CHECK FOR LOW-LEVEL CALLS                                    |
| L-17 | USE `SAFETRANSFER` INSTEAD OF `TRANSFER`                                       |
| L-18 | VOID CONSTRUCTOR                                                               |

### [L-1] DOS WITH BLOCK GAS LIMIT

#### Description:

Programming patterns such as looping over arrays of unknown size may lead to DoS when the gas cost of execution exceeds the block gas limit.

Reference: https://swcregistry.io/docs/SWC-128

This loops could drain all user gas and revert.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

122:    for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: contracts/RubiconMarket.sol

899:    for (uint i = 0; i < payAmts.length; i++) {

911:    for (uint i = 0; i < ids.length; i++) {

924:    for (uint i = 0; i < ids.length; i++) {


```

```solidity
File: contracts/V2Migrator.sol

31:     for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```

### [L-2] AVOID `TRANSFER()`/`SEND()` AS REENTRANCY MITIGATIONS

#### Description:

Although `transfer()` and `send()` have been recommended as a security best-practice to prevent reentrancy attacks because they only forward 2300 gas, the gas repricing of opcodes may break deployed contracts. Use `call()` instead, without hardcoded gas limits along with checks-effects-interactions pattern or reentrancy guards for reentrancy protection.

https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/

https://swcregistry.io/docs/SWC-134

#### **Proof Of Concept**

```solidity
File: contracts/V2Migrator.sol

59:         IERC20(bathTokenV2).transfer(

```

```solidity
File: contracts/utilities/FeeWrapper.sol

102:         IERC20(_feeToken).transfer(_feeTo, _feeAmount);

```

#### Recommended Mitigation Steps:

Using low-level `call.value(amount)` with the corresponding result check or using the OpenZeppelin `Address.sendValue` is advised:https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Address.sol#L60

### [L-3] CRITICAL ADDRESS CHANGES SHOULD USE TWO-STEP PROCEDURE

#### Description:

The critical procedures should be two step process.

Changing critical addresses in contracts should be a two-step process where the first transaction (from the old/current address) registers the new address (i.e. grants ownership) and the second transaction (from the new address) replaces the old address with the new one (i.e. claims ownership). This gives an opportunity to recover from incorrect addresses mistakenly used in the first step. If not, contract functionality might become inaccessible. (see [here](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/1488) and [here](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/2369))

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

25:     function setOwner(address owner_) external auth {

```

#### Recommended Mitigation Steps:

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

### [L-4] THE CRITICAL PARAMETERS IN INITIALIZE(...) ARE NOT SET SAFELY

#### Description:

The critical parameters in initialize(...) are not set safely:

- \_pAdmin should be validated

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

32:     function initialize(address _comptroller, address _pAdmin) external {
            require(!initialized, "BathHouseV2 already initialized!");
            comptroller = Comptroller(_comptroller);
            admin = msg.sender;
            proxyAdmin = _pAdmin;

            initialized = true;
    }
```

### [L-5] DO NOT USE DEPRECATED LIBRARY FUNCTIONS

#### Description:

You use `safeApprove `of openZeppelin although it’s deprecated. (see [here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38%3E).

You should change it to increase/decrease Allowance as OpenZeppilin says.

#### **Proof Of Concept**

```solidity
File: contracts/utilities/poolsUtility/Position.sol

358:         IERC20(_token).safeApprove(_bathToken, _amount);

```

### [L-6] DON’T USE OWNER

#### Description:

The owner manipulates the contract without a lock time period.

#### **Proof Of Concept**

```solidity
File: contracts/periphery/BathBuddy.sol

88:         require(msg.sender == owner);

```

### [L-7] LACK OF INPUT VALIDATION

#### Description:

For defence-in-depth purpose, it is recommended to perform additional validation against the amount that the user is attempting to deposit, mint, withdraw and redeem to ensure that the submitted amount is valid.
https://code4rena.com/reports/2022-11-redactedcartel#l-11-lack-of-input-validation

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

857:         uint256 amount

```

```solidity
File: contracts/V2Migrator.sol

10:         uint256 shares

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
233:        IERC20(asset).safeTransferFrom(msg.sender, address(this), amount);

242:     function withdraw(address token, uint256 amount) external onlyOwner {
243:            IERC20(token).safeTransfer(msg.sender, amount);
244:    }

```

### [L-8] LOSS OF PRECISION DUE TO ROUNDING

#### **Proof Of Concept**

```solidity
File: contracts/utilities/FeeWrapper.sol

41:             fees[i] = (tokenAmounts[i] * feeValue) / feeType;

```

### [L-9] UNIFY RETURN CRITERIA

#### Description:

In contracts, sometimes the name of the return variable is not defined and sometimes is, unifying the way of writing the code makes the code more uniform and readable.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

47:     ) public view returns (address) {

53:     ) external view returns (address buddy) {

142:         returns (string memory _name, string memory _symbol, uint8 _decimals)

```

```solidity
File: contracts/RubiconMarket.sol

35:     function isAuthorized(address src) internal view returns (bool) {

46:     function add(uint256 x, uint256 y) internal pure returns (uint256 z) {

50:     function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {

54:     function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {

58:     function min(uint256 x, uint256 y) internal pure returns (uint256 z) {

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

77:     function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

81:     function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {

85:     function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

89:     function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {

272:     function makerFee() public view returns (uint256) {

276:     function isActive(uint256 id) public view returns (bool active) {

280:     function getOwner(uint256 id) public view returns (address owner) {

284:     function getRecipient(uint256 id) public view returns (address owner) {

290:     ) public view returns (uint256, ERC20, uint256, ERC20) {

317:     ) public virtual can_buy(id) synchronized returns (bool) {

454:     ) public virtual can_cancel(id) synchronized returns (bool success) {

496:     ) external virtual returns (bytes32 id) {

518:     ) public virtual can_offer synchronized returns (uint256 id) {

568:     function _next_id() internal returns (uint256) {

574:     function getFeeBPS() public view returns (uint256) {

580:     ) public view returns (uint256 _amount) {

620:     function isClosed() public pure returns (bool closed) {

624:     function getTime() public view returns (uint64) {

738:     ) public override returns (bytes32) {

766:     ) external can_offer returns (uint256) {

787:     ) external can_offer returns (uint256) {

810:     ) external can_offer returns (uint256) {

833:     ) public can_offer returns (uint256) {

858:     ) public override can_buy(id) returns (bool) {

863:         function(uint256, uint256) returns (bool) fn = matchingEnabled

873:     ) public override can_cancel(id) returns (bool success) {

937:     function del_rank(uint256 id) external returns (bool) {

958:     ) external auth note returns (bool) {

967:     ) external view returns (uint256) {

977:     ) public view returns (uint256) {

985:     function getWorseOffer(uint256 id) public view returns (uint256) {

993:     function getBetterOffer(uint256 id) external view returns (uint256) {

1001:     ) public view returns (uint256) {

1010:     function getFirstUnsortedOffer() public view returns (uint256) {

1016:     function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1020:     function isOfferSorted(uint256 id) public view returns (bool) {

1033:     ) external returns (uint256 fill_amt) {

1074:     ) external returns (uint256 fill_amt) {

1119:     ) external view returns (uint256 fill_amt) {

1128:     ) public view returns (uint256 fill_amt) {

1153:     ) public view returns (uint256 fill_amt) {

1161:     ) public view returns (uint256 fill_amt) {

1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {

1208:     function _find(uint256 id) internal view returns (uint256) {

1225:     function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1264:     ) internal view returns (bool) {

1282:     ) internal returns (uint256 id) {

1407:     ) internal returns (bool) {

1440:     ) internal returns (bool) {

1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {

1482:     function getFeeTo() external view returns (address) {

```

```solidity
File: contracts/V2Migrator.sol

11:     ) external returns (uint256 amountWithdrawn);

13:     function underlyingToken() external view returns(IERC20);

```

```solidity
File: contracts/periphery/BathBuddy.sol

20:     ) external view returns (uint256);

114:     ) public view returns (uint256) {

121:     function rewardPerToken(address token) public view returns (uint256) {

142:     ) public view override returns (uint256) {

159:     ) external view returns (uint256) {

```

```solidity
File: contracts/utilities/FeeWrapper.sol

35:         returns (uint256[] memory amountsWithFee, uint256[] memory fees)

50:     ) external payable returns (bytes memory) {

62:     ) internal returns (bytes memory) {

78:     ) internal returns (bytes memory) {

110:     ) internal returns (uint256 _msgValue) {

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

65:     function borrowBalance(address bathToken) public returns (uint256 balance) {

74:     ) public view returns (uint256 balance) {

86:     function borrowRate(address bathToken) public view returns (uint256 rate) {

130:     ) internal returns (bool OK) {

258:     ) internal returns (uint256 _bathTokenAmount) {

308:     ) internal view returns (uint256 _max) {

326:     ) internal view returns (uint256 _debt) {

354:     ) internal returns (uint256 _bathTokenAmount) {

531:     ) internal returns (uint256 _limit, uint256 _lastBorrow) {

```

#### Recommended Mitigation Steps:

add `{return x}` if you want to return the updated value or else remove `returns(uint)` from the `function(){}` if no value you wanted to return

### [L-10] OWNER CAN RENOUNCE OWNERSHIP

#### Description:

Typically, the contract’s owner is the account that deploys the contract. As a result, the owner is able to perform certain privileged activities.

The Openzeppelin’s Ownable used in this project contract implements `renounceOwnership`. This can represent a certain risk if the ownership is renounced for any other reason than by design. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

#### **Proof Of Concept**

```solidity
File: contracts/utilities/poolsUtility/Position.sol

15: contract Position is Ownable, DSMath {

```

#### Recommended Mitigation Steps:

We recommend to either reimplement the function to disable it or to clearly specify if it is part of the contract design.

### [L-11] REQUIRE MESSAGES ARE TOO SHORT AND UNCLEAR

#### Description:

The correct and clear error description explains to the user why the function reverts, but the error descriptions below in the project are not self-explanatory. These error descriptions are very important in the debug features of DApps like Tenderly. Error definitions should be added to the require block, not exceeding 32 bytes.

#### **Proof Of Concept**

```solidity
File: contracts/BathHouseV2.sol

68:         require(
                tokenToBathToken[underlying] == address(0),
            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
        );

72:         require(
                underlying != address(0),
            "createBathToken: UNDERLYING == ADDRESS 0"
        );

76:         require(
                implementation != address(0),
            "createBathToken: IMPLEMENTATION == ADDRESS 0"
        );

144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");

```

```solidity
File: contracts/RubiconMarket.sol

246:         require(isActive(id));

252:         require(isActive(id));

265:         require(!locked);

374:         require(
                _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
            "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
        );

379:         require(
                _offer.pay_gem.transfer(msg.sender, quantity),
            "_offer.pay_gem.transfer(msg.sender, quantity) failed"
        );

460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))

461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

488:         require(cancel(uint256(id)));

519:         require(uint128(pay_amt) == pay_amt);

520:         require(uint128(buy_amt) == buy_amt);

521:         require(pay_amt > 0);

522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

523:         require(buy_amt > 0);

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

525:         require(pay_gem != buy_gem);

538:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

565:         require(buy(uint256(id), maxTakeAmount));

581:         require(amount > 0);

598:         require(!isClosed());

604:         require(isActive(id));

605:         require(!isClosed());

611:         require(isActive(id));

612:         require(
                (msg.sender == getOwner(id)) ||
                isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );

702:         require(_feeTo != address(0));

720:         require(isActive(id), "Offer was deleted or taken, or never existed.");

721:         require(
                isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );

753:         require(buy(uint256(id), maxTakeAmount));

757:         require(cancel(uint256(id)));

835:         require(_dust[address(pay_gem)] <= pay_amt);

877:                 require(_unsort(id));

938:         require(!locked);

940:         require(
                !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );

1034:         require(!locked);

1075:         require(!locked);

1107:         require(
                fill_amt <= max_fill_amount,
            "fill_amt exceeds max_fill_amount"
        );

1136:                 require(offerId != 0); //Fails if there are not enough offers to complete

1169:                 require(offerId != 0); //Fails if there are not enough offers to complete

1184:         require(buyEnabled);

1194:         require(super.buy(id, amount));

1209:         require(id > 0);

1226:         require(id > 0);

1366:         require(isActive(id));

1410:         require(_span[pay_gem][buy_gem] > 0);

1412:         require(
                _rank[id].delb == 0 && //assert id is in the sorted list
                isOfferSorted(id)
        );

1419:             require(_rank[_rank[id].next].prev == id);

1428:             require(_rank[_rank[id].prev].next == id);

1477:         require(newFeeTo != address(0));

```

```solidity
File: contracts/V2Migrator.sol

63:         require(
                IERC20(bathTokenV2).balanceOf(address(this)) == 0,
            "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
        );

```

```solidity
File: contracts/periphery/BathBuddy.sol

88:         require(msg.sender == owner);

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

236:         require(
                block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );

```

```solidity
File: contracts/utilities/FeeWrapper.sol

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

76:         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

100:         require(
                openPosition(quote, asset, quotePayAmount, leverage),
            "buyAllAmountWithLeverage: FAILED TO OPEN POSITION"
        );

114:         require(
                openPosition(asset, quote, assetPayAmount, leverage),
            "sellAllAmountWithLeverage: FAILED TO OPEN POSITION"
        );

340:         require(_errs[0] == 0);

591:             ? require(
                    _leverage > _wad && _leverage <= _leverageMax,
                "_leverageCheck{Long}: INVLAID LEVERAGE"
            )

595:             : require(
                    _leverage >= _wad && _leverage <= _leverageMax,
                "_leverageCheck{Short}: INVLAID LEVERAGE"
            );

```

#### Recommended Mitigation Steps:

Error definitions should be added to the require block, not exceeding 32 bytes or we should use custom errors.

### [L-12] THE SAFETRANSFER FUNCTION DOES NOT CHECK FOR POTENTIALLY SELF-DESTROYED TOKENS

#### Description:

If a pair gets created and after a while one of the tokens gets self-destroyed (maybe because of a bug) then `safeTransfer` would still succeed. It’s probably a good idea to check if the contract still exists by checking the bytecode length.

#### **Proof Of Concept**

```solidity
File: contracts/periphery/BathBuddy.sol

182:             rewardsToken.safeTransfer(holderRecipient, reward);

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

243:         IERC20(token).safeTransfer(msg.sender, amount);

```

### [L-13] MODIFIER SIDE-EFFECTS

#### Description:

Modifiers should only implement checks and not make state changes and external calls which violates the [checks-effects-interactions](https://solidity.readthedocs.io/en/develop/security-considerations.html#use-the-checks-effects-interactions-pattern) pattern. These side-effects may go unnoticed by developers/auditors because the modifier code is typically far from the function implementation.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

643:     modifier note() {
            bytes32 foo;
            bytes32 bar;
            uint256 wad;

            assembly {
                foo := calldataload(4)
                bar := calldataload(36)
                wad := callvalue()
            }

            emit LogNote(msg.sig, msg.sender, foo, bar, wad, msg.data);

            _;
    }

```

```solidity
File: contracts/periphery/BathBuddy.sol

247:     modifier updateReward(address account, address token) {
                rewardsPerTokensStored[token] = rewardPerToken(token);
            lastUpdateTime[token] = lastTimeRewardApplicable(token);
            if (account != address(0)) {
                tokenRewards[token][account] = earned(account, token);
                userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
                    token
                ];
            }
            _;
    }

```

### [L-14] A SINGLE POINT OF FAILURE

#### Description:

The owner role has a single point of failure and `onlyOwner` can use critical a few functions.

Owner is not behind a multisig and changes are not behind a timelock.

Even if protocol admins/developers are not malicious there is still a chance for Owner keys to be stolen. In such a case, the attacker can cause serious damage to the project due to important functions. In such a case, users who have invested in project will suffer high financial losses.

#### **Proof Of Concept**

```solidity
File: contracts/periphery/BathBuddy.sol

87:     modifier onlyOwner() {

191:         function notifyRewardAmount(
                uint256 reward,
                IERC20 rewardsToken
            ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

232:         function setRewardsDuration(
                uint256 _rewardsDuration,
                address token
            ) external onlyOwner {

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

93:         function buyAllAmountWithLeverage(
                address quote,
                address asset,
                uint256 quotePayAmount,
                uint256 leverage
            ) external onlyOwner {

107:         function sellAllAmountWithLeverage(
                address asset,
                address quote,
                uint256 assetPayAmount,
                uint256 leverage
            ) external onlyOwner {

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {

```

#### Recommended Mitigation Steps:

Add a time lock to critical functions. Admin-only functions that change critical parameters should emit events and have timelocks.

Events allow capturing the changed parameters so that off-chain tools/interfaces can register such changes with timelocks that allow users to evaluate them and consider if they would like to engage/exit based on how they perceive the changes as affecting the trustworthiness of the protocol or profitability of the implemented financial services.

Allow only multi-signature wallets to call the function to reduce the likelihood of an attack.

https://twitter.com/danielvf/status/1572963475101556738?s=20&t=V1kvzfJlsx-D2hfnG0OmuQ

Also detail them in documentation and NatSpec comments.

### [L-15] BLOCK VALUES AS A PROXY FOR TIME

#### Description:

Contracts often need access to time values to perform certain types of functionality. Values such as block.timestamp, and block.number can give you a sense of the current time or a time delta, however, they are not safe to use for most purposes.

In the case of block.timestamp, developers often attempt to use it to trigger time-dependent events. As Ethereum is decentralized, nodes can synchronize time only to some degree. Moreover, malicious miners can alter the timestamp of their blocks, especially if they can gain advantages by doing so. However, miners cant set a timestamp smaller than the previous one (otherwise the block will be rejected), nor can they set the timestamp too far ahead in the future. Taking all of the above into consideration, developers cant rely on the preciseness of the provided timestamp.

As for block.number, considering the block time on Ethereum is generally about 14 seconds, it`s possible to predict the time delta between blocks. However, block times are not constant and are subject to change for a variety of reasons, e.g. fork reorganisations and the difficulty bomb. Due to variable block times, block.number should also not be relied on for precise calculations of time.

Reference: https://swcregistry.io/docs/SWC-116
Reference: (https://github.com/kadenzipfel/smart-contract-vulnerabilities/blob/master/vulnerabilities/timestamp-dependence.md)

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

534:         info.timestamp = uint64(block.timestamp);

625:         return uint64(block.timestamp);

943:                 _rank[id].delb < block.number - 10

1433:         _rank[id].delb = block.number; //mark _rank[id] for deletion

```

```solidity
File: contracts/periphery/BathBuddy.sol

116:             block.timestamp < periodFinish[token]

117:                 ? block.timestamp

195:         if (block.timestamp >= periodFinish[address(rewardsToken)]) {

201:                 block.timestamp

223:         lastUpdateTime[address(rewardsToken)] = block.timestamp;

224:         periodFinish[address(rewardsToken)] = block.timestamp.add(

237:             block.timestamp > periodFinish[token],

```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

327:         uint256 _blockDelta = block.number - _startBlock;

413:             block.number,

```

### [L-16] ACCOUNT EXISTENCE CHECK FOR LOW-LEVEL CALLS

#### Description:

Low-level calls `call`/`delegatecall`/`staticcall` return true even if the account called is non-existent (per EVM design). Account existence must be checked prior to calling if needed.

Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#low-level-callsn

#### **Proof Of Concept**

```solidity
File: contracts/utilities/FeeWrapper.sol

82:         (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(

118:         (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");

```

#### Recommended Mitigation Steps:

In addition to the zero-address checks, add a check to verify that <address>.code.length > 0

### [L-17] USE `SAFETRANSFER` INSTEAD OF `TRANSFER`

#### Description:

It is good to add a `require()` statement that checks the return value of token transfers or to use something like OpenZeppelin’s `safeTransfer`/`safeTransferFrom` unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

For example, Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)‘s `transfer()` and `transferFrom()` functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert.

#### **Proof Of Concept**

```solidity
File: contracts/RubiconMarket.sol

380:             _offer.pay_gem.transfer(msg.sender, quantity),

381:             "_offer.pay_gem.transfer(msg.sender, quantity) failed"

460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))

461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

```

```solidity
File: contracts/V2Migrator.sol

59:         IERC20(bathTokenV2).transfer(

```

```solidity
File: contracts/utilities/FeeWrapper.sol

102:         IERC20(_feeToken).transfer(_feeTo, _feeAmount);

```

#### Recommended Mitigation Steps:

Consider using `safeTransfer`/`safeTransferFrom` or `require()` consistently.

### [L-18] VOID CONSTRUCTOR

#### Description:

Calls to base contract constructors that are unimplemented leads to misplaced assumptions. Check if the constructor is implemented or remove call if not.

Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#void-constructor

#### **Proof Of Concept**

```solidity
File: contracts/utilities/poolsUtility/Position.sol

54:     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

```
