## L-01 modifier can_offer is empty and useless 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260-L262

## L-02 DSMath should be a library instead of a contract 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L44-L45

## L-03 _bathify function - Malicious token can have the same name and symbol


## L-04 No whitelist of tokens
Readme says that they intend to allow only blue chip tokens to use in the protocol. However, there is no whitelist of tokens found anywhere in the protocol. This makes the protocol vulnerable to malicious tokens

## L-05 Have better naming conventions 
Avoid having functions with the same name to avoid confusion

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L511

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L761

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L824


## L-06 No check to see if the address arrays in claimrewards are the same length

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L113-L128


## L-07 The function NotifyRewardAmount can be manipulated with a direct deposit of reward tokens to the contract 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L216


## L-08 Function batchrequote does not check if pay.gem arrays are the same length

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L916-L933

## L-09 Oracle can be manipulated to change Max Borrow 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L304-L319


