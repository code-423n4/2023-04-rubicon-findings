No Storage Gap for Upgradeable Contracts


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L593-L594

```solidity

contract ExpiringMarket is DSAuth, SimpleMarket {
    bool public stopped;
    
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L674-L684

```solidity

contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
    bool public buyEnabled = true; //buy enabled TODO: review this decision!
    bool public matchingEnabled = true; //true: enable matching,

    /// @dev Below is variable to allow for a proxy-friendly constructor
    bool public initialized;

    /// @dev unused deprecated variable for applying a token distribution on top of a trade
    bool public AqueductDistributionLive;
    /// @dev unused deprecated variable for applying a token distribution of this token on top of a trade
    address public AqueductAddress;
    
```


For upgradeable contracts, there must be storage gap to "allow developers to freely add new state variables in the future without compromising the storage compatibility with existing deployments". Otherwise it may be very difficult to write new implementation code. Without storage gap, the variable in child contract might be overwritten by the upgraded base contract if new variables are added to the base contract. This could have unintended and very serious consequences to the child contracts.

Refer to the bottom part of this article: https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable

As an example, the ExpiringMarket contract inherits SimpleMarket, and the SimpleMarket contract does not contain any storage gap. If in a future upgrade, an additional variable is added to the SimpleMarket contract, that new variable will overwrite the storage slot of the stopped variable in the ExpiringMarket contract, causing unintended consequences.

Similarly, the ExpiringMarket does not contain any storage gap either, and the RubiconMarket contract inherits ExpiringMarket. If a new variable is added to the ExpiringMarket contract in an upgrade, that variable will overwrite the buyEnabled variable in ExpiringMarket contract.


Recommended Mitigation Steps

Recommend adding appropriate storage gap at the end of upgradeable contracts such as the below. 
Please reference OpenZeppelin upgradeable contract templates.

```solidity
uint256[50] private __gap;
```



