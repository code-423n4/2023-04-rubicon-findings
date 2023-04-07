Prevent division by 0

cations in the code precautions are not being taken for not dividing by 0, this will revert the code.
These functions can be called with 0 value in the input, this value is not checked for being bigger than 0, that means in some scenarios this can potentially trigger a division by zero. 


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L41 

            fees[i] = (tokenAmounts[i] * feeValue) / feeType; 

 the division operation (tokenAmounts[i] * feeValue) / feeType may result in a division by zero if feeType is equal to zero. There is no explicit check to ensure that feeType is greater than zero before performing the division.