# [G-01] Use unchecked when require() prevents it from overflowing (2 instances)
Since `tokenAmounts[i]` is sways bigger that `fees[i]`, we can safely add `unchecked{}` to lower the amount of gas **SAVED - 216**  
[utilities/FeeWrapper.sol/L42](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L42)


      for (uint256 i = 0; i < tokenAmounts.length; ++i) {
          fees[i] = (tokenAmounts[i] * feeValue) / feeType;
          unchecked{
          amountsWithFee[i] = tokenAmounts[i] - fees[i];
          }
      }

Here is the same since `_feeAmount` will always be lower that `msg.value` **SAVED - 182** 
[utilities/FeeWrapper.sol/L120](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/FeeWrapper.sol#L120)


     unchecked{
        _msgValue = msg.value - _feeAmount;
     }



# [G-02] Remove unnecessary require() (1 instance)
In [BathHouseV2.sol/L144](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/BathHouseV2.sol#L144) the `require(_underlying != address(0)` is unnecessary since in the previous function `createBathToken`  `_underlying` is already checked if it is `address(0)`.


     require(underlying != address(0);
     (string memory name, string memory symbol, uint8 decimals) = _bathify(underlying);


     function _bathify(address _underlying)
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)
    {
        require(_underlying != address(0), "_bathify: ADDRESS ZERO");  //@audit this can be removed

        _name = string.concat("bath", ERC20(_underlying).symbol());
        _symbol = string.concat(_name, "v2");
        _decimals = ERC20(_underlying).decimals();
    }

# [G-03] Change place in Position struct (1 instance)
In [Position.sol/L39](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L39) the boolean `isActive` can be moved up to the third place in order to use less storage slots, thus saving in memory.
Change to:

    struct Position {
        address asset; 
        address quote;
    +   bool isActive; 
        uint256 borrowedAmount; 
        uint256 bathTokenAmount; 
        uint256 blockNum; 
    -   bool isActive; 
    }

# [G-04] Use function instead of modifiers (12 instances)
Saving on RubiconMarket of about ~175 221 gas

[BathBuddy.sol/L87](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L87)
[BathBuddy.sol/L94](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94)
[BathBuddy.sol/L104](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L104)
[BathBuddy.sol/L247](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L247)

[BathHouseV2.sol/L26](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L26)

[RubiconMarket.sol/L30](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L30)
[RubiconMarket.sol/L245](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L245)
[RubiconMarket.sol/L251](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251)
[RubiconMarket.sol/L260](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260)
[RubiconMarket.sol/L264](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264)
[RubiconMarket.sol/L643](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L643)
[RubiconMarket.sol/L719](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L719)

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

Change to:

    function _onlyOnwer() internal {
         require(msg.sender == owner);
    }


# [G-05] a=a+b is always cheaper that a+=b, same for a-=b (5 instances)
[RubiconMarket.sol/L583](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583) 
[RubiconMarket.sol/L586](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586)
[Position.sol/L184](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L184)
[Position.sol/L431](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431)
[Position.sol/L560](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L560)


# [G-06] Delete mappings instead of setting them to 0 (1 instance)
It it much cheaper to delete the mapping if it is unwanted then to set it to 0
[RubiconMarket.sol/L1438](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1438)

# [G-07] There is no need to load variables into memory (1 instance)

[Position.sol/L78-L80](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L78-L80)
 
        uint256 blockNum = pos.blockNum;
        address bathTokenQuote = bathHouseV2.getBathTokenFromAsset(pos.quote);
        uint256 borrowedAmount = pos.borrowedAmount;

        balance = _calculateDebt(bathTokenQuote, blockNum, borrowedAmount); 

Change to: 

         balance = _calculateDebt(bathHouseV2.getBathTokenFromAsset(pos.quote), pos.blockNum, pos.borrowedAmount);

# [G-08] Using double if/require  instead of && saves gas (4 instances)
Saves ~8 gas per && instance
Using double require/if instead of && saves on gas.
When having a require/if statement with 2 or more expressions needed,always place the expression that cost less gas first. 

[Position.sol/L392](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L392)
[RubiconMarket.sol/L893-L898](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893-L898)
[RubiconMarket.sol/L1200](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1200)
[RubiconMarket.sol/L1324-L1327](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1324-L1327)

# [G-09] Duplicated require() checks should be moved to a modifier/function

[BathBuddy.sol/L122](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L122)
[BathBuddy.sol/L143](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L143)

    require(friendshipStarted, "I have not started a bathToken friendship");

[Position.sol/L76](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L76)
[Position.sol/L212](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L212)
[Position.sol/L228](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L228)

    require(pos.isActive, "increaseMargin: POS ISN'T ACTIVE");

# [G-10] Miscellaneous(3 instances)
Saved gas: ~7816 on deployment 

-Remove unused function [`Bump`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310)

-Remove [`can_offer()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260-L262)

-Remove `getTime()` since it is not used and it is cheaper to call block.timestamp anyway 
[RubiconMarket.sol/L624](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624-L626)

**Tools used: Foundry and remix**

