# Gas Optomazation

## Summary

|        | Issue  | Instances 
| ---    | ----   | ----      
|[G‑01]  | Use double if statements instead of && | 4
|[G‑02]  | Internal functions only called once can be inlined to save gas | 20
|[G‑03]  | Call block.timestamp direclty instead of function | 1
|[G-04]  | ++variable costs less gas than variable++, especially when it’s used in for-loops (—variable/variable— too) | 3
|[G-05]  | public functions to external | 5
|[G-06]  | Expressions for constant values such as a call to keccak256(), should use immutable rather than constant | 1
|[G-07]  | Use of Multiple require(s) | 5
|[G-08]  | Amounts should be checked for 0 before calling a transfer | 3
|[G-09]  | Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate | 12
|[G-10] | Functions guaranteed to revert when called by normal users can be marked payable | 7
|[G-11] | Do not calculate constants | 2
|[G-12] | <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES | 1
|[G-13] | Gas saving is achieved by removing the delete keyword | 4
|[G-14] | Avoid using state variable in emit | 2
|[G-15] | Change public state variable visibility to private | 4
|[G-16] | Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead | 3
|[G-17] | Use a more recent version of solidity | 2
|[G-18] | Save gas with the use of the import statement | 6
|[G-19] | Setting the constructor to payable | 2


### [G‑01] Use double if statements instead of &&
If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.



```solidity
File: contracts/RubiconMarket.sol

349    if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

1324   if (t_buy_amt > 0 && t_pay_amt > 0 && t_pay_amt >= _dust[address(t_pay_gem)]) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol


```solidity]
File: /contracts/utilities/poolsUtility/Position.sol

176     if (i.add(1) == vars.limit && vars.lastBorrow != 0) {

391     if (IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 && borrowBalance(_bathTokenAsset) == 0 ) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol 

### [G‑02] Internal functions only called once can be inlined to save gas

```solidity
File: contracts/BathHouseV2.sol

137    function _bathify(address _underlying) internal view returns (string memory _name, string memory _symbol, uint8 _decimals) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#137

```solidity
File: /contracts/utilities/FeeWrapper.sol

60      function _rubicall(CallParams memory _params) internal returns (bytes memory) {

76      function _rubicallPayable(CallParams memory _params) internal returns (bytes memory) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

251      function _borrowLoop( address _asset, address _quote, address _bathTokenAsset, address _bathTokenQuote, uint256 _amount, uint256 _toBorrow ) internal returns (uint256 _bathTokenAmount) {

272     function _borrow(address _cToken, uint256 _amount) internal {

280     function _repay(address _asset, address _quote, uint256 _posId) internal {

322     function _calculateDebt( address _bathToken, uint256 _startBlock, uint256 _borrowedAmount ) internal view returns (uint256 _debt) {

336     function _enterMarkets(address _bathToken) internal {

343      function _supply( address _token, address _bathToken, uint256 _amount ) internal returns (uint256 _bathTokenAmount) {

382     function _redeem(address _asset, uint256 _bathTokenAmount) internal {

401     function _savePosition( address _asset, address _quote, uint256 _borrowedAmount, uint256 _currentBathTokenAmount ) internal {

421     function _removePosition(uint256 _positionId) internal {

426     function _updateMargin( uint256 _positionId, uint256 _bathTokenAmount, uint256 _marginAmount ) internal {

454      function _marketBuy( address _asset, address _quote, uint256 _maxFill ) internal {

475     function _marketSell( address _asset, address _quote, uint256 _minFill ) internal {

526      function _borrowLimit( address _bathToken, address _asset, uint256 _assetAmount, uint256 _leverage ) internal returns (uint256 _limit, uint256 _lastBorrow) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: contracts/RubiconMarket.sol

568     function _next_id() internal returns (uint256) {

1225    function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1273    function _matcho(
        uint256 t_pay_amt, //taker sell how much
        ERC20 t_pay_gem, //taker sell which token
        uint256 t_buy_amt, //taker buy how much
        ERC20 t_buy_gem, //taker buy which token
        uint256 pos, //position id
        bool rounding, //match "close enough" orders?
        address owner,
        address recipient
    ) internal returns (uint256 id) {

1362    function _sort(
        uint256 id, //maker (ask) id
        uint256 pos //position to insert into
    ) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

### [G-03] Call block.timestamp direclty instead of function

```solidity
File: contracts/RubiconMarket.sol

624     function getTime() public view returns (uint64) {
        return uint64(block.timestamp);
    }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624

## [G-04] ++variable costs less gas than variable++, especially when it’s used in for-loops (—variable/variable— too)

```solidity
File: contracts/RubiconMarket.sol

899     for (uint i = 0; i < payAmts.length; i++) {

911     for (uint i = 0; i < ids.length; i++) {

924     for (uint i = 0; i < ids.length; i++) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

### [G-05] public functions to external
 External call cost is less expensive than of public functions.
 Contracts are allowed to override their parents’ functions and change the visibility from external to public.
```solidity
File: /contracts/periphery/BathBuddy.sol

142     function earned( address account, address token ) public view override returns (uint256) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L142

```solidity
File: contracts/RubiconMarket.sol

733     function make( ERC20 pay_gem, ERC20 buy_gem, uint128 pay_amt, uint128 buy_amt ) public override returns (bytes32) {

752     function take(bytes32 id, uint128 maxTakeAmount) public override {

855      function buy( uint256 id, uint256 amount ) public override can_buy(id) returns (bool) {

871     function cancel( uint256 id ) public override can_cancel(id) returns (bool success) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol


## [G-06] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

```solidity
File: contracts/RubiconMarket

232     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L32

## [G-07] Use of Multiple require(s) 
 Using mutiple require statements is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

```solidity
File: contracts/RubiconMarket

893     require( payAmts.length == payGems.length && payAmts.length == buyAmts.length && payAmts.length == buyGems.length, "Array lengths do not match" );

940     require( !isActive(id) && _rank[id].delb != 0 && _rank[id].delb < block.number - 10 );

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

591     require( _leverage > _wad && _leverage <= _leverageMax, "_leverageCheck{Long}: INVLAID LEVERAGE" )

595     require( _leverage >= _wad && _leverage <= _leverageMax, "_leverageCheck{Short}: INVLAID LEVERAGE" );
   

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

95   require( msg.sender == myBathTokenBuddy && msg.sender != address(0) && friendshipStarted, "You are not my buddy!" );

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95

## [G-08] Amounts should be checked for 0 before calling a transfer
 Checking non-zero transfer values can avoid an expensive external call and save gas.
While this is done at some places, it's not consistently done in the solution.

```solidity
File: /contracts/utilities/FeeWrapper.sol

102     IERC20(_feeToken).transfer(_feeTo, _feeAmount);

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102

```solidity
File: contracts/RubiconMarket.sol

460     require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt)) 

461     require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-09] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate

```solidity
File: /contracts/periphery/BathBuddy.sol

   51 mapping(address => uint256) public periodFinish; // Token specific

   52 mapping(address => uint256) public rewardRates; // Token specific reward rates

   53 mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

   54 mapping(address => uint256) public lastUpdateTime; //Token specific

   55 mapping(address => uint256) public rewardsPerTokensStored; // Token specific

   59 mapping(address => mapping(address => uint256))

   61 mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: contracts/RubcionMarket.sol

 691  mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

 692  mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

 693  mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted 
 orderbook

 694  mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers

 695  mapping(uint256 => uint256) public _near; //next unsorted offer id
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-10] Functions guaranteed to revert when called by normal users can be marked payable
 If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.
```solidity
 File: /contracts/periphery/BathBuddy.sol

 191    function notifyRewardAmount( uint256 reward, IERC20 rewardsToken ) external onlyOwner updateReward(address(0), address(rewardsToken)) {

 232    function setRewardsDuration( uint256 _rewardsDuration, address token ) external onlyOwner {
``` 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

93      function buyAllAmountWithLeverage( address quote, address asset, uint256 quotePayAmount, uint256 leverage ) external onlyOwner { 

107     function sellAllAmountWithLeverage( address asset, address quote, uint256 assetPayAmount, uint256 leverage ) external onlyOwner {

210     function closePosition(uint256 posId) external onlyOwner {

226     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242     function withdraw(address token, uint256 amount) external onlyOwner {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-11] Do not calculate constants
Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas

```solidity
File: contracts/RubiconMarket.sol

74     uint256 constant WAD = 10 ** 18;

75     uint256 constant RAY = 10 ** 27;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

### [G-12] <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES
```solidity
file: /contracts/utilities/poolsUtility/Position.sol
431      positions[_positionId].bathTokenAmount += _bathTokenAmount;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431

## [G-13] Gas saving is achieved by removing the delete keyword 

```solidity
file: /contracts/utilities/poolsUtility/Position.sol

422     elete positions[_positionId];

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L422

```solidity
file: contracts/RubiconMarket.sol

437     delete offers[id];

456     delete offers[id];

945     delete _rank[id];
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-14] Avoid using state variable in emit

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

418     emit PositionOpened(lastPositionId, pos);
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L418

```solidity
File: contracts/RubiconMarket.sol
27     emit LogSetOwner(owner);
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L27

## [G-15] Change public state variable visibility to private
If it is preferred to change the visibility of the owner
```solidity
File: /contracts/periphery/BathBuddy.sol

46     address public owner;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L46

```solidity
File: contracts/BathHouseV2.sol

 15     address public admin; 
 17     address public proxyAdmin;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: contracts/RubiconMarket.sol
23     address public owner;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L23

## [G-16] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
When using elements that are smaller than 32 bytes, your contractâ€™s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.

```solidity
File: /contracts/utilities/FeeWrapper.sol

23      address feeTo; // receiver of the fee
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L23

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

39     bool isActive; // false by default, when active - true
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L39

```solidity
File: contracts/RubiconMarket.sol

237     uint256 buy_amt;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L237

## [G-17] Use a more recent version of solidity

```solidity
File: /contracts/periphery/BathBuddy.sol

2      pragma solidity ^0.8.0;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

```solidity
File: contracts/RubiconMarket.sol

2      pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

## [G-18] Save gas with the use of the import statement
 With the import statement, it saves gas to specifically import only the parts of the contracts, not the complete ones.

```solidity
File: /contracts/periphery/BathBuddy.sol

4      import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L4

```solidity
File: contracts/BathHouseV2.sol

4      import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L4

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

4      import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5      import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: contracts/RubiconMarket.sol
11     import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L11

```solidity
File: contracts/V2Migrator.sol

5      import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L5

## [G-19] Setting the constructor to payable
You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of msg.value == 0 and saves 13 gas on deployment with no security risks.

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

54     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54

```solidity
File: contracts/V2Migrator.sol

30     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30