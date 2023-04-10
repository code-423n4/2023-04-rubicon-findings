1, There is no recovery function to change the value of the "stopped" variable in ExpiringMarket.
Consider add function for admin to change "stopped" variable, or delete relevant code if there is no plan to use this function.
Code here:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L594
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628-L631

2, SafeERC20.safeApprove() is deprecated, should use safeIncreaseAllowance and safeDecreaseAllowance instead, as seen in the comments of the OpenZeppelin code:
https://github.com/OpenZeppelin/openzeppelin-contracts/blob/828fe365eeff13e7aa188e449005ad81f7222189/contracts/token/ERC20/utils/SafeERC20.sol#L39-L44
Affected code:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L358


