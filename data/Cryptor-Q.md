## modifier can_offer is empty and useless 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260-L262

## DS Math should be a library instead of a contract 

## _bathify function - Malicious token can have the same name and symbol


## pay_gem and buy_gem can be equal in offer function
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L510-L525
Recommendation: Add a require statement to make sure that it cannot happen



