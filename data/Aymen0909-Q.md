# QA Report

## Summary

|               | Issue         | Risk     | Instances     |
| :-------------: |:-------------|:-------------:|:-------------:|
| 1 | fees should have an upper bound | Low | 2 |
| 2 | Use two step ownership transfer in `DSAuth` | Low | 1 |
| 3 | Can not change `admin` in `BathHouseV2` & `owner` in `BathBuddy` | Low | 1 |
| 4 | Related data should be grouped in a struct | NC | 7 |

## Findings

### 1- fees should have an upper bound :

#### Risk : Low

The RubiconMarket contract uses two fees : `makerFee` and `feeBPS`, both are in basis point of 100000 (meaning that 100000≈100%) but in the fees setter functions : `setMakerFee` and `setFeeBPS` there is no check to ensure that the fees are not above 100000, which means that the owner can set one of those fees (or both) to a large value (greater than 100000) this will cause a lot of problems in the protocol working as some functions like `calcAmountAfterFee` will be blocked due to an undeflow error which will result in the DOS of the buy & sell functions.

#### Mitigation
Add an upper bound value for both `makerFee` and `feeBPS` in their respective setter functions.


### 2- Use two step ownership transfer in `DSAuth` :

#### Risk : Low

In the RubiconMarket contract, `DSAuth` is used to manage the owner control access and the function `setOwner` is used to set a new owner, as the owner is responsible of setting critical protocol parameters it's safer to use a two-step ownership transfer instead to avoid any an unintentional owner transfer and this will also remove the risk of burning the ownership by transferring to address(0) (which is possible as there is no check on the new onwer address in `setOwner` function) .

#### Mitigation
You should considere using two-step ownership transfer for `DSAuth` and a non zero address check should be added in the `setOwner` function.


### 3- Can not change `admin` in `BathHouseV2` & `owner` in `BathBuddy` :

#### Risk : Low

The `admin` in `BathHouseV2` is also used as the `owner` of the `BathBuddy` contract, there is no way to change the `admin` address and thus the `owner` address can't be changed either, even though this does not have a negative impact on the protocol it's more convenient to add setter functions to enable the `admin` and `owner` transfer to other addresses in the future.

#### Mitigation
Consider adding a way to change the `admin` in `BathHouseV2` and the `owner` in `BathBuddy`.

### 4- Related data should be grouped in a struct :

When there are mappings that use the same key value, having separate fields is error prone, for instance in case of deletion or with future new fields.

#### Risk : Non critical

#### Proof of Concept

Instances include:

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

#### Mitigation

Group the related data in a struct and use one mapping :

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
```

And it would be used as a state variable :

```
mapping(address => Token) public tokensInfo;
```