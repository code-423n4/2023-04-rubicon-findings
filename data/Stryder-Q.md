# [N-01] Null Address check 

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25


Null Address Check not added 

Consider adding null value check for (0x0) address when you are assigning the value to address state variables 

consider adding the check : 

    function setOwner(address owner_) external auth {
    require(owner_ != address(0))
        owner = owner_;
        emit LogSetOwner(owner);
    }

# [N-02] No sting added in the require statements 

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L246

Require statement must be followed by a string that explains the reason for the condition failing. 

35 Other instances :
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L252-L256
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L265
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L459-L461
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L488
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L518-L525
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L538
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L565
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L581 
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L598
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L604
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L605 
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L612-L616
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L753 
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L757 
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L938
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L941-L944
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L1034
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L1075
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol?#L1226

# [N-03] Events are not indexed 

Link: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L23

Consider making the events indexed, at least one of the parameters must be indexed as indexed events are much easier to find than for the off-chain tools. If gas cost is not the issue, ideally all the 3 parameters 
that can be indexed should be indexed in every event with more than 2 parameters. 

7 other instances: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L24
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L261
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L265
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L266
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L50
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L51
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L52

# [N-04] State changes should not be done inside a modifier 

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=modifier%20synchronized()%20%7B,%7D

File: RubiconMarket.sol 

It's not advised to make state changes in the contract using the modifier, the best practice is to use the function itself for the state changes. In this case, the locked variable is being changed value. 
Modifiers are only there for checks, the state changes should be restricted to functions 
 