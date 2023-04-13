# [Q-1] It is recomended to divide by constant variable instead of a number --10 ** 9

####proof of concept:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1059
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1101
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1144
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1177


# [Q-2] Use Netspec for the assembly 
####proof of concept:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1177

```solidity
648:        assembly {
649:            foo := calldataload(4)
650:            bar := calldataload(36)
651:            wad := callvalue()
652:        }