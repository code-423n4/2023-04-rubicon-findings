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

