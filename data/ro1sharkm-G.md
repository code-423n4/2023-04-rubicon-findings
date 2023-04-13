## Splitting require() statements that use && saves gas 
Splitting saves 8 gas per &&
Instances:

```
file : contracts/RubiMarket.sol

893.      require(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );
```
Others:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L940-L943

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1324-L1328

## Emitting storage values instead of the memory one.
Values emitted shouldn’t be read from storage. The existing memory values should be used instead:
instance:
```
        rewardsDuration[token] = _rewardsDuration;
        emit RewardsDurationUpdated(rewardsDuration[token]);
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L240-L241