#### Gas-1 Combine multiple address mappings into single mapping with struct

Gas savings can be achieved by combining several mappings using the token, reward token info for mapping
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L52 -> Line 54 
```
mapping(address => uint256) public periodFinish; // Token specific
mapping(address => uint256) public rewardRates; // Token specific reward rates
mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
mapping(address => uint256) public lastUpdateTime; //Token specific
mapping(address => uint256) public rewardsPerTokensStored;

e.g CHANGE TO BELOW or subsets of below depend on how used 
struct TokenInfo {uint256 periodFinish, uint256 rewardRate, uint256 rewardsDuration ....etc }
mapping(address => TokenInfo) public tokensInfo
```

#### Gas-2 Use Assembly for Simple Getter and Setter functions

It may be possible to save on gas costs by using Yul assembly. Although generally this can be applied to all functions. This is not ideal as it can hinder readability, auditability of code, introduce errors or harm code as Yul Assembly will lack in-build safety checks.

However simple getter and setter functions normally have 1 to 2 line code, touch storage with not much critical error raising issues and can be rewritten in yul without impacting readability. e.g

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L574
```
Rewritten as 
 function getFeeBPS() public view returns (uint256) {
        return feeBPS;
    }

 function getFeeBPS() public view returns (uint256) {
        uint256 result;
        assembly {
            result := sload(feeBPS.slot)
        }
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628
```
Rewritten as 
 function stop() external auth {
        stopped = true;
    }

 function stop() external auth {
        assembly {
           sstore(stopped.slot,true)
        }
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466
```
function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }

function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        assembly {
          sstore(feeBPS.slot,_newFeeBPS)
        }
        return true;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1482
```
function getFeeTo() external view returns (address) {
        return feeTo;
    }

function getFeeTo() external view returns (address) {
        address res
        assembly { feeTo := sload(feeTo.slot)
        return res
    }
```

