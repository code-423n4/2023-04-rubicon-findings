| no | issue | Instance |
|----|-------|----------|
|[G-01]| instead of function ,call block.timestamp directly | 1
|[G-02]| public functions to external |5
|[G-03]| ++variable costs less gas than variable++, especially when it’s used in for-loops (—variable/variable— too)|3
|[G‑04]| Use double if statements instead of &&|2
|[G-05]| Do not calculate constants|2
|[G-06]| += const more gas then = + for state variable|1
|[G-07]| Gas saving is achieved by removing the delete keyword|3
|[G-08]| Avoid using state variable in emit| 1
|[G-09]| Change public state variable visibility to private|2
|[G-10]| Use a more recent version of solidity|2
|[G-11]| Save gas with the use of the import statement | 3
|[G-12]|Setting the constructor to payable | 2
|[G-13]| Amounts should be checked for 0 before calling a transfer | 3
|[G-14]|Use of Multiple require instead of && | 4
|[G‑15]| Internal functions only called once can be inlined to save gas | 13

### [G-01] instead of function ,call block.timestamp directly

```solidity
624     function getTime() public view returns (uint64) {
        return uint64(block.timestamp);
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624


### [G-02] public functions to external
 External call cost is less expensive than of public functions.
 Contracts are allowed to override their parents’ functions and change the visibility from external to public.
 
```solidity
733     function make( ERC20 pay_gem, ERC20 buy_gem, uint128 pay_amt, uint128 buy_amt ) public override returns (bytes32) {
 ```


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733

```solidity
752     function take(bytes32 id, uint128 maxTakeAmount) public override {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L752
```solidity
855      function buy( uint256 id, uint256 amount ) public override can_buy(id) returns (bool) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L855
```solidity

871     function cancel( uint256 id ) public override can_cancel(id) returns (bool success) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871

### [G-03] ++variable costs less gas than variable++, especially when it’s used in for-loops (—variable/variable— too)

```solidity
899     for (uint i = 0; i < payAmts.length; i++) {
 ```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899

```solidity
911     for (uint i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911

```solidity
924     for (uint i = 0; i < ids.length; i++) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924



### [G‑04] Use double if statements instead of &&

```solidity
176     if (i.add(1) == vars.limit && vars.lastBorrow != 0) {
```    
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L176
```solidity
391     if (IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 && borrowBalance(_bathTokenAsset) == 0 ) {
```    
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#391

### [G-05] Do not calculate constants
```solidity
74     uint256 constant WAD = 10 ** 18;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74

```solidity
75     uint256 constant RAY = 10 ** 27;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L75

### [G-06] <X> += <Y> const more gas then <X> = <X> + <Y> for state variable 
```solidity
431      positions[_positionId].bathTokenAmount += _bathTokenAmount;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431

### [G-07] Gas saving is achieved by removing the delete keyword 

```solidity
422     delete positions[_positionId];
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L422

```solidity
437     delete offers[id];
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L437




```solidity
945     delete _rank[id];
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L945


### [G-08] Avoid using state variable in emit

```solidity
418     emit PositionOpened(lastPositionId, pos);
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L418



### [G-09] Change public state variable visibility to private

```solidity
46     address public owner;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L46

```solidity
23     address public owner;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L23

### [G-10] Use a more recent version of solidity

```solidity
2      pragma solidity ^0.8.0;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

```solidity
2      pragma solidity ^0.8.9;
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2


## [G-11] Save gas with the use of the import statement
 With the import statement, it saves gas to specifically import only the parts of the contracts, not the complete ones.

```solidity
4      import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L4

```solidity
5      import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L5

## [G-12] Setting the constructor to payable

```solidity
54     constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54

```solidity
30     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30

## [G-13] Amounts should be checked for 0 before calling a transfer
```solidity
102     IERC20(_feeToken).transfer(_feeTo, _feeAmount);

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102

```solidity
460     require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt)) 
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L460

```solidity
461     require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L461

### [G-14] Use of Multiple require instead of &&

```solidity
893     require( payAmts.length == payGems.length && payAmts.length == buyAmts.length && payAmts.length == buyGems.length, "Array lengths do not match" );
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893

```solidity
591     require( _leverage > _wad && _leverage <= _leverageMax, "_leverageCheck{Long}: INVLAID LEVERAGE" ) 
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L591


```solidity
595     require( _leverage >= _wad && _leverage <= _leverageMax, "_leverageCheck{Short}: INVLAID LEVERAGE" );   
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#595



```solidity
95   require( msg.sender == myBathTokenBuddy && msg.sender != address(0) && friendshipStarted, "You are not my buddy!" );

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95

### [G‑15] Internal functions only called once can be inlined to save gas

```solidity
137    function _bathify(address _underlying) internal view returns (string memory _name, string memory _symbol, uint8 _decimals) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#137

```solidity
60      function _rubicall(CallParams memory _params) internal returns (bytes memory) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L60

```solidity
76      function _rubicallPayable(CallParams memory _params) internal returns (bytes memory) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76

```solidity
251      function _borrowLoop( address _asset, address _quote, address _bathTokenAsset, address _bathTokenQuote, uint256 _amount, uint256 _toBorrow ) internal returns (uint256 _bathTokenAmount) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L251


```solidity
280     function _repay(address _asset, address _quote, uint256 _posId) internal {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L280

```solidity
336     function _enterMarkets(address _bathToken) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L336

```solidity
343      function _supply( address _token, address _bathToken, uint256 _amount ) internal returns (uint256 _bathTokenAmount) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L343

```solidity
382     function _redeem(address _asset, uint256 _bathTokenAmount) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L382

```solidity
401     function _savePosition( address _asset, address _quote, uint256 _borrowedAmount, uint256 _currentBathTokenAmount ) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L401

```solidity
426     function _updateMargin( uint256 _positionId, uint256 _bathTokenAmount, uint256 _marginAmount ) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L426

```solidity
454      function _marketBuy( address _asset, address _quote, uint256 _maxFill ) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L454

```solidity
475     function _marketSell( address _asset, address _quote, uint256 _minFill ) internal {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L474

```solidity
526      function _borrowLimit( address _bathToken, address _asset, uint256 _assetAmount, uint256 _leverage ) internal returns (uint256 _limit, uint256 _lastBorrow) {

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526

