## QA REPORT

| |Issue|
|-|:-|
| [01] | `RubiconMarket.setFeeBPS` FUNCTION DOES NOT HAVE A LIMIT FOR SETTING `feeBPS` |
| [02] | VULNERABILITY IN VERSION 4.8.0 OF `@openzeppelin/contracts` |
| [03] | MISSING LENGTH CHECKS FOR ARRAY INPUTS |
| [04] | MISSING `address(0)` CHECKS FOR CRITICAL ADDRESS INPUTS |
| [05] | `synchronized` MODIFIER CAN BE PLACED AND EXECUTED BEFORE OTHER MODIFIERS IN FUNCTIONS |
| [06] | THERE IS NO NEED TO CHECK `msg.sender != address(0)` IN `onlyBuddy` MODIFIER |
| [07] | UNUSED IMPORTS CAN BE REMOVED |
| [08] | CONSTANTS CAN BE USED INSTEAD OF MAGIC NUMBERS |
| [09] | FLOATING PRAGMAS |
| [10] | INCOMPLETE NATSPEC COMMENTS |
| [11] | MISSING NATSPEC COMMENTS |

## [01] `RubiconMarket.setFeeBPS` FUNCTION DOES NOT HAVE A LIMIT FOR SETTING `feeBPS`
The following `RubiconMarket.setFeeBPS` function can be called to set `feeBPS` without a limit, which can have unexpected consequences. For example, if `feeBPS` is set to be more than 100_000, `fee` can even exceed `spend` when calling the `RubiconMarket.buy` function below. As a result, users who are unaware of such `feeBPS` change can pay huge `fee` unexpectedly.

As a mitigation, the `RubiconMarket.setFeeBPS` function can be updated to only set `feeBPS` to a value that cannot exceed a certain reasonable limit, which should be less than 100_000.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1469
```solidity
    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314-L448
```solidity
    function buy(
        uint256 id,
        uint256 quantity
    ) public virtual can_buy(id) synchronized returns (bool) {
        OfferInfo memory _offer = offers[id];
        uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

        require(uint128(spend) == spend, "spend is not an int");
        require(uint128(quantity) == quantity, "quantity is not an int");

        ///@dev For backwards semantic compatibility.
        if (
            quantity == 0 ||
            spend == 0 ||
            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt
        ) {
            return false;
        }

        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
        offers[id].buy_amt = sub(_offer.buy_amt, spend);

        /// @dev Fee logic added on taker trades
        uint256 fee = mul(spend, feeBPS) / 100_000;
        require(
            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
            "Insufficient funds to cover fee"
        );
        ...
    }
```

## [02] VULNERABILITY IN VERSION 4.8.0 OF `@openzeppelin/contracts`
As shown in the following code in `package.json`, version 4.8.0 of `@openzeppelin/contracts` can be used. As described in https://security.snyk.io/package/npm/@openzeppelin%2Fcontracts/4.8.0, this version is vulnerable to incorrect calculation when using the ERC721Consecutive contract. To reduce the potential attack surface and be more future-proofed, please consider upgrading this package to at least version 4.8.2.

https://github.com/code-423n4/2023-04-rubicon/blob/main/package.json#L47
```solidity
    "@openzeppelin/contracts": "^4.8.0",
```

## [03] MISSING LENGTH CHECKS FOR ARRAY INPUTS
The items in the `buddies` array input should correspond to the items in the `rewardsTokens` array input when calling the following `BathHouseV2.claimRewards` function. To prevent input errors, please consider checking that the lengths of the `buddies` and `rewardsTokens` array inputs are the same. If not, calling this function should revert.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115-L128
```solidity
    function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);
        // get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }
```

## [04] MISSING `address(0)` CHECKS FOR CRITICAL ADDRESS INPUTS
To prevent unintended behaviors, the critical address inputs should be checked against `address(0)`. The `address(0)` checks are missing for the address input variables in the following constructors. Please consider checking them.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32-L39
```solidity
    function initialize(address _comptroller, address _pAdmin) external {
        require(!initialized, "BathHouseV2 already initialized!");
        comptroller = Comptroller(_comptroller);
        admin = msg.sender;
        proxyAdmin = _pAdmin;

        initialized = true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L562
```solidity
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
        require(pay_amt > 0);
        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
        require(buy_amt > 0);
        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
        require(pay_gem != buy_gem);

        OfferInfo memory info;
        info.pay_amt = pay_amt;
        info.pay_gem = pay_gem;
        info.buy_amt = buy_amt;
        info.buy_gem = buy_gem;
        info.recipient = recipient;
        info.owner = owner;
        info.timestamp = uint64(block.timestamp);
        id = _next_id();
        offers[id] = info;
        ...
    }
```

## [05] `synchronized` MODIFIER CAN BE PLACED AND EXECUTED BEFORE OTHER MODIFIERS IN FUNCTIONS
The `synchronized` modifier is similar to OpenZeppelin's `nonReentrant` modifier. As a best practice, this `synchronized` modifier can be placed and executed before other modifiers in functions to prevent reentrancies through other modifiers and make code more efficient. To follow the best practice, please consider placing the `synchronized` modifier before other modifiers in the following functions.

```solidity
contracts\RubiconMarket.sol
  317: ) public virtual can_buy(id) synchronized returns (bool) {  
  454: ) public virtual can_cancel(id) synchronized returns (bool success) {  
  518: ) public virtual can_offer synchronized returns (uint256 id) {  
```

## [06] THERE IS NO NEED TO CHECK `msg.sender != address(0)` IN `onlyBuddy` MODIFIER
Since `address(0)` can never be `msg.sender`, it is unnecessary to check `msg.sender != address(0)` in the following `onlyBuddy` modifier. To make the code more efficient, please consider removing this check in the following `onlyBuddy` modifier.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94-L102
```solidity
    modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }

```

## [07] UNUSED IMPORTS CAN BE REMOVED
The following imports are not used. For a higher efficiency, please consider removing them.

```solidity
contracts\BathHouseV2.sol
  8: import "./compound-v2-fork/Unitroller.sol";

contracts\V2Migrator.sol
  4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

contracts\utilities\FeeWrapper.sol
  5: import "./RubiconRouter.sol";
```

## [08] CONSTANTS CAN BE USED INSTEAD OF MAGIC NUMBERS
To improve readability and maintainability, a constant can be used instead of the magic number. Please consider replacing the magic numbers, such as `100_000`, used in the following code with constants.

```solidity
contracts\RubiconMarket.sol
  338: uint256 fee = mul(spend, feeBPS) / 100_000; 
  346: uint256 mFee = mul(spend, makerFee()) / 100_000; 
  583: _amount -= mul(amount, feeBPS) / 100_000;   
  586: _amount -= mul(amount, makerFee()) / 100_000;   
  711: feeBPS = 1; 

contracts\periphery\BathBuddy.sol
  132: .mul(1e18)  
  153: .div(1e18)  
```

## [09] FLOATING PRAGMAS
It is a best practice to lock pragmas instead of using floating pragmas to ensure that contracts are tested and deployed with the intended compiler version. Accidentally deploying contracts with different compiler versions can lead to unexpected risks and undiscovered bugs. Please consider locking pragmas for the following files.

```solidity
contracts\RubiconMarket.sol
  2: pragma solidity ^0.8.9;

contracts\periphery\BathBuddy.sol
  2: pragma solidity ^0.8.0;
```

## [10] INCOMPLETE NATSPEC COMMENTS
NatSpec comments provide rich code documentation. The following functions miss the `@param` or `@return` comments. Please consider completing the NatSpec comments for these functions.

```solidity
contracts\BathHouseV2.sol
  45: function getBathTokenFromAsset( 
  60: function createBathToken(   
  115: function claimRewards(  
  131: function getReward(address buddy, address rewardsToken) external {  

contracts\RubiconMarket.sol
  314: function buy(   
  452: function cancel(    
  511: function offer( 
  700: function initialize(address _feeTo) public {    

contracts\periphery\BathBuddy.sol
  168: function getReward( 

contracts\utilities\FeeWrapper.sol
  28: function calculateFee(  
  48: function rubicall(  
  60: function _rubicall(  
  76: function _rubicallPayable(  
  93: function _chargeFee(FeeParams memory _feeParams, address _target) internal {  
  108: function _chargeFeePayable(  
```

## [11] MISSING NATSPEC COMMENTS
NatSpec comments provide rich code documentation. The following functions miss NatSpec comments. Please consider adding NatSpec comments for these functions.

```solidity
contracts\BathHouseV2.sol
  32: function initialize(address _comptroller, address _pAdmin) external {   
  51: function whoIsBuddy(   
  137: function _bathify(  

contracts\RubiconMarket.sol
  25: function setOwner(address owner_) external auth {   
  35: function isAuthorized(address src) internal view returns (bool) {   
  272: function makerFee() public view returns (uint256) {   
  276: function isActive(uint256 id) public view returns (bool active) {
  280: function getOwner(uint256 id) public view returns (address owner) {
  284: function getRecipient(uint256 id) public view returns (address owner) {
  288: function getOffer(   
  297: function bump(bytes32 id_) external can_buy(uint256(id_)) {   
  487: function kill(bytes32 id) external virtual {    
  491: function make(  
  568: function _next_id() internal returns (uint256) {    
  574: function getFeeBPS() public view returns (uint256) {    
  578: function calcAmountAfterFee(    
  620: function isClosed() public pure returns (bool closed) { 
  624: function getTime() public view returns (uint64) { 
  628: function stop() external auth { 
  733: function make(  
  752: function take(bytes32 id, uint128 maxTakeAmount) public override { 
  756: function kill(bytes32 id) external override { 
  761: function offer( 
  780: function offer( 
  802: function offer( 
  824: function offer( 
  855: function buy(   
  1183: function _buys(uint256 id, uint256 amount) internal returns (bool) {   
  1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {   
  1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {   
  1476: function setFeeTo(address newFeeTo) external auth returns (bool) {   
  1482: function getFeeTo() external view returns (address) {   

contracts\V2Migrator.sol
  38: function migrate(IBathToken bathTokenV1) external { 

contracts\periphery\BathBuddy.sol
  71: function spawnBuddy(    
  112: function lastTimeRewardApplicable(  
  121: function rewardPerToken(address token) public view returns (uint256) {  
  139: function earned(    
  157: function getRewardForDuration(    
  232: function setRewardsDuration(
```