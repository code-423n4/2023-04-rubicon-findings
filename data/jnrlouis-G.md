## Gas Optimization Report

# [G-01] `internal` functions only called once can be inlined to save gas

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

*There are 5 instances of this issue:*

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L108

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L93

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L76

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L60

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L568

# [G-02] Functions guaranteed to revert when normal users call can be marked `payable`

If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are
CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

*There are 13 instances of this issue:*

```javascript
File: contracts/utilities/poolsUtility/Position.sol

93      function buyAllAmountWithLeverage(
                address quote,
                address asset,
                uint256 quotePayAmount,
                uint256 leverage
            ) external onlyOwner

107     function sellAllAmountWithLeverage(
                address asset,
                address quote,
                uint256 assetPayAmount,
                uint256 leverage
            ) external onlyOwner

210     function closePosition(uint256 posId) external onlyOwner
        
226     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner

242     function withdraw(address token, uint256 amount) external onlyOwner
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L93

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L107

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L210

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L226

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L242

```javascript
File: contracts/BathHouseV2.sol

60    function createBathToken(
        address underlying,
        InterestRateModel interestRateModel,
        uint256 initialExchangeRateMantissa,
        address implementation,
        bytes memory becomeImplementationData
    ) external onlyAdmin
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L60-L66

```javascript
File: contracts/periphery/BathBuddy.sol

191     function notifyRewardAmount(
        uint256 reward,
        IERC20 rewardsToken
    ) external onlyOwner updateReward(address(0), address(rewardsToken))

232     function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L191

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232

```javascript
File: contracts/RubiconMarket.sol

628         function stop() external auth

965         function getMinSell(
                ERC20 pay_gem //token for which minimum sell amount is queried
            ) external view returns (uint256) {
                return _dust[address(pay_gem)];
            }

1466        function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
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

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L965-L969

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1480

# [G-03] Functions or Arguments not used should be deleted

Functions or Arguments not used should be deleted to save gas.

*There are 2 instances of this issue:*

```javascript
File: contracts/utilities/poolsUtility/Position.sol

526        function _borrowLimit(
                address _bathToken,
                address _asset,
                uint256 _assetAmount,
                uint256 _leverage
            ) internal returns (uint256 _limit, uint256 _lastBorrow) {
```
`_asset` is not used anywhere in the function

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L526

```javascript
File: contracts/RubiconMarket.sol

29         function bump(bytes32 id_) external can_buy(uint256(id_))
```
`bump` function doesn't return or update any variable, and it is not used anywhere.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297


# [G-04] Division by 2 should use bit shifting

<y> / 2 is the same as <y> >> 1. The DIV opcode costs 5 gas, whereas SHR only costs 3 gas

*There are 4 instances of this issue:*

```javascript
File: contracts/RubiconMarket.sol

77    function wmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = add(mul(x, y), WAD / 2) / WAD;
    }


    function rmul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = add(mul(x, y), RAY / 2) / RAY;
    }


    function wdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = add(mul(x, WAD), y / 2) / y;
    }


    function rdiv(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = add(mul(x, RAY), y / 2) / y;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L77-L91

