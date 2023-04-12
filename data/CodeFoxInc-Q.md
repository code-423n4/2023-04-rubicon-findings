# Low-risk and Non-critical issues

## L-01 Use `safeTransferFrom` instead of `transferFrom`

In line 340, use `safeTransferFrom` instead of `transferFrom` to avoid some abnormal situations. Some ERC20 token returns no value and it can be handled by `safeTransferFrom`. 

In the code base some contracts use the library `SafeERC20`, but some do not use. 

Please keep the consistency of using the library and always follow the best practice when interacting with some external tokens. 

If it is not followed, when an ERC20 token which is not fully following the ERC20 standard is listed, it can cause some unexpected behaviors in the protocol. 

```solidity
File: BathBuddy.sol
40:     using SafeERC20 for IERC20; // @audit it uses the library here
```

```solidity
File: RubiconMarket.sol
337:         /// @dev Fee logic added on taker trades
338:         uint256 fee = mul(spend, feeBPS) / 100_000;
339:         require(
340:             _offer.buy_gem.transferFrom(msg.sender, feeTo, fee), // @audit low-risk use safetransferfrom instead, because some erc20 token has no return value
341:             "Insufficient funds to cover fee"
342:         );
```

```solidity
File: RubiconMarket.sol
452:     function cancel(
453:         uint256 id
454:     ) public virtual can_cancel(id) synchronized returns (bool success) {
455:         OfferInfo memory _offer = offers[id];
456:         delete offers[id];
457: 
458:         /// @dev V1 orders after V2 upgrade will point to address(0) in owner
459:         _offer.owner == address(0) && msg.sender == _offer.recipient
460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt)) // @audit low-risk use safetransferfrom instead, because some erc20 token has no return value
461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt)); // @audit low-risk use safetransferfrom instead, because some erc20 token has no return value
```

```solidity
File: RubiconMarket.sol
539: 
540:         require(pay_gem.transferFrom(msg.sender, address(this), pay_amt)); // @audit use safeTransferFrom instead. Because some ERC20 token does not return a value
541:
```

### Recommendation

Change the function to the functions in the `SafeERC20` library. I will give some examples here: 

```diff
		function cancel(
        uint256 id
    ) public virtual can_cancel(id) synchronized returns (bool success) {
        OfferInfo memory _offer = offers[id];
        delete offers[id];

        /// @dev V1 orders after V2 upgrade will point to address(0) in owner
        _offer.owner == address(0) && msg.sender == _offer.recipient
-            ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt)) // @audit low-risk use safetransferfrom instead, because some erc20 token has no return value
+            ? require(_offer.pay_gem.safeTransfer(_offer.recipient, _offer.pay_amt)) 
-            : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt)); // @audit low-risk use safetransferfrom instead, because some erc20 token has no return value
+            : require(_offer.pay_gem.safeTransfer(_offer.owner, _offer.pay_amt)); // @audit low-risk use safetransferfrom instead, because some erc20 token has no return value
```

```diff
- require(pay_gem.transferFrom(msg.sender, address(this), pay_amt)); // @audit use safeTransferFrom instead. Because some ERC20 token does not return a value
+ require(pay_gem.safeTransferFrom(msg.sender, address(this), pay_amt)); 
```

## L-02 Make sure the `recipient` is not `address(0)`

In the below code, there is no arguments’ validation to make sure the recipient is not zero address. I recommend add the validation code. 

```solidity
File: RubiconMarket.sol
510:     /// @notice Key function to make a new offer. Takes funds from the caller into market escrow.
511:     function offer(
512:         uint256 pay_amt,
513:         ERC20 pay_gem,
514:         uint256 buy_amt,
515:         ERC20 buy_gem,
516:         address owner,
517:         address recipient
518:     ) public virtual can_offer synchronized returns (uint256 id) {
519:         require(uint128(pay_amt) == pay_amt);
520:         require(uint128(buy_amt) == buy_amt);
521:         require(pay_amt > 0); // @audit != 0 to improve gas
522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
523:         require(buy_amt > 0);
524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
525:         require(pay_gem != buy_gem);
```

```diff
/// @notice Key function to make a new offer. Takes funds from the caller into market escrow.
    function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
        require(uint128(pay_amt) == pay_amt);
        require(uint128(buy_amt) == buy_amt);
        require(pay_amt > 0); // @audit != 0 to improve gas
        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
        require(buy_amt > 0);
        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
        require(pay_gem != buy_gem);
+       require(recipient != address(0), "recipent is address(0)");
+       require(owner != address(0), ""recipent is address(0)"");
```

## NC-01 Use a newer solidity version and make it fixed

Through out the code base, the used solidity version is floating and not following the best practice. It is recommended use a static version of solidity instead of a floating version in production phase. 

Old version of Solidity is used , newer version can be used `(0.8.18)`. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1-L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1-L2)

```solidity
File: BathBuddy.sol
1: // SPDX-License-Identifier: MIT
2: pragma solidity ^0.8.0;
3: // @audit 1. do not use a floating version and use a fixed version // 2. use a newer version of solidity
```

```solidity
File: RubiconMarket.sol
1: /// SPDX-License-Identifier: AGPL-3.0
2: pragma solidity ^0.8.9; // @audit use a fiexed and newer version instead
```

### Recommendation

Follow other files about the solidity version. And the recommended version now is ``.

As recommended in the [slither document](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity), `0.8.18` is a good option. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L5)

```diff
-    pragma solidity ^0.8.0;
+    pragma solidity 0.8.18;
```

## NC-02 SafeMath library is not necessary since solidity 0.8.0

Since `solidity 0.8.0` the `SafeMath` library is not necessary because the underflow and overflow restriction is imbedded in the normal calculation syntax. 

Delete the library from the code base and use the normal calculation syntax. At the same time, it can also save some gas. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L9](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L9)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L20](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L20)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L39](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L39)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L17](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L17)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L46-L60](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L46-L60)

## NC-03 The function does not follow the Solidity’s standard naming convention

The codes below is internal function and it should be named with `_`(underscore). Large part of the code base’s internal functions are named properly. And team should consistently follow the practice. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L120-L133](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L120-L133)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L680-L690](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L680-L690)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L278-L292](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L278-L292)

This is an example of how it should be changed. 

```diff
		...
    /// @param leverage - leverage multiplier - n*1e18
-    function openPosition(
+    function _openPosition(
-        address asset,
-        address quote,
-        uint256 initMargin,
-        uint256 leverage
+        address _asset,
+        address _quote,
+        uint256 _initMargin,
+        uint256 _leverage
-    ) internal returns (bool OK) {
+    ) internal returns (_bool OK) {
		...
```

## NC-04 Use IERC20 instead of full ERC20 contract

I noticed in several files, full ERC20 contract is imported. If you want to interact with the ERC20 token contract, only importing the IERC20 is supposed to be enough for the project. 

```solidity
File: RubiconMarket.sol
11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol"; // @audit import like this: https://github.com/code-423n4/2023-04-caviar/blob/cd8a92667bcb6657f70657183769c244d04c015c/src/EthRouter.sol#L31-L35

File: BathHouseV2.sol
4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol"; // @audit use IERC20 is enough, not the full ERC20 contract.

File: RubiconMarket.sol
11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol"; // @audit import like this: https://github.com/code-423n4/2023-04-caviar/blob/cd8a92667bcb6657f70657183769c244d04c015c/src/EthRouter.sol#L31-L35

File: Position.sol
5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L8](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L8)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L5)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L11](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L11)

### Recommendation

The IERC20 interface does not include `symbol()`, `name()`, and `decimals()` functions, as they are not part of the minimal ERC20 standard. 

However, many token contracts implement these functions as part of the ERC20 extension. To interact with these functions, you can create a custom interface that inherits from IERC20 and includes the necessary functions. Here's an example:

```solidity
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

interface IExtendedERC20 is IERC20 {
     function name() external view returns (string memory);
     function symbol() external view returns (string memory);
     function decimals() external view returns (uint8);
}
```

Now you can use this interface to interact with the name, symbol, and decimals functions ins other functions. 

## NC-05 Finish the TODOs and delete the unnecessary event

There are some TODOs still exist in the code base. And some events commented out. Deal with all of them before deploying the contracts. 

These are two examples in the code base: 

```solidity
File: RubiconMarket.sol
099:     /// TODO: double check it is sound logic to kill this event // @audit delete unused events
100:     /// event LogTrade(
101:     ///     uint256 pay_amt,
102:     ///     address indexed pay_gem,
103:     ///     uint256 buy_amt,
104:     ///     address indexed buy_gem
105:     /// );
106: 
107:     /// event LogMake(
108:     ///     bytes32 indexed id,
109:     ///     bytes32 indexed pair,
110:     ///     address indexed maker,
111:     ///     ERC20 pay_gem,
112:     ///     ERC20 buy_gem,
113:     ///     uint128 pay_amt,
114:     ///     uint128 buy_amt,
115:     ///     uint64 timestamp
116:     /// );
```

```solidity
File: RubiconMarket.sol
184:     /// TODO: double check it is sound logic to kill this event
185:     /// event LogInt(string lol, uint256 input);
186: 
187:     /// event FeeTake(
188:     ///     bytes32 indexed id,
189:     ///     bytes32 indexed pair,
190:     ///     ERC20 asset,
191:     ///     address indexed taker,
192:     ///     address feeTo,
193:     ///     uint256 feeAmt,
194:     ///     uint64 timestamp
195:     /// );
196: 
197:     /// TODO: we will need to make sure this emit is included in any taker pay maker scenario
198:     event emitFee(
199:         bytes32 indexed id,
200:         address indexed taker,
201:         address indexed feeTo,
202:         bytes32 pair,
203:         ERC20 asset,
204:         uint256 feeAmt
205:     );
```

```solidity
File: BathBuddy.sol
092:     // TODO: do we need this?
093:     // Enforce only soul-bound Bath Token has calling rights
094:     modifier onlyBuddy() { // @audit non-critical not used should be removed
095:         require(
096:             msg.sender == myBathTokenBuddy &&
097:                 msg.sender != address(0) &&
098:                 friendshipStarted,
099:             "You are not my buddy!"
100:         );
101:         _;
102:     }
```

## NC-06 The link in the comment is broken

```solidity
File: RubiconMarket.sol
8: /// @notice Please see the repository for this code at https://github.com/RubiconDeFi/rubicon-protocol-v1; // @audit For this codebase, it is not the correct link anymore.
```

## NC-07 Add NatSpec documentation

Generally the NatSpec documentation is not followed consistently in the code base. 

I suggest follow the practice and make the code more readable. 

These are some examples in the code base which isn’t following the practice  and please notice that there are multiple functions which are not following. 

```solidity
File: BathHouseV2.sol
135:     //============================= INTERNALS =============================
136:     // 🛀
137:     function _bathify(
138:         address _underlying
139:     )
140:         internal
141:         view
142:         returns (string memory _name, string memory _symbol, uint8 _decimals)
143:     {
144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");
145: 
146:         _name = string.concat("bath", ERC20(_underlying).symbol());
147:         _symbol = string.concat(_name, "v2"); // @audit non-critical token symbol is not consistent with bath token v1. v1 is "bath" + underlying token symbol. v2 is "bath" + underlying token symbol + "v2". name should be with the `v2` suffix as what is done in the BathTokenV1 contract.
148:         _decimals = ERC20(_underlying).decimals();
149:     }
150: }
```

```solidity
File: RubiconMarket.sol
312:     /// @notice Accept a given `quantity` of an offer. Transfers funds from caller/taker to offer maker, and from market to caller/taker.
313:     /// @notice The fee for taker trades is paid in this function.
314:     function buy(
315:         uint256 id,
316:         uint256 quantity
317:     ) public virtual can_buy(id) synchronized returns (bool) {
318:         OfferInfo memory _offer = offers[id];
319:         uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;
320: 
321:         require(uint128(spend) == spend, "spend is not an int");
322:         require(uint128(quantity) == quantity, "quantity is not an int");
323:
```

```solidity
File: RubiconMarket.sol
509: 
510:     /// @notice Key function to make a new offer. Takes funds from the caller into market escrow.
511:     function offer(
512:         uint256 pay_amt,
513:         ERC20 pay_gem,
514:         uint256 buy_amt,
515:         ERC20 buy_gem,
516:         address owner,
517:         address recipient
518:     ) public virtual can_offer synchronized returns (uint256 id) {
519:         require(uint128(pay_amt) == pay_amt);
520:         require(uint128(buy_amt) == buy_amt);
521:         require(pay_amt > 0); // @audit != 0 to improve gas consumption
522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
523:         require(buy_amt > 0);
524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
```

### Recommendation

Follow the best practice and add [Natspec documentation](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html) for all the functions in the code base. 

## NC-08 Only import the contract you need

When importing a file, we can only import the contracts we need. It can optimize our code size and save gas and improve the readability of the code. 

Through out the code base this is not followed, so I strongly recommend follow this practice. 

```solidity
File: BathHouseV2.sol
4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol"; // @audit use IERC20 is enough, not the full ERC20 contract.
5: import "./compound-v2-fork/InterestRateModel.sol"; // @audit import only the necessary part of the contract, not the whole contract. Use `import {<name>} from "<name>.sol";`
6: import "./compound-v2-fork/CErc20Delegator.sol";
7: import "./compound-v2-fork/Comptroller.sol";
8: import "./compound-v2-fork/Unitroller.sol";
9: import "./periphery/BathBuddy.sol";
```

### Recommendation

This is an example of how to do it. 

[https://github.com/code-423n4/2023-04-caviar/blob/cd8a92667bcb6657f70657183769c244d04c015c/src/EthRouter.sol#L31-L35](https://github.com/code-423n4/2023-04-caviar/blob/cd8a92667bcb6657f70657183769c244d04c015c/src/EthRouter.sol#L31-L35)

## NC-09 Set the error message consistently and follow the best practice

I noticed through out the code base, some `require` functions don’t have error messages and some of them have error messages but they are not following the best practice. 

```solidity
File: RubiconMarket.sol
598: 
599:     /// @dev After close_time has been reached, no new offers are allowed.
600:     modifier can_offer() override {
601:         require(!isClosed()); // @audit should add error message to improve the readability 
602:         _;
603:     }
604: 
605:     /// @dev After close, no new buys are allowed.
606:     modifier can_buy(uint256 id) override {
607:         require(isActive(id)); // @audit should add error message to improve the readability 
608:         require(!isClosed()); // @audit should add error message to improve the readability 
609:         _;
610:     }
611: 
612:     /// @dev After close, anyone can cancel an offer.
613:     modifier can_cancel(uint256 id) virtual override {
614:         require(isActive(id)); // @audit should add error message to improve the readability 
615:         require(
616:             (msg.sender == getOwner(id)) ||
617:                 isClosed() ||
618:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))
619:         ); // @audit should add error message
620:         _;
621:     }
```

Functions like `can_offer` and `can_buy` are just used as above. So they lacks error messages. 

And although the `can_cancel` function is overrode as below, the error message still lack consistency and is not following the best practice. 

```solidity
File: RubiconMarket.sol
720: 
721:     // // After close, anyone can cancel an offer
722:     modifier can_cancel(uint256 id) override {
723:         require(isActive(id), "Offer was deleted or taken, or never existed.");
724:         require(
725:             isClosed() ||
726:                 msg.sender == getOwner(id) ||
727:                 id == dustId ||
728:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
729:             "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
730:         ); // @audit gas put the sequence as this to improve the gas consumption: (msg.sender == getRecipient(id) && getOwner(id) == address(0)) || msg.sender == getOwner(id) || id == dustid || isClosed()
731:         _;
732:     }
```

### Recommendation

Follow the best practice by adding error messages like this: `<contract_name>: <error_message>`. Follow this rule through all of the code base please. 

```diff
		modifier can_cancel(uint256 id) override {
-        require(isActive(id), "Offer was deleted or taken, or never existed.");
+        require(isActive(id), "RubiconMarket: Offer was deleted or taken, or never existed.");
        require(
            isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
-            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
+.          "RubiconMarket: Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        ); 
        _;
    }
```

## NC-10 Make it emits events when storage is changed

The initializing functions in the code base always changes the storage of the contract. It is recommended emit the events when storages are changed. 

```solidity
File: RubiconMarket.sol
702:     /// @dev Proxy-safe initialization of storage
703:     function initialize(address _feeTo) public {
704:         require(!initialized, "contract is already initialized"); // @audit non-critical use openzeppelin's library to improve the future upgradeability of the contract. 
705:         require(_feeTo != address(0));
706: 
707:         /// @notice The market fee recipient
708:         feeTo = _feeTo; // @audit should add an event when storage changes
709: 
710:         owner = msg.sender; // @audit should add an event when storage changes
711:         emit LogSetOwner(msg.sender);
712: 
713:         /// @notice The starting fee on taker trades in basis points
714:         feeBPS = 1;
715: 
716:         initialized = true; // @audit should add an event when storage changes
717:         matchingEnabled = true; // @audit should add an event when storage changes
718:         buyEnabled = true; // @audit should add an event when storage changes
719:     }
```

Add more events because the storage is changed, e.g. periodFinish, lastUpdateTime, rewardRatesevent `RewardRateUpdated(address indexed token, uint256 newRewardRate); event LastUpdateTimeUpdated(address indexed token, uint256 newLastUpdateTime); event PeriodFinishUpdated(address indexed token, uint256 newPeriodFinish);`

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L189-L228](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L189-L228)

```diff
File: BathBuddy.sol
191:     function notifyRewardAmount(
192:         uint256 reward,
193:         IERC20 rewardsToken
194:     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
195:         if (block.timestamp >= periodFinish[address(rewardsToken)]) {
196:             rewardRates[address(rewardsToken)] = reward.div(
197:                 rewardsDuration[address(rewardsToken)]
198:             );
199:         } else {
200:             uint256 remaining = periodFinish[address(rewardsToken)].sub(
201:                 block.timestamp
202:             );
203:             uint256 leftover = remaining.mul(
204:                 rewardRates[address(rewardsToken)]
205:             );
206:             rewardRates[address(rewardsToken)] = reward.add(leftover).div(
207:                 rewardsDuration[address(rewardsToken)]
208:             );
209:         }
210: 
211:         // Ensure the provided reward amount is not more than the balance in the contract.
212:         // This keeps the reward rate in the right range, preventing overflows due to
213:         // very high values of rewardRate in the earned and rewardsPerToken functions;
214:         // Reward + leftover must be less than 2^256 / 10^18 to avoid overflow.
215:         // Note********** ERC20s must be here*************
216:         uint256 balance = rewardsToken.balanceOf(address(this));
217:         require(
218:             rewardRates[address(rewardsToken)] <=
219:                 balance.div(rewardsDuration[address(rewardsToken)]),
220:             "Provided reward too high"
221:         );
222: 
223:         lastUpdateTime[address(rewardsToken)] = block.timestamp;
224:         periodFinish[address(rewardsToken)] = block.timestamp.add(
225:             rewardsDuration[address(rewardsToken)]
226:         );
227:         emit RewardAdded(reward); // @audit add more events because the storage is changed, e.g. periodFinish, lastUpdateTime, rewardRatesevent `RewardRateUpdated(address indexed token, uint256 newRewardRate); event LastUpdateTimeUpdated(address indexed token, uint256 newLastUpdateTime); event PeriodFinishUpdated(address indexed token, uint256 newPeriodFinish);`
228:     }
```

## NC-11 Code can become simpler

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L34-L41](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L34-L41)

Make the function simpler like this: 

```diff
		function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) { // 
-            return true;
-        } else {
-            return false;
-        } // @audit non-critical change to this: `return src == owner;` for simplicity.
+        return src == owner; 
    }
```

## NC-12 Use the contract as `abstract` if it is possible

There is no storage inside the contract, so use it as library if it is possible. 

```solidity
File: RubiconMarket.sol
44: /// @notice DSMath library for safe math without integer overflow/underflow
45: contract DSMath {   // @audit use this contract as library instead of contract
```

### Recommendation

```diff
- contract DSMath {   // @audit use this contract as `abstract` instead of contract
+ abstract contract DSMath {   // @audit use this contract as library instead of contract
```

## NC-13 Improve the naming of the event

The naming of the events with `emit` is not necessary and we can rename it without it to improve the readability. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L118](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L118)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L152](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L152)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L198](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L198)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L209](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L209)

### Recommendation

```diff
-  event emitOffer( // @audit naming of preface `emit` may not be necessary
+  event Offer
```

```diff
-  event emitDelete(
+  event Delete(
```

```diff
-    event emitCancel(
+    event Cancel(
```

## NC-14 Variable name after `returns` is not needed

Variable name `owner` after `returns` is not needed when the function has the code `return` in the function. Always either `return` the value in the function or name it after the key word `returns`. Do not use both. 

```diff
-   function getRecipient(uint256 id) public view returns (address owner) {
+   function getRecipient(uint256 id) public view returns (address) {
        return offers[id].recipient; // @audit non-critical change address owner to address recipient
    }
```

## NC-15 Do not let anyone call the `initialize` function in implementation(logic) contract

In the upgradeable patterns, UUPS pattern has a vulnerability like this. This time the upgradeable pattern is Transparent Proxy, so leave the functions there can be called by anyone cause no problems as far as I know. 

But leaving the implementation contract’s initializing function which can be called by any random person out there is not a good practice. 

### Recommendation

For following the best practice, I recommend add a constructor function into the contract to prevent it from happening. 

And I also recommend inherit the OpenZeppelin’s `Initializable` contract because it includes `_disableInitializers` function. 

```solidity
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";
```

Code below is from the wizard by OpenZeppelin’s Wizard which follows the best practices. It includes a constructor function which will call the `_disableInitializers`. 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

contract MyToken is Initializable, ERC20Upgradeable {
    /// @custom:oz-upgrades-unsafe-allow constructor
    constructor() {
        _disableInitializers();
    }

    function initialize() initializer public {
        __ERC20_init("MyToken", "MTK");
    }
}
```

What you should do is: 

```diff
+ import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

+    constructor() {
+        _disableInitializers();
+    }
```

## NC-16 Argument not used should be left empty

The argument is not used in the function so you can ignore it. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L525-L531](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L525-L531)

```diff
		function _borrowLimit(
        address _bathToken,
-       address _asset, // @audit non-critical not used and should be removed 
+       address, 
        uint256 _assetAmount,
        uint256 _leverage
    ) internal returns (uint256 _limit, uint256 _lastBorrow) {
```

## NC-17 Can emit explicit event value and improve gas cost

In function `setRewardsDuration`, you can make the event more clear by adding `calldata` `token` as a value too. And using `calldata` instead of `memory` cache as event’s argument can also improve the gas cost. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L230-L242](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L230-L242)

### Recommendation

Change the code as below: 

```diff
		function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period" // @audit  too long error message and it costs a lot of gas. Should use shorter mesage or adopt the new error message controlling pattern. 
        );
        rewardsDuration[token] = _rewardsDuration;
-        emit RewardsDurationUpdated(rewardsDuration[token]); // @audit non-critical (token, _rewardsDuration)
+        emit RewardsDurationUpdated(_rewardsDuration, token); // @audit non-critical (token, _rewardsDuration)
    }
```


## NC-18 Set the `symbol` of BathToken V1 and V2 compatible

In `BathTokenV1` contract, the `symbol` and `name` is defined as follow: 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathTokenV1.sol#L258-L266](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathTokenV1.sol#L258-L266)

```solidity
File: BathTokenV1.sol
257:         require(!initialized);
258:         string memory _symbol = string(
259:             abi.encodePacked(("bath"), token.symbol())
260:         );
261:         symbol = _symbol;
```

But in `BathHouseV2` contract, the `symbol` and `name` is defined as follow: 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L143-L149](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L143-L149)

```solidity
File: BathHouseV2.sol
137:     function _bathify(
138:         address _underlying
139:     )
140:         internal
141:         view
142:         returns (string memory _name, string memory _symbol, uint8 _decimals)
143:     {
144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");
145: 
146:         _name = string.concat("bath", ERC20(_underlying).symbol());
147:         _symbol = string.concat(_name, "v2"); 
						// @audit token symbol is not consistent with bath token v1. v1 is "bath" + underlying token symbol. v2 is "bath" + underlying token symbol + "v2". name should be with the `v2` suffix as what is done in the BathTokenV1 contract.
148:         _decimals = ERC20(_underlying).decimals();
149:     }
```

Symbols should be consistent and compatible with each other no matter it is Bath Token V1 or V2. They should be "bath" + underlying token symbol without the `v2` suffix as determined by V1 version. 

In this way, in the `V2Migrator`’s constructor, we can use the code as follows to validate the arguments as I mentioned in my Med submission. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L29-L35](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L29-L35)

```solidity
File: V2Migrator.sol
27: 
28:     /// @dev underlying tokens should be the same for corresponding pools
29:     /// i.e. USDC -> USDC, WETH -> WETH, etc.
30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {
32:             // set v1 to v2 bathTokens // @audit add a validation check statement to ensure that the length of the two arrays are the same. And add a `require(IERC20(bathTokensV1[i]).symbol() == IERC20(bathTokenV2[i]).symbol());` -> this can make sure that the underlying tokens are the same and nothing will go as unexpected. -> symbol should be consistent with v1 and v2 it should be "bath" + underlying token symbol without the `v2` suffix. If the addresses is in disorder, then there is a risk that the bathToken is migrated to the wrong v2 bathToken, cause total mess of the system.
33:             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
34:         }
35:     }
```

### Recommendation

Change the code as follow to make the symbol and name consistent and compatible with each other.  

```diff
	function _bathify(
        address _underlying
    )
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)
    {
        require(_underlying != address(0), "_bathify: ADDRESS ZERO");

-        _name = string.concat("bath", ERC20(_underlying).symbol());
-        _symbol = string.concat(_name, "v2"); // @audit non-critical token symbol is not consistent with bath token v1. v1 is "bath" + underlying token symbol. v2 is "bath" + underlying token symbol + "v2". name should be with the `v2` suffix as what is done in the BathTokenV1 contract.
+       _symbol = string.concat("bath", ERC20(_underlying).symbol());
+				_name = string(abi.encodePacked(_symbol, (" v2")));
        _decimals = ERC20(_underlying).decimals();
    }
```