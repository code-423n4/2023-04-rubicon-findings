## Unused functions
Unused functions may be removed to reduce contract size and save gas on deployment.

Here is a specific instance entailed:

[File:https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
594:    bool public stopped;

628:    function stop() external auth {
629:        stopped = true;
630:    }
```