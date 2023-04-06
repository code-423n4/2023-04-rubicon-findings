Use SafeERC20 library for token interactions:
The code already imports the SafeERC20 library, but it doesn't use it for token interactions. Using SafeERC20 ensures safe transfers, approvals, and other token operations. Replace the IERC20 calls with the corresponding SafeERC20 functions for added safety.
