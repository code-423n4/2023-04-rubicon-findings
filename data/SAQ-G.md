## Summary

### Gas Optimization

no | Issue |Instances||
|-|:-|:-:|:-:|
| [G&#x2023;01] | EXTERNAL,INTERNAL AND PUBLIC FUNCTION USE THEY VIEW OR PURE KEYWORD |17 | - |
| [G&#x2023;02] | USE A MORE RECENT VERSION OF SOLIDITY | 4 | - |
| [G&#x2023;03] | ++i COSTS LESS GAS THAN i++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--i/i-- TOO)  | 3 | - |
| [G&#x2023;04] | SAVING MORE THEN ONE UINT256,BYTES32 IN ONE SLOT  | 7 | - |
| [G&#x2023;05] | USING ^ INSTEAD OF != | 11 | - |
| [G&#x2023;06] | INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS  | 11 | - |
| [G&#x2023;07] | DIVISION BY TWO SHOULD USE BIT SHIFTING | 8 | - |
| [G&#x2023;08] | USE UINT256 INSTEAD OF UINT8 | 2 | - |
| [G&#x2023;09] | <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES  | 1 | - |
| [G&#x2023;10] | USE FIXED SIZE BYTE  | 3 | - |
| [G&#x2023;11] | PUBLIC FUNCTIONS TO EXTERNAL |24| - |
| [G&#x2023;12] | USE BYTES32 IMMUTEABLE  | 1 | - |
|[G&#x2023;13]  | Call block.timestamp direclty instead of function | 1 | - |
|[G&#x2023;14]  | Use double if statements instead of && | 4 | - |

## Gas Optimizations  

### [G-1] EXTERNAL,INTERNAL AND PUBLIC FUNCTION USE THEY VIEW OR PURE KEYWORD
```solidity
file:   Position.sol
65      function borrowBalance(address bathToken) public returns (uint256 balance) 
130     internal returns (bool OK)
272     function _borrow(address _cToken, uint256 _amount) internal
280     function _repay(address _asset, address _quote, uint256 _posId) internal
336     function _enterMarkets(address _bathToken) internal
343     function _exitMarket(address _bathToken) internal 
382     function _redeem(address _asset, uint256 _bathTokenAmount) internal 
406     function _savePosition(
        address _asset,
        address _quote,
        uint256 _borrowedAmount,
        uint256 _currentBathTokenAmount
    ) internal 
421     function _removePosition(uint256 _positionId) internal
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L65
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L130
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L272
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L280
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L336 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L343
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L382
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L406
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L421

### [G-1] EXTERNAL, INTERNAL AND PUBLIC FUNCTION USE THEY VIEW OR PURE KEYWORD
```solidity
file:   RubiconMarket.sol

568     function _next_id() internal returns (uint256) 

700     function initialize(address _feeTo) public 

824     function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        bool matching, //match "close enough" orders?
        address owner, // owner of the offer
        address recipient // recipient of the offer's fill
    ) public can_offer returns (uint256) 

1183    function _buys(uint256 id, uint256 amount) internal returns (bool)

1273    function _matcho(
        uint256 t_pay_amt, //taker sell how much
        ERC20 t_pay_gem, //taker sell which token
        uint256 t_buy_amt, //taker buy how much
        ERC20 t_buy_gem, //taker buy which token
        uint256 pos, //position id
        bool rounding, //match "close enough" orders?
        address owner,
        address recipient
    ) internal returns (uint256 id)    

1362     function _sort(
        uint256 id, //maker (ask) id
        uint256 pos //position to insert into
    ) internal

1405    function _unsort(
        uint256 id //id of maker (ask) offer to remove from sorted list
    ) internal returns (bool)

1438    function _hide(
        uint256 id //id of maker offer to remove from unsorted list
    ) internal returns (bool)

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L824-L833
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1183
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1273-L1282
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1362-L1365
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1405-L1407
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1438-L1440

### [G-2] USE A MORE RECENT VERSION OF SOLIDITY

```solidity
file: BathHouseV2.sol
2     pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L2
 
```solidity
file: V2Migrator.sol
2     pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L2


```solidity
file: Position.sol
2     pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L2

```solidity
file: FeeWrapper.sol
2     pragma solidity 0.8.17;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L2

### [G-3] ++i COSTS LESS GAS THAN i++, ESPECIALLY WHEN IT’S USED IN FOR-LOOPS (--i/i-- TOO)
```solidity
file:   RubiconMarket.sol
899     for (uint i = 0; i < payAmts.length; i++) 
911     function batchCancel(uint[] calldata ids) external {
        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
        }
    }
924     for (uint i = 0; i < ids.length; i++) 

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924


### [G-4] SAVING MORE THEN ONE UINT256,BYTES32 IN ONE SLOT 
```solidity
file:   RubiconMarket.sol
118     event emitOffer(
        bytes32 indexed id,
        bytes32 indexed pair,
        address indexed maker,
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    );     
686     struct sortInfo {
        uint256 next; //points to id of next higher offer
        uint256 prev; //points to id of previous lower offer
        uint256 delb; //the blocknumber where this entry was marked for delete
    }
802     function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        address owner,
        address recipient
    )
1283    uint256 best_maker_id; //highest maker id
        uint256 t_buy_amt_old; //taker buy how much saved
        uint256 m_buy_amt; //maker offer wants to buy this much token
        uint256 m_pay_amt   
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L118
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L686
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L802
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1283

### [G-4] SAVING MORE THEN ONE UINT256,BYTES32 IN ONE SLOT 
```solidity
file:   Position.sol
23      struct PosVars {
        uint256 borrowedAmount; // how much to borrow in current borrow loop
        uint256 initAssetBalance; // initial balance of asset
        uint256 limit; // limit of borrowing loops
        uint256 lastBorrow; // how much borrow on the last loop (check _borrowLimit)
        uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution
        uint256 currentAssetBalance; // balance of asset in the moment of execution
        uint256 toBorrow; // certain perc. to borrow from max amount available to borrow
    }
426    function _updateMargin(
        uint256 _positionId,
        uint256 _bathTokenAmount,
        uint256 _marginAmount
    ) 
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L23
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L426

### [G-4] SAVING MORE THEN ONE UINT256,BYTES32 IN ONE SLOT 
```solidity
file: FeeWrapper.sol
19      struct FeeParams {
        address feeToken; // token in the form of which fee paid
        uint256 totalAmount; // amount without deducted fee
        uint256 feeAmount; // amount of feeToken from which fee should be charged
        address feeTo; // receiver of the fee
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L19

### [G-5] USING ^ INSTEAD OF !=

```solidity
file:    RubiconMarket.sol
349      if (_offer.owner == address(0) && getRecipient(id) != address(0))
702      require(_feeTo != address(0));
1022     _rank[id].next != 0 ||
1023     _rank[id].prev != 0 ||     
1040     require(offerId != 0, "0 offerId");
1217     while (top != 0 && _isPricedLtOrEq(id, top))
1229     while (pos != 0 && !isActive(pos))
1244     while (pos != 0 && _isPricedLtOrEq(id, pos))
1252     while (pos != 0 && !_isPricedLtOrEq(id, pos)) 

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L702
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1022
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1023
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1040
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1217
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1229
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1244
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1252


### [G-5] USING ^ INSTEAD OF !=

```solidity
file:    BathBuddy.sol
250      if (account != address(0))

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L250

### [G-5] USING ^ INSTEAD OF !=

```solidity
file:    Position.sol
176      if (i.add(1) == vars.limit && vars.lastBorrow != 0)     

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L176


### [G-6]  INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS  

```solidity
file:    BathHouseV2.sol
137     function _bathify(
        address _underlying
    )
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L137-L142

### [G-6]  INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS  

```solidity
file:    Position.sol
326     function _calculateDebt(
        address _bathToken,
        uint256 _startBlock,
        uint256 _borrowedAmount
    ) internal

343     function _exitMarket(address _bathToken) internal {
     

383     function _redeem(address _asset, uint256 _bathTokenAmount) internal {
        



401     function _savePosition(
        address _asset,
        address _quote,
        uint256 _borrowedAmount,
        uint256 _currentBathTokenAmount
    ) internal 

421     function _removePosition(uint256 _positionId) internal
426     function _updateMargin(
        uint256 _positionId,
        uint256 _bathTokenAmount,
        uint256 _marginAmount
    ) internal 
454     function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal
475     function _marketSell(
        address _asset,
        address _quote,
        uint256 _minFill
    ) internal
526     function _borrowLimit(
        address _bathToken,
        address _asset,
        uint256 _assetAmount,
        uint256 _leverage
    ) internal returns
586     function _leverageCheck(uint256 _leverage, bool _long) internal pure

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L322-L326
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L343-L344
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L382-L383
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L401-L406
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L421
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L426-L430
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L454-L458
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L475-L479
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526-L531
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L586


### [G-7] DIVISION BY TWO SHOULD USE BIT SHIFTING 

```solidity
file:    RubiconMarket.sol

55       require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

78       z = add(mul(x, y), WAD / 2) / WAD;

82       z = add(mul(x, y), RAY / 2) / RAY;

86       z = add(mul(x, WAD), y / 2) / y;

90       z = add(mul(x, RAY), y / 2) / y;

1099     mul(buy_amt, 10 ** 9),

1142     mul(pay_amt, 10 ** 9),

1175     mul(buy_amt, 10 ** 9),

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L55
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L78
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L82
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L86
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L90
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1099
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1142
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1175

### [G-8]  USE UINT256 INSTEAD OF UINT8  

```solidity
file:     BathHouseV2.sol

82       (string memory name, string memory symbol, uint8 decimals) = _bathify

142      returns (string memory _name, string memory _symbol, uint8 _decimals)

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L82
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L142


### [G-9] <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES
```solidity
file:    Position.sol
431      positions[_positionId].bathTokenAmount += _bathTokenAmount;

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431


### [G-10]  USE FIXED SIZE BYTE
```solidity
file:    RubiconMarket.sol

297     function bump(bytes32 id_) external can_buy(uint256(id_)) {
        uint256 id = uint256(id_);

487     function kill(bytes32 id) external virtual {
        require(cancel(uint256(id)));
    }

564     function take(bytes32 id, uint128 maxTakeAmount) external virtual {
        require(buy(uint256(id), maxTakeAmount));
    }

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L298
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L487-L489
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L564-L566

### [G-11]  PUBLIC FUNCTIONS TO EXTERNAL
```solidity
file:   RubiconMarket.sol

511     function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id)  

574     function getFeeBPS() public view returns (uint256)

578      function calcAmountAfterFee(
        uint256 amount
    ) public view returns (uint256 _amount) 

620     function isClosed() public pure returns (bool closed)

624     function getTime() public view returns (uint64) 

733     function make(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    ) public override returns (bytes32)

752    function take(bytes32 id, uint128 maxTakeAmount) public override 

824     function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        bool matching, //match "close enough" orders?
        address owner, // owner of the offer
        address recipient // recipient of the offer's fill
    ) public can_offer returns (uint256)

855     function buy(
        uint256 id,
        uint256 amount
    ) public override can_buy(id) returns (bool)

871     function cancel(
        uint256 id
    ) public override can_cancel(id) returns (bool success)  

974    function getBestOffer(
        ERC20 sell_gem,
        ERC20 buy_gem
    ) public view returns (uint256)    


985    function getWorseOffer(uint256 id) public view returns (uint256)

998    function getOfferCount(
        ERC20 sell_gem,
        ERC20 buy_gem
    ) public view returns (uint256)


1010   function getFirstUnsortedOffer() public view returns 


1016   function getNextUnsortedOffer(uint256 id) public view returns

1020   function isOfferSorted(uint256 id) public view returns

1128   function sellAllAmount(
        ERC20 pay_gem,
        uint256 pay_amt,
        ERC20 buy_gem,
        uint256 min_fill_amount
    ) external returns (uint256 fill_amt) 

1149   function getPayAmountWithFee(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint256 buy_amt
    ) public view returns (uint256 fill_amt) 

1157   function getPayAmount(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint256 buy_amt
    ) public view returns (uint256 fill_amt) 


```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L518
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L578-L580
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733-L738
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L752
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L824-L833
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L855-L858
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871-L873
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L974-L977
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L985
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L998-L1001
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1020
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028-L1033
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1149-L1153
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1157-L1161


### [G-11]   PUBLIC FUNCTIONS TO EXTERNAL
```solidity
file:   BathHouseV2.sol
45      function getBathTokenFromAsset(
        address asset
    ) public view returns (address)      

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L45-L47

```solidity
file:   BathBuddy.sol

139     function earned(
        address account,
        address token
    ) public view override returns (uint256)     

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L139-L142


```solidity
file:   Position.sol

65      function borrowBalance(address bathToken) public returns (uint256 balance)

72      function borrowBalanceOfPos(
        uint256 posId
    ) public view returns (uint256 balance)

86     function borrowRate(address bathToken) public view returns (uint256 rate)

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L65
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L72-L74
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L86

### [G-12]   USE BYTES32 IMMUTEABLE 
```solidity
file:     RubiconMarket.sol

232       bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232


### [G‑13] Call block.timestamp direclty instead of function
```solidity
file:   RubiconMarket.sol
624     function getTime() public view returns (uint64) {
        return uint64(block.timestamp);
    } 

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624-L626


### [G-14]  Use double if statements instead of && 
```solidity
file: RubiconMarket.sol

349  if (_offer.owner == address(0) && getRecipient(id) != address(0))
1324 if (
            t_buy_amt > 0 &&
            t_pay_amt > 0 &&
            t_pay_amt >= _dust[address(t_pay_gem)]
        )

```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1324-L1328

### [G-14]  Use double if statements instead of && 

```solidity
file:    Position.sol
176      if (i.add(1) == vars.limit && vars.lastBorrow != 0)     
391       if (
            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
            borrowBalance(_bathTokenAsset) == 0
        )      
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L176
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L391-L394
