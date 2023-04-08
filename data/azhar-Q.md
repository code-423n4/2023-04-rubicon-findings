##Vulnerable to Reentrancy Attack

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L118

###Solution

This re-entracny issue can be mitigated by tightly following the “Check-effects-interactions” pattern and/or using OpenZeppelin Contract’s ReentrancyGuard contract. 

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol#L43-L54

Make sure all the public and external functions must have that reentrancy guard modifier.