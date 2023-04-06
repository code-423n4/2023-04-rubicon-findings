1,Combine multiple state changes into a single transaction,
       In the _chargeFee function, there are three separate calls to IERC20 functions to transfer tokens and approve them. These could be combined into a single transaction to reduce the number of state changes and thus save gas.
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L93),

2,Use bytes memory instead of bytes,
           In the CallParams struct, the args field is defined as bytes. It would be more efficient to define it as bytes memory since it is only used temporarily and does not need to be stored in storage.
(https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L12)

