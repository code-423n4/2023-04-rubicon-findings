#  [L-01] For changing the owner use time delay and Ownable2Step
[RubiconMarket.sol/L25-L28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28)
This implementation for changing the contract owner is not faulty, but can always be approved. I would recommend for [Ownable2Step](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)(by OpenZeppelin) to be used, since it is more secure and more optimized. Also to the can be added a time delay of a day or two, so any potential breaches/changes will be noticed.

# [L-02] Function that take multiple arrays should be checked if the arrays match
## [L-02.1] 
Usually there is no need to check array, but in some functions were the full length of an array is iterated, and all of it's variables are matched together and are ment to be used in a function, for those function it is recommended to check if the arrays length match. The main reason the arrays need to be check is that the function will go thru normally and execute all of the correct matches, but when it reaches an incorrect match (token address to price, and the end user does not have the amount of the given token) the function will revert causing all other correct matches to be reverted and thus will do nothing while costing enormous amounts of gas. [RubiconMarket.sol/L917](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917)
## [L-02.2] 
As mentioned above not checking arrays lengths is costly in terms of gas. As here [V2Migrator/L30-L35](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30-L35) if the lengths are mismatched, some  V1 addresses may be assigned V2 addresses of type address(0), causing the contract to be redeployed costing gas.

# [L-03] For better protection and cheaper costs use ReentrancyGuard
Again the current implementation for the reentracy protection is working, but it is gas inefficient, since it uses bool instead of uint to check for the lock. You can implement [ReentrancyGuard.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol)(by OpenZeppelin) for more effective code.


 

