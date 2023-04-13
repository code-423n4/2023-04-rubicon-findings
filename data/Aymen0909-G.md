# Gas Optimizations

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1  | State variables only set in the constructor should be declared `immutable`  | 4 |
| 2  | Variables inside struct should be packed to save gas  | 1  |
| 3  | Multiple address/IDs mappings can be combined into a single mapping of an address/id to a struct | 7 |
| 4  | Using `storage` instead of `memory` for struct/array saves gas | 2 |
| 5  | `storage` variable should be cached into `memory` variables instead of re-reading them  |  8 |
| 6  | `memory` values should be emitted in events instead of `storage` ones  | 2 |


## Findings

### 1- State variables only set in the constructor should be declared `immutable` :

The state variables only set in the constructor should be declared `immutable`, this avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

There are 4 instances of this issue:

File: Position.sol [Line 44-47](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L44-L47)
```
Comptroller public comptroller;
PriceOracle public oracle;
RubiconMarket public rubiconMarket;
BathHouseV2 public bathHouseV2;
```

### 2- Variables inside `struct` should be packed to save gas :

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings.

There is 1 instance of this issue:

File: Position.sol [Line 33-40](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L33-L40)

```
struct Position {
    address asset; // supplied as collateral
    address quote; // borrowed token
    uint256 borrowedAmount; // amount of borrowed quote
    uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
    uint256 blockNum; // block number on which position was opened
    bool isActive; // false by default, when active - true
}
```

The boolean variable `isActive` can be packed with the address variable `quote` to save gas, the struct should be modified as follow :

```
struct Position {
    address asset; // supplied as collateral
    address quote; // borrowed token
    bool isActive; // false by default, when active - true
    uint256 borrowedAmount; // amount of borrowed quote
    uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
    uint256 blockNum; // block number on which position was opened
}
```

And as `blockNum` represents the value of the chain block number it can not really overflow `2^96`, thus its value can be packed with an address variable which only takes 20 bytes, the struct should be further modified as follow :

```
struct Position {
    address asset; // supplied as collateral
    uint96 blockNum; // block number on which position was opened
    address quote; // borrowed token
    bool isActive; // false by default, when active - true
    uint256 borrowedAmount; // amount of borrowed quote
    uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
}
```

### 3- Multiple address/IDs mappings can be combined into a single mapping of an address/id to a struct :

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations.

There are 7 instances of this issue :

File: BathBuddy.sol [Line 52-61](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L52-L61)
```
mapping(address => uint256) public periodFinish; // Token specific
mapping(address => uint256) public rewardRates; // Token specific reward rates
mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
mapping(address => uint256) public lastUpdateTime; //Token specific
mapping(address => uint256) public rewardsPerTokensStored; // Token specific

// Token then user always
mapping(address => mapping(address => uint256))
    public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```

These mappings could be refactored into the following struct and mapping for example :

```
struct Token {
    uint256 periodFinish;
    uint256 rewardRates; 
    uint256 rewardsDuration; 
    uint256 lastUpdateTime; 
    uint256 rewardsPerTokensStored; 
    mapping(address => uint256) userRewardsPerTokenPaid; 
    mapping(address => uint256) tokenRewards;
}
    
mapping(address => Token) public tokens;
```


### 4- Using `storage` instead of `memory` for struct/array saves gas :

When fetching data from a `storage` location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from `storage`, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new `memory` variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. 

The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct.

There are 2 instances of this issue :

File: Position.sol 

[Line 75](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L75)
```
Position memory pos = positions[posId];
```

[Line 211](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L211)
```
Position memory pos = positions[posId];
```

### 5- `storage` variable should be cached into `memory` variables instead of re-reading them :

The instances below point to the second+ access of a state variable within a function, the caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read, thus saves **100gas** for each instance.

There are 8 instances of this issue :

File: BathBuddy.sol 

[Line 124-133](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L124-L133)

In the code linked above the external call `IERC20(myBathTokenBuddy).totalSupply()` is done twice which cost more gas, the totalSupply value should be cached into a memory variable in order to save gas by avoiding multiple external calls.

[Line 197-225](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L197-L225)

In the code linked above the value of `rewardsDuration[address(rewardsToken)]` is read multiple times (4) from storage and it's value does not change so it should be cached into a memory variable in order to save gas by avoiding multiple reads from storage.

File: Position.sol 

[Line 416-418](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/poolsUtility/Position.sol#L416-L418)

In the code linked above the value of `lastPositionId` is read multiple times (2) from storage and it's value does not change so it should be cached into a memory variable in order to save gas by avoiding multiple reads from storage.

File: RubiconMarket.sol 

[Line 1045-1058](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1045-L1058)

In the code linked above the values of `offers[offerId].buy_amt` and `offers[offerId].pay_amt` are read multiple times (4) from storage and their value do not change so they should be cached into a memory variables in order to save gas by avoiding multiple reads from storage.

[Line 1085-1100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1085-L1100)

In the code linked above the value of `offers[offerId].pay_amt` is read multiple times (5) from storage and it's value does not change so it should be cached into a memory variable in order to save gas by avoiding multiple reads from storage.

[Line 1085-1100](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1085-L1100)

In the code linked above the value of `offers[offerId].buy_amt` is read multiple times (3) from storage and it's value does not change so it should be cached into a memory variable in order to save gas by avoiding multiple reads from storage.

[Line 1130-1143](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1130-L1143)

In the code linked above the value of `offers[offerId].buy_amt` is read multiple times (3) from storage and it's value does not change so it should be cached into a memory variable in order to save gas by avoiding multiple reads from storage.

[Line 1163-1176](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163-L1176)

In the code linked above the value of `offers[offerId].pay_amt` is read multiple times (3) from storage and it's value does not change so it should be cached into a memory variable in order to save gas by avoiding multiple reads from storage.


### 6- `memory` values should be emitted in events instead of `storage` ones :

The values emitted in events shouldn’t be read from storage but the existing memory values should be used instead, this will save **~100 GAS**.

There are 2 instances of this issue :

File: RubiconMarket.sol [Line 27](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L27)
```
emit LogSetOwner(owner);
```

File: BathBuddy.sol [Line 241](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L241)
```
emit RewardsDurationUpdated(rewardsDuration[token]);
```