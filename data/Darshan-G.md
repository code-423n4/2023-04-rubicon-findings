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
