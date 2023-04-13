# Report
## Low Risk ##
### [L-1]: Many smart contracts in one file
**Context:**
[RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol) 

**Description:**
a large number of smart contracts in one file makes it difficult to work with them. 

**Recommendation:**
Make separate files for each smart contract for more convenient interaction with them.

### [L-2]: Split require() statements that use &&
**Context:**

[L95-L100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95-L100)

**Description:**
Instead of using the && operator in a single require statement to check multiple conditions you can use multiple require statements with 1 condition. it will be more convenient for users if they understand which specific condition is not met.

**Recommendation:**
Change to:
```
        require(msg.sender == myBathTokenBuddy, ''You are not my buddy!'');
        require(msg.sender != address(0), ''My buddy cannot be address(0)!''); 
        require(friendshipStarted, "I have not started a bathToken friendship!");
```

### [L-3]: Unsafe casting may overflow
**Context:**

1. ```uint128(_offer.pay_amt),``` [L480](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L480) 
1. ```uint128(_offer.buy_amt)``` [L481](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L481) 
1. ```take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need``` [L1061](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1061) 

**Description:**

While Solidity 0.8.x checks for overflows on arithmetic operations, it does not do so for casting.

**Recommendation:**

Use OpenZeppelin’s SafeCast library to prevent unexpected overflows.

### [L-4]: Omissions in Events
**Context:**

1. ```event LogSetOwner(address indexed owner);``` [L17](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L17) (the events should include the new value and old value where possible)
1. ```emit LogMinSell(address(pay_gem), dust);``` [L960](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L960) (the events should include the new value and old value where possible)

**Description:**

Events are generally emitted when sensitive changes are made to the contracts. Some events are missing important parameters. 

### [L-5]: Critical changes should use two-step procedure
**Context:**

1. ```function setOwner(address owner_) external auth {``` [L25](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25) 
1. ```function setMinSell(``` [L955](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955)
1. ```function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {``` [L1466](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466) 
1. ```function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {``` [L1471](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471) 
1. ```function setFeeTo(address newFeeTo) external auth returns (bool) {``` [L1476](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476) 
1. ```function setRewardsDuration(``` [L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232)
1. ```function _updateMargin(``` [L426](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L426)

**Recommendation:**

The best practice is to use two-step procedure for critical changes to make them less error-prone. 

### [L-6]: Owner can renounce ownership
**Context:**

```import "@openzeppelin/contracts/access/Ownable.sol";``` [L7](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L7) 

**Description:**

@openzeppelin/contracts/access/Ownable.sol' used in this project contract implements renounceOwnership() function. Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner.

**Recommendation:**

You need to reimplement the function.

### [L-7]: Variable is unused 
**Context:**

1. ```uint256 id = uint256(id_);``` [L298](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L298) 
1. ```address _asset,``` [L528](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L528)

### [L-8]: function bump is useless
**Context:**

[RubiconMarket.bump()](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310)

**Description:**
The function only declares a variable and nothing else. 
```
    function bump(bytes32 id_) external can_buy(uint256(id_)) { 
        uint256 id = uint256(id_); 
        /// TODO: double check it is sound logic to kill this event 
        /// emit LogBump( 
        ///     id_,
        ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
        ///     offers[id].owner,
        ///     offers[id].pay_gem,
        ///     offers[id].buy_gem,
        ///     uint128(offers[id].pay_amt),
        ///     uint128(offers[id].buy_amt),
        ///     offers[id].timestamp
        /// );
    }
```

## Non-Critical Issues ##
### [N-1]: Non-specific or inappropriate name
**Context:**
1. ```function isAuthorized(address src) internal view returns (bool) {``` [L35](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35) (function check if msg.sender == owner, function can be rename to isOwner())
1. ```modifier synchronized() {``` [L264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264) (what is synchronized? function can be rename to isLocked())
1. ```function make(``` [L491](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L491) (function can be rename to makeOffer)
1. ```function _matcho(``` [L1273](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1273) (function can be rename to matchOffer)

### [N-2]: Make function parameters more consistent
**Context:**
1. ```bool rounding``` [L786](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L786)
1. ```bool matching, //match "close enough" orders?``` [L830](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L830)

**Description:**
There is parameter "rounding" in function [offer()](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L780-L799) but in function [offer()](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L824-L852) it is named "matching".

### [N-3]: Input validation can be added
**Context:**
1. ```function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {``` [L1466](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466)
1. ```function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {``` [L1471](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471)

**Recommendation:**

Define the minimum and maximum to prevent setting the wrong value, and validate it in function.

### [N-4]: SafeMath is not needed when using Solidity version >= 0.8.0
**Context:**
1. ```import "@openzeppelin/contracts/utils/math/SafeMath.sol";``` [L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6) 
1. ```import "@openzeppelin/contracts/utils/math/SafeMath.sol";``` [L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6)

**Description:**
The Position and BathBuddy contracts use Solidity version 0.8.17 which already implements overflow checks by default.

### [N-5]: Inconsistency with big numbers
**Context:**
1. ```uint256 fee = mul(spend, feeBPS) / 100_000;``` [L338](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L338) (100_000!)
1. ```uint256 mFee = mul(spend, makerFee()) / 100_000;``` [L346](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L346) (100_000!)
1. ```_amount -= mul(amount, feeBPS) / 100_000;``` [L583](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583) (100_000!)
1. ```_amount -= mul(amount, makerFee()) / 100_000;``` [L586](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586) (100_000!)

and:
1. ```uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);``` [L459](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L459) (10000!)
1. ```uint256 _fee = _minFill.mul(_feeBPS).div(10000);``` [L481](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L481) (10000!)
1. ```_fee = _payAmount.mul(_feeBPS).div(10000);``` [L490](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L490) (10000!)

**Description:**
Some of the big numbers use underscore to divide groups of digits in the number literal so that it is easier to read them. But there are also the big numbers that does not use underscore.

**Recommendation:**
Stick big numbers to one style.

### [N-6]: Function defines a named return variable but then it uses return statements
**Context:**

1. ```function min(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L58](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58) 
1. ```function max(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L62](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62) 
1. ```function imin(int256 x, int256 y) internal pure returns (int256 z) {``` [L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66) 
1. ```function imax(int256 x, int256 y) internal pure returns (int256 z) {``` [L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70) 
1. ```return offers[id].timestamp > 0;``` [L277](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L277) 
1. ```return offers[id].owner;``` [L281](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L281) 
1. ```return offers[id].recipient;``` [L285](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L285) 
1. ```return``` [L497](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L497) 
1. ```return false;``` [L621](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L621) 
1. ```return super.cancel(id); //delete the offer.``` [L882](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L882) 

**Recommendation:**

Choose named return variable or return statement.

### [N-7]: Commented code
**Context:**

1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L100-L116 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L129-L150 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L187-L195 
1. ```/// event OfferDeleted(bytes32 indexed id);``` [L207](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L207) 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L300-L309 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L429-L434 
1. ```/// emit OfferDeleted(bytes32(id));``` [L438](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L438) 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551 
1. ```/// event LogBuyEnabled(bool isEnabled); /// TODO: this event is not used in the contract, remove?``` [L661](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L661) 
1. ```/// event LogMatchingEnabled(bool isEnabled); /// TODO: this event is not used in the contract, remove?``` [L663](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L663) 
1. ```/// event LogInsert(address keeper, uint256 id); /// TODO: this event is not used in the contract, remove?``` [L666](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L666) 
1. https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1346-L1359 

### [N-8]: Use of immutable instead of constant keccak expression
**Context:**

1. ```bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");``` [L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232) 
1. ```// if (matchingEnabled) {``` [L838](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L838) 
1. ```// }``` [L850](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L850) 
1. ```// return super.offer(pay_amt, pay_gem, buy_amt, buy_gem);``` [L851](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L851) 

**Description:**

According to [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/contracts.html#constant-and-immutable-state-variables) for a constant variable, the expression assigned to it is copied to all the places where it is accessed and also re-evaluated each time. It is recommended to use immutable instead. 

### [N-9]: Follow Solidity standard naming conventions
**Context:**

1. ```event emitOffer(``` [L118](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L118) 
1. ```ERC20 pay_gem,``` [L122](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L122)
1. ```ERC20 buy_gem,``` [L123](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L123)
1. ```uint128 pay_amt,``` [L124](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L124) 
1. ```uint128 buy_amt``` [L125](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L125) 
1. ```event emitTake(``` [L152](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L152) 
1. ```ERC20 pay_gem,``` [L157](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L157) 
1. ```ERC20 buy_gem,``` [L158](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L158) 
1. ```uint128 take_amt,``` [L159](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L159) 
1. ```uint128 give_amt``` [L160](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L160) 
1. ```event emitCancel(``` [L174](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L174)
1. ```ERC20 pay_gem,``` [L178](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L178) 
1. ```ERC20 buy_gem,``` [L179](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L179) 
1. ```uint128 pay_amt,``` [L180](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L180) 
1. ```uint128 buy_amt``` [L181](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L181) 
1. ```event emitFee(``` [L198](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L198) 
1. ```event emitDelete(``` [L209](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L209) 
1. ```uint256 public last_offer_id;``` [L219](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L219) 
1. ```uint256 pay_amt;``` [L235](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L235) 
1. ```ERC20 pay_gem;``` [L236](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L236) 
1. ```uint256 buy_amt;``` [L237](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L237) 
1. ```ERC20 buy_gem;``` [L238](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L238) 
1. ```modifier can_buy(uint256 id) virtual {``` [L245](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L245) 
1. ```modifier can_cancel(uint256 id) virtual {``` [L251](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251) 
1. ```modifier can_offer() virtual {``` [L260](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260) 
1. ```ERC20 pay_gem,``` [L492](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L492) 
1. ```ERC20 buy_gem,``` [L493](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L493) 
1. ```uint128 pay_amt,``` [L494](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L494) 
1. ```uint128 buy_amt``` [L495](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L495) 
1. ```uint256 pay_amt,``` [L512](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L512) 
1. ```ERC20 pay_gem,``` [L513](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L513) 
1. ```uint256 buy_amt,``` [L514](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L514) 
1. ```ERC20 buy_gem,``` [L515](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L515) 
1. ```function _next_id() internal returns (uint256) {``` [L568](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568) 
1. ```modifier can_offer() override {``` [L597](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L597) 
1. ```modifier can_buy(uint256 id) override {``` [L603](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L603) 
1. ```modifier can_cancel(uint256 id) virtual override {``` [L610](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L610) 
1. ```event LogMinSell(address pay_gem, uint256 min_amount);``` [L662](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L662) (address pay_gem, uint256 min_amount)
1. ```bool public AqueductDistributionLive;``` [L682](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682) 
1. ```address public AqueductAddress;``` [L684](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L684) 
1. ```modifier can_cancel(uint256 id) override {``` [L719](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L719) 
1. ```ERC20 pay_gem,``` [L734](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L734) 
1. ```ERC20 buy_gem,``` [L735](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L735) 
1. ```uint128 pay_amt,``` [L736](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L736) 
1. ```uint128 buy_amt``` [L737](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L737) 
1. ```uint256 pay_amt, //maker (ask) sell how much``` [L762](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L762) 
1. ```ERC20 pay_gem, //maker (ask) sell which token``` [L763](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L763) 
1. ```uint256 buy_amt, //maker (ask) buy how much``` [L764](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L764) 
1. ```ERC20 buy_gem //maker (ask) buy which token``` [L765](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L765) 
1. ```uint256 pay_amt, //maker (ask) sell how much``` [L781](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L781) 
1. ```ERC20 pay_gem, //maker (ask) sell which token``` [L782](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L782) 
1. ```uint256 buy_amt, //maker (ask) buy how much``` [L783](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L783) 
1. ```ERC20 buy_gem, //maker (ask) buy which token``` [L784](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L784) 
1. ```uint256 pay_amt, //maker (ask) sell how much``` [L803](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L803) 
1. ```ERC20 pay_gem, //maker (ask) sell which token``` [L804](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L804) 
1. ```uint256 buy_amt, //maker (ask) buy how much``` [L805](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L805) 
1. ```ERC20 buy_gem, //maker (ask) buy which token``` [L806](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L806) 
1. ```uint256 pay_amt, //maker (ask) sell how much``` [L825](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L825) 
1. ```ERC20 pay_gem, //maker (ask) sell which token``` [L826](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L826) 
1. ```uint256 buy_amt, //maker (ask) buy how much``` [L827](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L827) 
1. ```ERC20 buy_gem, //maker (ask) buy which token``` [L828](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L828) 
1. ```function del_rank(uint256 id) external returns (bool) {``` [L937](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L937) 
1. ```ERC20 pay_gem, //token to assign minimum sell amount to``` [L956](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L956) 
1. ```ERC20 pay_gem //token for which minimum sell amount is queried``` [L966](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L966) 
1. ```ERC20 sell_gem,``` [L975](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L975) 
1. ```ERC20 buy_gem``` [L976](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L976) 
1. ```ERC20 sell_gem,``` [L999](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L999) 
1. ```ERC20 buy_gem``` [L1000](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1000) 
1. ```ERC20 pay_gem,``` [L1029](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1029) 
1. ```uint256 pay_amt,``` [L1030](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1030) 
1. ```ERC20 buy_gem,``` [L1031](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1031) 
1. ```uint256 min_fill_amount``` [L1032](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1032) 
1. ```) external returns (uint256 fill_amt) {``` [L1033](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1033) 
1. ```ERC20 buy_gem,``` [L1070](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1070) 
1. ```uint256 buy_amt,``` [L1071](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1071) 
1. ```ERC20 pay_gem,``` [L1072](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1072) 
1. ```uint256 max_fill_amount``` [L1073](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1073) 
1. ```) external returns (uint256 fill_amt) {``` [L1074](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1074) 
1. ```ERC20 buy_gem,``` [L1116](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1116) 
1. ```ERC20 pay_gem,``` [L1117](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1117) 
1. ```uint256 pay_amt``` [L1118](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1118) 
1. ```) external view returns (uint256 fill_amt) {``` [L1119](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1119) 
1. ```ERC20 buy_gem,``` [L1125](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1125) 
1. ```ERC20 pay_gem,``` [L1126](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1126) 
1. ```uint256 pay_amt``` [L1127](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1127) 
1. ```) public view returns (uint256 fill_amt) {``` [L1128](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1128) 
1. ```ERC20 pay_gem,``` [L1150](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1150) 
1. ```ERC20 buy_gem,``` [L1151](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1151) 
1. ```uint256 buy_amt``` [L1152](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1152) 
1. ```) public view returns (uint256 fill_amt) {``` [L1153](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1153) 
1. ```ERC20 pay_gem,``` [L1158](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1158) 
1. ```ERC20 buy_gem,``` [L1159](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1159) 
1. ```uint256 buy_amt``` [L1160](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1160) 
1. ```) public view returns (uint256 fill_amt) {``` [L1161](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1161) 
1. ```address buy_gem = address(offers[id].buy_gem);``` [L1211](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1211) 
1. ```address pay_gem = address(offers[id].pay_gem);``` [L1212](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1212) 
1. ```uint256 old_top = 0;``` [L1214](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1214) 
1. ```uint256 old_pos;``` [L1240](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1240) 
1. ```uint256 t_pay_amt, //taker sell how much``` [L1274](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1274) 
1. ```ERC20 t_pay_gem, //taker sell which token``` [L1275](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1275) 
1. ```uint256 t_buy_amt, //taker buy how much``` [L1276](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1276) 
1. ```ERC20 t_buy_gem, //taker buy which token``` [L1277](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1277) 
1. ```uint256 best_maker_id; //highest maker id``` [L1283](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1283) 
1. ```uint256 t_buy_amt_old; //taker buy how much saved``` [L1284](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1284) 
1. ```uint256 m_buy_amt; //maker offer wants to buy this much token``` [L1285](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1285) 
1. ```uint256 m_pay_amt; //maker offer wants to sell this much token``` [L1286](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1286) 
1. ```ERC20 buy_gem = offers[id].buy_gem;``` [L1368](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1368) 
1. ```ERC20 pay_gem = offers[id].pay_gem;``` [L1369](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1369) 
1. ```uint256 prev_id; //maker (ask) id``` [L1370](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1370) 
1. ```address buy_gem = address(offers[id].buy_gem);``` [L1408](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1408) 
1. ```address pay_gem = address(offers[id].pay_gem);``` [L1409](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1409) 

**Description:**

The above codes don't follow [Solidity's standard naming convention](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#naming-conventions).  
- Structs should be named using the CapWords style. Examples: MyCoin, Position, PositionXY.
- Events should be named using the CapWords style. Examples: Deposit, Transfer, Approval, BeforeTransfer, AfterTransfer.
- Functions should use mixedCase. Examples: getBalance, transfer, verifyOwner, addMember, changeOwner.
- Function arguments should use mixedCase. Examples: initialSupply, account, recipientAddress, senderAddress, newOwner.
- Local and State Variable should use mixedCase. Examples: totalSupply, remainingSupply, balancesOf, creatorAddress, isPreSale, tokenExchangeRate.
- Constants should be named with all capital letters with underscores separating words. Examples: MAX_BLOCKS, TOKEN_NAME, TOKEN_TICKER, CONTRACT_VERSION.
- Modifier should use mixedCase. Examples: onlyBy, onlyAfter, onlyDuringThePreSale.
- Enums, in the style of simple type declarations, should be named using the CapWords style. Examples: TokenGroup, Frame, HashStyle, CharacterLocation.

### [N-10]: Wrong order of functions
**Context:**

1. ```modifier auth() {``` [L30](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30) (modifier definition can not go after external function)
1. ```uint256 constant WAD = 10 ** 18;``` [L74](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74) (state variable declaration can not go after internal function)
1. ```function whoIsBuddy(``` [L51](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L51) (external function can not go after public function)
1. ```modifier onlyOwner() {``` [L87](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L87) (modifier definition can not go after external function)

**Description:**

According to [official solidity documentation](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions) functions should be grouped according to their visibility and ordered:

+ constructor

+ receive function (if exists)

+ fallback function (if exists)

+ external

+ public

+ internal

+ private

Within a grouping, place the view and pure functions last.

**Recommendation:**

Put the functions in the correct order according to the documentation.

### [N-11]: NatSpec comments should be increased in contracts
**Context:**

All contracts.

**Description:**

https://docs.soliditylang.org/en/v0.8.19/natspec-format.html

**Recommendation:**

Increase NatSpec comments in all contracts.


### [N-12]: Constants сan be used
**Context:**

1. ```uint256 fee = mul(spend, feeBPS) / 100_000;``` [L338](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L338)
1. ```uint256 mFee = mul(spend, makerFee()) / 100_000;``` [L346](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L346) 
1. ```_amount -= mul(amount, feeBPS) / 100_000;``` [L583](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583)
1. ```_amount -= mul(amount, makerFee()) / 100_000;``` [L586](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586)
1. ```mul(pay_amt, 10 ** 9),``` [L1057](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1057)
1. ```) / 10 ** 9;``` [L1059](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1059)
1. ```mul(buy_amt, 10 ** 9),``` [L1099](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1099)
1. ```) / 10 ** 9``` [L1101](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1101) 
1. ```mul(pay_amt, 10 ** 9),``` [L1142](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1142)
1. ```) / 10 ** 9``` [L1144](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1144)
1. ```mul(buy_amt, 10 ** 9),``` [L1175](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1175)
1. ```) / 10 ** 9``` [L1177](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1177)
1. ```uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);``` [L459](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L459)
1. ```uint256 _fee = _minFill.mul(_feeBPS).div(10000);``` [L481](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L481) 
1. ```_fee = _payAmount.mul(_feeBPS).div(10000);``` [L490](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L490)


**Recommendation:**
Define constant variables for repeated values.


### [N-13]: Change imports
**Context:**

1. ```import "@openzeppelin/contracts/token/ERC20/ERC20.sol";``` [L11](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L11) 
1. ```import "@openzeppelin/contracts/utils/StorageSlot.sol";``` [L12](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L12) 
1. ```import "@openzeppelin/contracts/token/ERC20/ERC20.sol";``` [L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L4) 
1. ```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";``` [L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L4) 
1. ```import "@openzeppelin/contracts/token/ERC20/IERC20.sol";``` [L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L5) 
1. ```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";``` [L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L4) 
1. ```import "@openzeppelin/contracts/security/ReentrancyGuard.sol";``` [L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L5) 
1. ```import "@openzeppelin/contracts/utils/math/SafeMath.sol";``` [L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6) 
1. ```import "@openzeppelin/contracts/security/Pausable.sol";``` [L7](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L7) 
1. ```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";``` [L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L4) 
1. ```import "@openzeppelin/contracts/token/ERC20/ERC20.sol";``` [L5](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L5) 
1. ```import "@openzeppelin/contracts/utils/math/SafeMath.sol";``` [L6](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6) 
1. ```import "@openzeppelin/contracts/access/Ownable.sol";``` [L7](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L7) 
1. ```import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";``` [L4](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L4) 

**Description:**

Example:
Instead of ```import "@openzeppelin/contracts/access/Ownable.sol";```

You can do your imports like this:
```import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";```.

### [N-14]: Missing leading underscores
**Context:**

1. ```function isAuthorized(address src) internal view returns (bool) {``` [L35](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35)
1. ```function add(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L46](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L46)
1. ```function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L50](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L50)
1. ```function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L54](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L54)
1. ```function min(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L58](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58)
1. ```function max(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L62](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L62)
1. ```function imin(int256 x, int256 y) internal pure returns (int256 z) {``` [L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L66)
1. ```function imax(int256 x, int256 y) internal pure returns (int256 z) {``` [L70](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L70)
1. ```function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L77](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L77) 
1. ```function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L81](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L81) 
1. ```function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L85](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L85)
1. ```function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {``` [L89](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L89)
1. ```bool locked;``` [L224](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224)
1. ```uint256 internal feeBPS;``` [L227](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L227) 
1. ```address internal feeTo;``` [L230](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L230) 
1. ```bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");``` [L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232)
1. ```bool private initialized;``` [L18](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L18) 
1. ```mapping(address => address) private tokenToBathToken;``` [L20](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L20) 
1. ```mapping(address => address) private bathTokenToBuddy;``` [L21](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L21) 
1. ```function openPosition(``` [L125](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L125) 

**Description:**

Internal and private functions, state variables, constants, and immutables should starting with an underscore.


### [N-15]: Unnecessary leading underscores
**Context:**

1. ```mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids``` [L691](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691) (_rank should not start with _)
1. ```mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair``` [L692](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L692) (_best should not start with _)
1. ```mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook``` [L693](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L693) (_span should not start with _)
1. ```mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers``` [L694](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L694) (_dust should not start with _)
1. ```mapping(uint256 => uint256) public _near; //next unsorted offer id``` [L695](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L695) (_near should not start with _)
1. ```uint256 public _head; //first unsorted offer id``` [L696](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L696) (_head should not start with _)

**Description:**

Internal and private (NOT PUBLIC) functions, state variables, constants, and immutables should starting with an underscore.

### [N-16]: Mark visibility of variables
**Context:**

1. ```uint256 constant WAD = 10 ** 18;``` [L74](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74) 
1. ```uint256 constant RAY = 10 ** 27;``` [L75](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L75) 
1. ```bool locked;``` [L224](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224) 

### [N-17]: Emit events in initialize() functions
**Context:**

1. ```function initialize(address _feeTo) public {``` [L700](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700)
1. ```function initialize(address _comptroller, address _pAdmin) external {``` [L32](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32) 


### [N-18]: Line is too long
**Context:**

1. ```require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6``` [L522](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L522) 
1. ```require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6``` [L524](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L524) 

**Description:**

Maximum suggested line length is [120 characters](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length).
