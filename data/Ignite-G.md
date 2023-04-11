### Gas Optimizations
| |Issue|Instances|Average Gas Saved|
|---|---|---|---|
| Gas-1 | Using `storage` instead of `memory` saves gas | 3 | 7391 |
| Gas-2 | State variables that are only set in the `constructor` should be declared `immutable` | 4 | 6300 |
| Gas-3 | Setting the `constructor` to `payable` | 2 | 39 |
| Gas-4 | Using `delete` statement for state variables to save gas | 3 | - |
| Gas-5 | Optimize function names to save gas | All Contracts | - |
| Gas-6 | Use solidity version 0.8.17 or more to gain some gas boost | 2 | - |

### [Gas-1] Using `storage` instead of `memory` saves gas

When fetching data from a `storage` location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from `storage`, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional MLOAD rather than a cheap stack read. 

#### <ins>Instances(3):</ins>

```solidity
75: Position memory pos = positions[posId];

211: Position memory pos = positions[posId
                                     
227: Position memory pos = positions[posId];
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L75

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L211

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L227


#### <ins>Proof of Concept:</ins>

For example, one possible optimization is to change the memory to storage for instances provided in the `Position` contract.

```
yarn run test --grep "Pools Utility Test"
```

<B>Fetching data from a `storage` location, assigning the data to a `memory` variable:</B>

|Solc version: 0.8.17| Optimizer enabled: true | | | |Runs: 5 |Block limit: 30000000 gas|
|---|---|---|---|---|---|---|
|  Contract          |  Method                     |  Min        |  Max        | Avg      |  # calls      |  usd (avg) 
|  Position          |  buyAllAmountWithLeverage   |     694718  |    1320686  |  1062123  |            7  |          - 
|  Position          |  closePosition              |     469339  |     515243  |   500436  |            6  |          - 
|  Position          |  increaseMargin             |     201175  |     201211  |   201193  |            2  |          - 
|  Position          |  sellAllAmountWithLeverage  |     926300  |    1320943  |  1024961  |            4  |          - |

Total Avg Gas: `2,788,713`.

<B>Using `storage` instead of `memory`:</B>

| Solc version: 0.8.17 | Optimizer enabled: true | | | |Runs: 5 |Block limit: 30000000 gas|
|---|---|---|---|---|---|---|
|  Contract          |  Method                     |  Min        |  Max        | Avg      |  # calls      |  usd (avg) 
|  Position          |  buyAllAmountWithLeverage   |     694718  |    1320686  |  1062123  |            7  |          - 
|  Position          |  closePosition              |     468562  |     514467  |   499659  |            6  |          - 
|  Position          |  increaseMargin             |     194561  |     194597  |   194579  |            2  |          - 
|  Position          |  sellAllAmountWithLeverage  |     926300  |    1320943  |  1024961  |            4  |          - |

Total Avg Gas: `2,781,322`.

#### <ins>Recommendation</ins>
Using `storage` instead of `memory` saves gas.

### [Gas-2] State variables that are only set in the `constructor` should be declared `immutable`

Avoids a Gsset (20000 gas) in the `constructor`, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

#### <ins>Instances(4):</ins>

```solidity
55: oracle = PriceOracle(_oracle);
56: rubiconMarket = RubiconMarket(_rubiconMarket);
57: bathHouseV2 = BathHouseV2(_bathHouseV2);
58: comptroller = bathHouseV2.comptroller();
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L55
    
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L56
    
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L57
    
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L58

#### <ins>Recommendation</ins>
State variables that are only set in the `constructor` should be declared `immutable`.

### [Gas-3] Setting the `constructor` to `payable`

Saves 13 gas per instance.

#### <ins>Instances(2):</ins>

```solidity
30: constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30

```solidity
54: constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54

#### <ins>Recommendation</ins>
Setting the `constructor` to `payable`.

### [Gas-4] Using `delete` statement for state variables to save gas
Set a value to zero manually, the compiler will generate additional bytecode to perform operation, which can increase the gas cost.

Using `delete` statement generates less bytecode compared to setting the value to zero manually, resulting in a lower gas cost.

#### <ins>Instances(3):</ins>

```solidity
1449: _near[id] = 0; //delete order from unsorted order list

1462: _near[id] = 0; //delete order from unsorted order list
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462

```solidity
181: tokenRewards[address(rewardsToken)][holderRecipient] = 0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L181

#### <ins>Recommendation</ins>
Use the delete statement to set any state to 0.

### [Gas-5] Optimize function names to save gas

Optimizing function names can save gas because lower method ID will result in smaller gas, which can be significant for contracts with a large number of function calls.

Reference: https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92

#### <ins>Instances:</ins>

All in-scope contracts

#### <ins>Recommendation</ins>
Find a lower method ID name for the most called functions.

### [Gas-6] Use solidity version 0.8.17 or more to gain some gas boost

Consider upgrading to the latest version of Solidity, which is 0.8.17 or higher for additional gas savings.

#### <ins>Instances(2):</ins>


```solidity
pragma solidity ^0.8.9;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

```solidity
pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

#### <ins>Recommendation</ins>
Using solidity compiler version 0.8.17 or higher.
