## Prevent division by 0

cations in the code precautions are not being taken for not dividing by 0, this will revert the code.
These functions can be called with 0 value in the input, this value is not checked for being bigger than 0, that means in some scenarios this can potentially trigger a division by zero. 


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L41 

            fees[i] = (tokenAmounts[i] * feeValue) / feeType; 

 the division operation (tokenAmounts[i] * feeValue) / feeType may result in a division by zero if feeType is equal to zero. There is no explicit check to ensure that feeType is greater than zero before performing the division.


## variable = 0

Setting a variable to 0 will actually waste gas. it should get inlined to save gas:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1062

               pay_amt = 0; //All amount is sold

               buy_amt = 0; //All amount is bought 

                uint256 old_top = 0;
                _near[id] = 0; //delete order from unsorted order list



## Inline a modifier that’s only used once 

As   modifier onlyAdmin()    is only used once in this contract 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L66 


    modifier onlyAdmin() {
        require(msg.sender == admin, "onlyAdmin: !admin");
        _;
    } 


    function createBathToken(
        address underlying,
        InterestRateModel interestRateModel,
        uint256 initialExchangeRateMantissa,
        address implementation,
        bytes memory becomeImplementationData
    ) external onlyAdmin {
        // underlying can be used only for one bathToken
        require(
            tokenToBathToken[underlying] == address(0),
            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
        );
        require(
            underlying != address(0),
            "createBathToken: UNDERLYING == ADDRESS 0"
        );
        require(
            implementation != address(0),
            "createBathToken: IMPLEMENTATION == ADDRESS 0"
        );


Refer - https://code4rena.com/reports/2022-03-timeswap/#low-risk-and-non-critical-issues 


## > 0 is less efficient than != 0 for unsigned integers 

 Recommended to use the != 0 operator instead of the > 0 operator when comparing unsigned integers to zero in Solidity, as it can help to optimize gas costs and contract execution time.

https://code4rena.com/reports/2022-03-timeswap/#low-risk-and-non-critical-issues 



https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L180 

  if (reward > 0) { 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L41 

        require(bathBalance > 0, "migrate: ZERO AMOUNT"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L313 

        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L318 

        require(_max > 0, "_maxBorrow: can't borrow 0");

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L277 
        return offers[id].timestamp > 0; 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345 
        if (makerFee() > 0) {   

   
	   require(pay_amt > 0);
        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
        require(buy_amt > 0);

       require(amount > 0);
        _amount = amount;
        _amount -= mul(amount, feeBPS) / 100_000;

        if (makerFee() > 0) {
            _amount -= mul(amount, makerFee()) / 100_000;
        }

       while (pay_amt > 0) { 

      while (buy_amt > 0) { 

         if (pay_amt > 0) { 

       require(id > 0); 

        while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) { 

         t_buy_amt > 0 &&
            t_pay_amt > 0 &&
            t_pay_amt >= _dust[address(t_pay_gem)]
        ) { 

        require(_span[pay_gem][buy_gem] > 0);

        while (uid > 0 && uid != id) {


## solidity v0.8.0 SafeMath library is used by default in arithmetic operations

Since solidity v0.8.0 SafeMath library is used by default in arithmetic operations. Using external SafeMath libraries is unnecessary. 

Proof of Concept
https://blog.soliditylang.org/2020/12/16/solidity-v0.8.0-release-announcement/#:~:text=the%20near%20future.-,Checked,-arithmetic%2C%20i.e

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6 



## Use custom errors to save deployment and runtime costs in case of revert

Instead of using strings for error messages, you can use custom errors to reduce both deployment and runtime gas costs. In addition, they are very convenient as you can easily pass dynamic information to them. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L70 

        require(_OK, "low-level call to the Rubicon failed");
 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L86 

         require(_OK, "low-level call to the router failed");

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115 

        require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L119 
        	
	require(OK, "ETH transfer failed");

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L76 
        
         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L100 


    require(
            openPosition(quote, asset, quotePayAmount, leverage),
            "buyAllAmountWithLeverage: FAILED TO OPEN POSITION"
        );
    }

    /// @notice open short position in Rubicon Market
    function sellAllAmountWithLeverage(
        address asset,
        address quote,
        uint256 assetPayAmount,
        uint256 leverage
    ) external onlyOwner {
        _leverageCheck(leverage, false);
        require(
            openPosition(asset, quote, assetPayAmount, leverage),
            "sellAllAmountWithLeverage: FAILED TO OPEN POSITION"
        );
    }

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L212 

        require(pos.isActive, "closePosition: POS ISN'T ACTIVE");

        // load values to memory
        address asset = pos.asset;
        address quote = pos.quote;
        uint256 bathTokenAmount = pos.bathTokenAmount;

        _repay(asset, quote, posId);
        _redeem(asset, bathTokenAmount);

        _removePosition(posId);
    }

    /// @notice add more collateral to certain position
    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
        Position memory pos = positions[posId];
        require(pos.isActive, "increaseMargin: POS ISN'T ACTIVE");

        address asset = pos.asset;
 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L273 

        require(
            CErc20Interface(_cToken).borrow(_amount) == 0,
            "_borrow: BORROW FAILED"
        );
    }

    /// @notice repay debt + interest
    function _repay(address _asset, address _quote, uint256 _posId) internal {
        address _bathTokenQuote = bathHouseV2.getBathTokenFromAsset(_quote);
        uint256 _amountToRepay = borrowBalanceOfPos(_posId);

        // sell asset for quote
        _rubiconSwap(_asset, _quote, _amountToRepay, false);
        uint256 _quoteBalance = IERC20(_quote).balanceOf(address(this));

        require(
            _amountToRepay <= _quoteBalance,
            "_repay: balance of quote lt. debt"
        );
        uint256 _borrowBalance = borrowBalance(_bathTokenQuote);
        if (_amountToRepay > _borrowBalance) {
            _amountToRepay = _borrowBalance;
        }

        IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
        require(
            CErc20Interface(_bathTokenQuote).repayBorrow(_amountToRepay) == 0,
            "_repay: ERROR"
        );
    }

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L312

        require(_err == 0, "_maxBorrow: ERROR");
        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
        require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

        uint256 _price = oracle.getUnderlyingPrice(CToken(_bathToken));
        _max = (_liq.mul(10 ** 18)).div(_price);
        require(_max > 0, "_maxBorrow: can't borrow 0");
    }

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L340

   require(_errs[0] == 0);
    }

    function _exitMarket(address _bathToken) internal {
        require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");
    }

    //============================= COLLATERAL =============================

    /// @notice supply collateral to cToken's market
    function _supply(
        address _token,
        address _bathToken,
        uint256 _amount
    ) internal returns (uint256 _bathTokenAmount) {
        uint256 _initBathTokenAmount = IERC20(_bathToken).balanceOf(
            address(this)
        );
        IERC20(_token).safeApprove(_bathToken, _amount);
        require(
            CErc20Interface(_bathToken).mint(_amount) == 0,
            "_supply: MINT FAILED"
        );
        uint256 _currentBathTokenAmount = IERC20(_bathToken).balanceOf(
            address(this)
        );
 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L385 

    require(
            CErc20Interface(_bathTokenAsset).redeem(_bathTokenAmount) == 0,
            "_redeem: REDEEM FAILED"
        );

        // exit bathToken market only if there is no collateral and debt in it
        if (
            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
            borrowBalance(_bathTokenAsset) == 0
        ) {
            _exitMarket(_bathTokenAsset);
        }
    }

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L591

       _long // long can't be with 1x leverage
            ? require(
                _leverage > _wad && _leverage <= _leverageMax,
                "_leverageCheck{Long}: INVLAID LEVERAGE"
            )
            : require(
                _leverage >= _wad && _leverage <= _leverageMax,
                "_leverageCheck{Short}: INVLAID LEVERAGE"
            );
    }
}
 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L76 

        require(!friendshipStarted, "I already have a buddy!");
        owner = _owner;
        myBathTokenBuddy = newBud;
        bathHouse = _bathHouse;

        // Note, rewards duration must be set by admin

        // Constructor pattern
        friendshipStarted = true;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    // TODO: do we need this?
    // Enforce only soul-bound Bath Token has calling rights
    modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }

    modifier onlyBathHouse() {
        require(msg.sender == bathHouse, "You are not my beloved bath house!");
        _;
    } 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L122 

        require(friendshipStarted, "I have not started a bathToken friendship");

        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
            return rewardsPerTokensStored[token];
        }
        return
            rewardsPerTokensStored[token].add(
                lastTimeRewardApplicable(token)
                    .sub(lastUpdateTime[token])
                    .mul(rewardRates[token])
                    .mul(1e18)
                    .div(IERC20(myBathTokenBuddy).totalSupply())
            );
    }

    // Determines a user rewards
    // Note, uses share logic from bathToken
    function earned(
        address account,
        address token
    ) public view override returns (uint256) {
        require(friendshipStarted, "I have not started a bathToken friendship");
 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L217

        require(
            rewardRates[address(rewardsToken)] <=
                balance.div(rewardsDuration[address(rewardsToken)]),
            "Provided reward too high"
        );

        lastUpdateTime[address(rewardsToken)] = block.timestamp;
        periodFinish[address(rewardsToken)] = block.timestamp.add(
            rewardsDuration[address(rewardsToken)]
        );
        emit RewardAdded(reward);
    }

    // This must be set before notifying a new rewards program for a given token
    // Must be used before? notifyRewardAmount to set the new period
    function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );
        rewardsDuration[token] = _rewardsDuration;
        emit RewardsDurationUpdated(rewardsDuration[token]);
    }

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L41 

        require(bathBalance > 0, "migrate: ZERO AMOUNT"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L54

        require(
            CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
            "migrate: MINT FAILED"
        );
        /// @dev v2 bathTokens shouldn't be sent to this contract from anywhere other than this function
        IERC20(bathTokenV2).transfer(
            msg.sender,
            IERC20(bathTokenV2).balanceOf(address(this))
        );
        require(
            IERC20(bathTokenV2).balanceOf(address(this)) == 0,
            "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
        ); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L27

        require(msg.sender == admin, "onlyAdmin: !admin");
        _;
    }

    // proxy-constructor
    function initialize(address _comptroller, address _pAdmin) external {
        require(!initialized, "BathHouseV2 already initialized!"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L68 

        require(
            tokenToBathToken[underlying] == address(0),
            "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
        );
        require(
            underlying != address(0),
            "createBathToken: UNDERLYING == ADDRESS 0"
        );
        require(
            implementation != address(0),
            "createBathToken: IMPLEMENTATION == ADDRESS 0"
        ); 


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L144 

        require(_underlying != address(0), "_bathify: ADDRESS ZERO"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol 
