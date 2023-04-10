## [L-01] ## [L-01] `V2Migrator` imports SafeERC20.sol but uses normal transfer/transferFrom
The V2Migrator contract imports SafeERC20 but uses normal transfer/transferFrom, consider importing normal ERC20 if you don't want to use safeTranfer and safeTransferFrom or use those 2 functions instead of normal transfer/transferFrom

## [L-02] `FeeWrapper` imports SafeERC20.sol but uses normal transfer/transferFrom
The FeeWrapper.sol contract imports SafeERC20.sol and is using it for IERC20 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L10 but the functions safeTransfer/safeTransferFrom are not used anywhere in the contract, consider using them if you work with ERC20 that behave weird (*e.g USDT*) or import and use normal ERC20.sol.
## [L-03] `isClosed()` is used in different instances in `RubiconMarket.sol` but the checks are useless since it returns `false` every time
The function `isClosed()` is used in different instances, for example 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L598
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L605
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L614
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L722 , but the function returns all the time `false` so the checks will be useless since you can't change the outcome as you can see here https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620-L622
## [L-04] Don't keep the mappings from `RubiconMarket` on public if you already have getter functions for them
`RubiconMarket` contract has all the mappings set on public so the solidity compiler will create getter functions for them, but the contract already has getter function for all the mapping 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L965-L969
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L974-L979
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L985-L986
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L993-L995
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L998-L1003
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1010-L1012
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1016-L1018
## [L-05] The check on modifier `onlyBuddy()` from the `BathBuddy.sol` contract is useless since the `msg.sender` can't be the address(0)
The `onlyBuddy()` is checking is `msg.sender` != address(0) as it can be seen here 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L97
but this check is useless since no one knows the private key of the address(0) so the `msg.sender` can never be address(0)