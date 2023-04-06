a) can_offer() virtual modifier
===============================

The can_offer() virtual modifier is not enforcing any behaviour constraints in SimpleMarkat contract. This modifier is attached to offer function, but does not have any effect.

The ExpiringMarket contract is where the behaviour is implemented. 

Since modifiers are not part of the function signature, there is no reason to declare virtual and override them where not necessary. 

Modifiers are the gatekeepers and since both contracts exist in the same file, the reviewer might mistake the behaviour of the function, which will not be the case when deployed.

Suggest to use can_offer i ExpiringMarkets directly.  

b) Multiple declarations of IBathBuddy interface
================================================

IBathBuddy interfaces was declared twice in the project.

1) under interfaces in IBathBuddy.sol
2) under periphery in BathBuddy.sol

There should be a single declaration as this will be extended in future


c) modifier onlyBuddy() in BathBuddy contract
=============================================
1) The modifier is never used. 
2) Also the condition, msg.sender != address(0) does not make any sense. No one has keys to address(0) to become a sender. The attention here should be on what the developer is intending to do and make a safe assumptions around that. 