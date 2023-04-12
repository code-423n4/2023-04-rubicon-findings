| Number |Details|Instances|Gas saved|
|:--:|:-------|:--:|:--:|
|[G-01]| Using bools for storage incurs overhead (c4udit extended) | 1 | 17100 |
|[G-02]| Superfluous event emission | 1 | 375 |
|[G-03]| (Suggestion) Batch multiple liquidity migration in a single call |  | 21000 (\*) |
|[G-04]| State variables should be cached in stack variables rather than re-reading them from storage | 1 | 100 |
|[G-05]| Storage packing | 2 |  |
|[G-05-1]| Pack `BathBuddy` mappings into one struct |  | 4000 |
|[G-05-2]| `Position.Position` struct |  | 34200 |


## [G-01] Using bools for storage incurs overhead (c4udit extended)

Applicable to the re-entrancy guard found in contract `SimpleMarket`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

Since a `boolean` takes up 8 bits in the storage, the `locked` variable should be set to `uint8` to avoid storage collision when upgrading. The variable should also be initialized properly to 1 (or 2) after upgrading.

This is a case of **[GAS-2]** found by the automated tool, however this instance was not caught by the tool. See [4naly3er report](https://gist.github.com/Picodes/01a3dd4b07d396e51b16fa758dad0fa5#gas-2-using-bools-for-storage-incurs-overhead) for detailed impact.

## [G-02] Superfluous event emission

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L109-L110

The two events emitted always go together, furthermore they emits repeated info (`bathToken` is emitted twice).

We recommend emitting only one event, defined as so: 
```solidity=
event BathTokenCreated(address bathToken, address underlying, address bathBuddy);
```

Saves one event emission (375 gas), as well as one less memory access, without any loss of information.

## [G-03] (Suggestion) Batch multiple liquidity migration in a single call

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L38

The function `migrate()` in `V2Migrator` allows for migrating liquidity one-at-a-time. We suggest adding a function for allowing users to migrate liquidity for multiple pools at a time, eliminating the need for multiple transactions. Saves **21000 gas** (base gas cost) for each transaction saved.

However, care should be taken such that, if one migration fails, it doesn't revert the entire transaction and preventing migration of other pools. Possible solutions include using low-level calls, or to allow users to select a subset of pools to migrate.

## [G-04] State variables should be cached in stack variables rather than re-reading them from storage

This finding extends upon **[GAS-04]** of C4udit.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L112-L119

```solidity
function lastTimeRewardApplicable(
    address token
) public view returns (uint256) {
    return
        block.timestamp < periodFinish[token]
            ? block.timestamp
            : periodFinish[token];
}
```

The function above accesses `periodFinish[token]` twice from storage. Although this is a view function, this function is called from [the `updateReward` modifier](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L249), which gets called every time a user claims their reward.

```solidity=
lastUpdateTime[token] = lastTimeRewardApplicable(token);
```

Saves 100 gas per instance (see automated report for details).

## [G-05] Storage packing

### [G-05-1] Pack `BathBuddy` mappings into one struct

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52-L56

The following mappings all have the same key meaning:

```solidity
mapping(address => uint256) public periodFinish; // Token specific
mapping(address => uint256) public rewardRates; // Token specific reward rates
mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
mapping(address => uint256) public lastUpdateTime; //Token specific
mapping(address => uint256) public rewardsPerTokensStored; // Token specific
```

All of these mappings are different attributes of the same reward token. We suggest packing all said values into a single struct as shown. Note that we also attempted to safely pack the relevant variables into fewer storage slots.

```solidity
struct RewardData {
    uint96 periodFinish;
    uint96 rewardsDuration;
    uint64 lastUpdateTime; 
    // note that this is only one storage slot
    uint256 rewardRates;
    uint256 rewardsPerTokensStored;
}

mapping(address => RewardData) public rewardData;
```

For the user claiming reward, since all of the users' data are accessed using one reward claim, this essentially converts two Gcoldsload (2100 gas) into Gwarmaccess (100 gas), saving **4000** gas in total.

### [G-05-2] `Position.Position` struct

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L33-L40

Note the current storage layout of `Position` struct

```solidity
struct Position {
    address asset;
    address quote;
    uint256 borrowedAmount;
    uint256 bathTokenAmount;
    uint256 blockNum;
    bool isActive;
}
```

Each variable takes up one storage slot. We can reduce **two storage slots** without loss of safely with the following layout:

```solidity
struct Position {
    address asset;
    uint96 blockNum; // end of slot 1
    address quote;
    bool isActive;  // end of slot 2
    uint256 borrowedAmount;  // end of slot 3
    uint256 bathTokenAmount; // end of slot 4
}
```

A storage write that changes value from zero to non-zero uses one Gsset (20000 gas), whereas from non-zero to non-zero uses one Gsreset (2900 gas). The change will convert two Gsset to Gsreset, saving **34200 gas** for every positions created.
