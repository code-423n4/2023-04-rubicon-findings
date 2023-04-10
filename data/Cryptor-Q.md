## modifier can_offer is empty and useless 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260-L262

## DSMath should be a library instead of a contract 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L44-L45

## _bathify function - Malicious token can have the same name and symbol


## No whitelist of tokens
Readme says that they intend to allow only blue chip tokens to use in the protocol. However, there is no whitelist of tokens found anywhere in the protocol. This makes the protocol vulnerable to malicious tokens

## Have better naming conventions 

## No check to see if the address arrays in claimrewards are the same 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L113-L128