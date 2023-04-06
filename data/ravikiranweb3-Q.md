The can_offer() virtual modifier is not enforcing any behaviour constraints in SimpleMarkat contract. This modifier is attached to offer function, but does not have any effect.

The ExpiringMarket contract is where the behaviour is implemented. 

Since modifiers are not part of the function signature, there is no reason to declare virtual and override them where not necessary. 

Modifiers are the gatekeepers and since both contracts exist in the same file, the reviewer might mistake the behaviour of the function, which will not be the case when deployed.

Suggest to use can_offer i ExpiringMarkets directly.  