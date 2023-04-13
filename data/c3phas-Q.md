### Table Of Contents
- [Findings](#findings)
- [Unused import file](#unused-import-file)
- [Consider using OpenZeppelin’s SafeCast library to prevent unexpected overflows when casting from uint256](#consider-using-openzeppelins-safecast-library-to-prevent-unexpected-overflows-when-casting-from-uint256)
  - [Recommended Mitigation Steps:](#recommended-mitigation-steps)
- [Use openzeppelin initializer](#use-openzeppelin-initializer)
- [SafeMath no longer required](#safemath-no-longer-required)
- [Rename the following function](#rename-the-following-function)
  - [Constants should be defined rather than using magic numbers](#constants-should-be-defined-rather-than-using-magic-numbers)
- [Natspec is incomplete/missing](#natspec-is-incompletemissing)
- [Unused named return](#unused-named-return)
- [Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. `10**18`)](#use-scientific-notation-eg-1e18-rather-than-exponentiation-eg-1018)
- [Lack of event emission after critical  functions](#lack-of-event-emission-after-critical--functions)
- [Lack of event emission for setters](#lack-of-event-emission-for-setters)
- [Unused modifers](#unused-modifers)
  - [The nonReentrant modifier should occur before all other modifiers](#the-nonreentrant-modifier-should-occur-before-all-other-modifiers)
- [Shorthand way to write if / else statement](#shorthand-way-to-write-if--else-statement)
- [Function can be marked as view](#function-can-be-marked-as-view)
- [Commented code should be removed](#commented-code-should-be-removed)
- [Lack of consistenty between uint vs uint256](#lack-of-consistenty-between-uint-vs-uint256)
  - [Code Structure Deviates From Best-Practice](#code-structure-deviates-from-best-practice)
  - [Function \&\& Variable Naming Convention](#function--variable-naming-convention)

## Findings

## Unused import file
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L7
```solidity
File: /contracts/periphery/BathBuddy.sol
7:import "@openzeppelin/contracts/security/Pausable.sol";
```

## Consider using OpenZeppelin’s SafeCast library to prevent unexpected overflows when casting from uint256
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1061
```solidity
File: /contracts/RubiconMarket.sol
1061:                take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
```

`baux` is a uint256 variable which is being cast to uint128.
While Solidity 0.8.x checks for overflows on arithmetic operations, it does not do so for casting – the OpenZeppelin SafeCast library offers this

**Other instances*
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1103

### Recommended Mitigation Steps:

Consider using OpenZeppelin’s SafeCast library to prevent unexpected overflows when casting from uint256.


## Use openzeppelin initializer
To prevent a contract from being _initialized_ multiple times, you need to add a check to ensure the `initialize` function is called only once:
We can see that implemented in the following by using a variable `initialized`

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700-L701
```solidity
File: /contracts/RubiconMarket.sol
700:    function initialize(address _feeTo) public {
701:        require(!initialized, "contract is already initialized");
```
**Another instance**
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32-L39


OpenZeppelin Contracts provides an `Initializable` base contract that has an `initializer` modifier that takes care of this:
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/proxy/utils/Initializable.sol

## SafeMath no longer required
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L6
```solidity
File: /contracts/periphery/BathBuddy.sol
6:import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```

```solidity
39:    using SafeMath for uint256;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L6
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
6:import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```

The safeMath pattern has been used extensively throughout the inscope contracts. Using the above as an example

## Rename the following function
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1273-L1282

```solidity
File: /contracts/RubiconMarket.sol

//@audit: rename _matcho to _match
1273:    function _matcho(
1274:        uint256 t_pay_amt, //taker sell how much
1275:        ERC20 t_pay_gem, //taker sell which token
1276:        uint256 t_buy_amt, //taker buy how much
1277:        ERC20 t_buy_gem, //taker buy which token
1278:        uint256 pos, //position id
1279:        bool rounding, //match "close enough" orders?
1280:        address owner,
1281:        address recipient
1282:    ) internal returns (uint256 id) {
```

The comment describe this function as follows
`    //match offers with taker offer, and execute token transactions`
I suppose a small typo occured here and the function was named `_matcho instead of _match`


### Constants should be defined rather than using magic numbers
There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. Additionally, for complex values, inline comments explaining how they were calculated or why they were chosen are highly recommended. 
Following [Solidity’s style guide](https://solidity.readthedocs.io/en/latest/style-guide.html#constants), constants should be named in UPPER_CASE_WITH_UNDERSCORES format, and specific public getters should be defined to read each one of them.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1057
```solidity
File: /contracts/RubiconMarket.sol

//@audit: 10 ** 9
1057:                    mul(pay_amt, 10 ** 9),

//@audit: 10 ** 9
1059:                ) / 10 ** 9;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L459
```solidity
File: /contracts/utilities/poolsUtility/Position.sol

//@audit: 10000
459:        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);

//@audit: 10000
481:        uint256 _fee = _minFill.mul(_feeBPS).div(10000);

//@audit: 10000
490:        _fee = _payAmount.mul(_feeBPS).div(10000);
```


## Natspec is incomplete/missing
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276
```solidity
File:/contracts/RubiconMarket.sol

//@audit: @param id
276:    function isActive(uint256 id) public view returns (bool active) {

//@audit: @param id
280:    function getOwner(uint256 id) public view returns (address owner) {

//@audit: @param id
284:    function getRecipient(uint256 id) public view returns (address owner) {

//@audit: @param id
288:    function getOffer(
289:        uint256 id
290:    ) public view returns (uint256, ERC20, uint256, ERC20) {

//@audit: @param id_
297:    function bump(bytes32 id_) external can_buy(uint256(id_)) {

//@audit: @param id,@param quantity
314:    function buy(
315:        uint256 id,
316:        uint256 quantity
317:    ) public virtual can_buy(id) synchronized returns (bool) {

//@audit: @param id
//@audit: @return success
452:    function cancel(
453:        uint256 id
454:    ) public virtual can_cancel(id) synchronized returns (bool success) {

//@audit: @param id
487:    function kill(bytes32 id) external virtual {

//@audit: @param pay_gem,buy_gem,pay_amt,buy_amt
//@audit: @return id
491:    function make(
492:        ERC20 pay_gem,
493:        ERC20 buy_gem,
494:        uint128 pay_amt,
495:        uint128 buy_amt
496:    ) external virtual returns (bytes32 id) {

//@audit: @param pay_amt,pay_gem,buy_amt,buy_gem,owner,recipient
//@audit: @return id
511:    function offer(
512:        uint256 pay_amt,
513:        ERC20 pay_gem,
514:        uint256 buy_amt,
515:        ERC20 buy_gem,
516:        address owner,
517:        address recipient
518:    ) public virtual can_offer synchronized returns (uint256 id) {

//@audit: @param id, maxtakeAmount
564:    function take(bytes32 id, uint128 maxTakeAmount) external virtual {

//@audit: @param amount
578:    function calcAmountAfterFee(
579:        uint256 amount
580:    ) public view returns (uint256 _amount) {

//@audit: @param pay_gem,pay_gem,pay_amt,buy_amt
733:    function make(
734:        ERC20 pay_gem,
735:        ERC20 pay_gem,
736:        uint128 pay_amt,
737:        uint128 buy_amt
738:    ) public override returns (bytes32) {

//@audit: @param id,maxTakeAmount
752:    function take(bytes32 id, uint128 maxTakeAmount) public override {

//@audit: @param pay_amount,pay_gem,buy_amount,buy_gem
761:    function offer(
762:        uint256 pay_amt, //maker (ask) sell how much
763:        ERC20 pay_gem, //maker (ask) sell which token
764:        uint256 buy_amt, //maker (ask) buy how much
765:        ERC20 buy_gem //maker (ask) buy which token
766:    ) external can_offer returns (uint256) {

//@audit: @param pay_amount,pay_gem,buy_amount,buy_gem,pos,rounding
780:    function offer(
781:        uint256 pay_amt, //maker (ask) sell how much
782:        ERC20 pay_gem, //maker (ask) sell which token
783:        uint256 buy_amt, //maker (ask) buy how much
784:        ERC20 buy_gem, //maker (ask) buy which token
785:        uint pos, //position to insert offer, 0 should be used if unknown
786:        bool rounding
787:    ) external can_offer returns (uint256) {

//@audit: @param pay_amount,pay_gem,buy_amount,buy_gem,pos,owner,recipient
802:    function offer(
803:        uint256 pay_amt, //maker (ask) sell how much
804:        ERC20 pay_gem, //maker (ask) sell which token
805:        uint256 buy_amt, //maker (ask) buy how much
806:        ERC20 buy_gem, //maker (ask) buy which token
807:        uint256 pos, //position to insert offer, 0 should be used if unknown
808:        address owner,
809:        address recipient
810:    ) external can_offer returns (uint256) {

//@audit: @param id, amount
855:    function buy(
856:        uint256 id,
857:        uint256 amount
858:    ) public override can_buy(id) returns (bool) {

//@audit: @param id
//@audit: @return success
871:    function cancel(
872:        uint256 id
873:    ) public override can_cancel(id) returns (bool success) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L65
```solidity
File: /contracts/utilities/poolsUtility/Position.sol

//@audit: @param bathToken
65:    function borrowBalance(address bathToken) public returns (uint256 balance) {

//@audit: @param posId
72:    function borrowBalanceOfPos(
73:        uint256 posId
74:    ) public view returns (uint256 balance) {

//@audit: @param bathToken
//@audit: @return rate
86:    function borrowRate(address bathToken) public view returns (uint256 rate) {

//@audit: @param quote,asset,quotePayAmount,leverage
93:    function buyAllAmountWithLeverage(
94:        address quote,
95:        address asset,
96:        uint256 quotePayAmount,
97:        uint256 leverage
98:    ) external onlyOwner {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L28-L36
```solidity
File: /contracts/utilities/FeeWrapper.sol

//@audit: @param tokenAmounts, feeType,feeValue
//@audit: @return amountsWithFee,fees
28:    function calculateFee(
29:        uint256[] memory tokenAmounts,
30:        uint256 feeType,
31:        uint256 feeValue
32:    )
33:        external
34:        view
35:        returns (uint256[] memory amountsWithFee, uint256[] memory fees)
36:    {
```

## Unused named return
Using both named returns and a return statement isn’t necessary in  a function.To  improve code quality, consider using only one of those.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276
```solidity
File:/contracts/RubiconMarket.sol

//@audit: active
276:    function isActive(uint256 id) public view returns (bool active) {

//@audit: owner
280:    function getOwner(uint256 id) public view returns (address owner) {

//@audit: owner
284:    function getRecipient(uint256 id) public view returns (address owner) {
```

## Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. `10**18`)
While the compiler knows to optimize away the exponentiation, it's still better coding practice to use idioms that do not require compiler optimization, if they exist

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75
```solidity
File: /contracts/RubiconMarket.sol
74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317
```solidity
File: /contracts/utilities/poolsUtility/Position.sol

//@audit: 10 ** 18
317:        _max = (_liq.mul(10 ** 18)).div(_price);

//@audit: 10 ** 18
331:        ).div(10 ** 18);
```

## Lack of event emission after critical  functions
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32-L39
```solidity
File: /contracts/BathHouseV2.sol
32:    function initialize(address _comptroller, address _pAdmin) external {
33:        require(!initialized, "BathHouseV2 already initialized!");
34:        comptroller = Comptroller(_comptroller);
35:        admin = msg.sender;
36:        proxyAdmin = _pAdmin;

38:        initialized = true;
39:    }
```


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71-L85
```solidity
File: /contracts/periphery/BathBuddy.sol
71:    function spawnBuddy(
72:        address _owner,
73:        address newBud,
74:        address _bathHouse
75:    ) external {
76:        require(!friendshipStarted, "I already have a buddy!");
77:        owner = _owner;
78:        myBathTokenBuddy = newBud;
79:        bathHouse = _bathHouse;

84:        friendshipStarted = true;
85:    }
```

## Lack of event emission for setters
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466
```solidity
File: /contracts/RubiconMarket.sol
1466:    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:    function setFeeTo(address newFeeTo) external auth returns (bool) {
```

## Unused modifers
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L92-L102
```solidity
File: /contracts/periphery/BathBuddy.sol
92:    // TODO: do we need this?
93:    // Enforce only soul-bound Bath Token has calling rights
94:    modifier onlyBuddy() {
95:        require(
96:            msg.sender == myBathTokenBuddy &&
97:                msg.sender != address(0) &&
98:                friendshipStarted,
99:            "You are not my buddy!"
100:        );
101:        _;
102:    }
```
The above modifier has not been used anywhere in the code, as such we could get rid of it

### The nonReentrant modifier should occur before all other modifiers
This is a best-practice to protect against reentrancy in other modifiers

In the instance shown below, we have a modifier `synchronized` but looking at it's implementation, all it's doing is serve as non reentrant modifier.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L314-L317
```solidity
File: /contracts/RubiconMarket.sol
314:    function buy(
315:        uint256 id,
316:        uint256 quantity
317:    ) public virtual can_buy(id) synchronized returns (bool) {
```

The `synchronized modifer` serves as a non reentrant modifier as such we should have it occur before the other modifiers
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L264-L269
```solidity
File: /contracts/RubiconMarket.sol
264:    modifier synchronized() {
265:        require(!locked);
266:        locked = true;
267:        _;
268:        locked = false;
269:    }
```


## Shorthand way to write if / else statement

The normal if / else statement can be refactored in a shorthand way to write it:

1.  Increases readability
2.  Shortens the overall SLOC.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L36-L40
```solidity
File: /contracts/RubiconMarket.sol
36:        if (src == owner) {
37:            return true;
38:        } else {
39:            return false;
40:        }
```

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..979768c 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -33,11 +33,7 @@ contract DSAuth is DSAuthEvents {
     }

     function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
+        return src == owner ? true: false;
     }
 }
```

## Function can be marked as view
If no state changes are done within a function, then the said function can be declared as view
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297-L310
```solidity
File: /contracts/RubiconMarket.sol
297:    function bump(bytes32 id_) external can_buy(uint256(id_)) {
298:        uint256 id = uint256(id_);
299:        /// TODO: double check it is sound logic to kill this event
300:        /// emit LogBump(
301:        ///     id_,
302:        ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
303:        ///     offers[id].owner,
304:        ///     offers[id].pay_gem,
305:        ///     offers[id].buy_gem,
306:        ///     uint128(offers[id].pay_amt),
307:        ///     uint128(offers[id].buy_amt),
308:        ///     offers[id].timestamp
309:        /// );
310:    }
```

The initial intention was to have the function emit an event which would have made this not a view function, however the emit code has been commented out which allows us to declare this function as `view`. 

We can however uncoment the code(event) and keep the function as it was


## Commented code should be removed

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L4-L5
```solidity
File: /contracts/RubiconMarket.sol
4: // Uncomment this line to use console.log
5: // import "hardhat/console.sol";
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L100-L116
```solidity
File: /contracts/RubiconMarket.sol
100:    /// event LogTrade(
101:    ///     uint256 pay_amt,
102:    ///     address indexed pay_gem,
103:    ///     uint256 buy_amt,
104:    ///     address indexed buy_gem
105:    /// );


107:    /// event LogMake(
108:    ///     bytes32 indexed id,
109:    ///     bytes32 indexed pair,
110:    ///     address indexed maker,
111:    ///     ERC20 pay_gem,
112:    ///     ERC20 buy_gem,
113:    ///     uint128 pay_amt,
114:    ///     uint128 buy_amt,
115:    ///     uint64 timestamp
116:    /// );
```

**Other instances**
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L129-L150
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L163-L172
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L187-L195

## Lack of consistenty between uint vs uint256

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L780-L787
```solidity
File: /contracts/RubiconMarket.sol

//@audit: uint256 pay_amt and uint pos
780:    function offer(
781:        uint256 pay_amt, //maker (ask) sell how much
782:        ERC20 pay_gem, //maker (ask) sell which token
783:        uint256 buy_amt, //maker (ask) buy how much
784:        ERC20 buy_gem, //maker (ask) buy which token
785:        uint pos, //position to insert offer, 0 should be used if unknown
786:        bool rounding
787:    ) external can_offer returns (uint256) {
```

As both uint and uint256 represent the same thing, I propose we stick to uint256 as it clearly depicts the size ie 256


### Code Structure Deviates From Best-Practice
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier. Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Some constructs deviate from this recommended best-practice: Modifiers are in the middle of contracts. External/public functions are mixed with internal/private ones. Few events are declared in contracts while most others are in corresponding interfaces.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L65
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
65:    function borrowBalance(address bathToken) public returns (uint256 balance) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L93-L98
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
93:    function buyAllAmountWithLeverage(
94:        address quote,
95:        address asset,
96:        uint256 quotePayAmount,
97:        uint256 leverage
98:    ) external onlyOwner {
```

From above we see a public function declared before an external, per the docs it is recommended to have all external functions come first then public

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L259
In the above line , we have events being declared at the end of the file and also modifiers


 **Recommendation:**
Consider adopting recommended best-practice for code structure and layout.

See: [https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-layout](https://docs.soliditylang.org/en/v0.8.15/style-guide.html#order-of-layout)


### Function && Variable Naming Convention
Description:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the mixedCase format unless variables are declared as constant in which case they utilize the UPPER_CASE_WITH_UNDERSCORES format. Internal/private functions and variables should lead with an _underscore.

On this particular file ie Position.sol, internal functions are named with an underscore prefix eg `function _borrow`  which is in accordance to the docs suggestions
However the following function is not following the same style.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L125-L130
```solidity
File: /contracts/utilities/poolsUtility/Position.sol
125:    function openPosition(
126:        address asset,
127:        address quote,
128:        uint256 initMargin,
129:        uint256 leverage
130:    ) internal returns (bool OK) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71-L75
```solidity
File: /contracts/periphery/BathBuddy.sol

//@audit: newBud should be renamed to _newBud
71:    function spawnBuddy(
72:        address _owner,
73:        address newBud,
74:        address _bathHouse
75:    ) external {
```
Rename the function parameter `newBud` to `_newBud` to be consistent with the other parameters

**Recommendation:**
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the Solidity style guide or be consistent with one particular style.

