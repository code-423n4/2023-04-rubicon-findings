##G-01
State variables that could be declared constant.

Description
State variables that are not updated following deployment should be declared constant to save gas.

Line 682
```solidity
bool public AqueductDistributionLive;
```

Line 684
```solidity
address public AqueductAddress;
```

URL
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L682-L684

POC
```solidity
RubiconMarket.AqueductAddress (contracts/RubiconMarket.sol#684) should be constant  

RubiconMarket.AqueductDistributionLive (contracts/RubiconMarket.sol#682) should be constant  
```

Tools Used
Manual review

Recommendation
Add the constant attribute to state variables that never change.

##
##
##G-02
State variables that could be declared immutable

Description
State variables that are not updated following deployment should be declared immutable to save gas.

URL
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L44-L47

POC

Line 44
```solidity
    Comptroller public comptroller;
```
Line 45
```solidity
    PriceOracle public oracle;
```
Line 46
```solidity
    RubiconMarket public rubiconMarket;
```
Line 47
```solidity
    BathHouseV2 public bathHouseV2;
```
```solidity
Position.bathHouseV2 (contracts/utilities/poolsUtility/Position.sol#47) should be immutable  

Position.comptroller (contracts/utilities/poolsUtility/Position.sol#44) should be immutable  

Position.oracle (contracts/utilities/poolsUtility/Position.sol#45) should be immutable  

Position.rubiconMarket (contracts/utilities/poolsUtility/Position.sol#46) should be immutable  
```

Tools Used
Manual review

Recommendation
Add the immutable attribute to state variables that never change or are set only in the constructor.
