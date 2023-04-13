

# BathBuddy is not using their OZ upgradeable counterparts

The BathBuddy contract inherits non-upgradable contracts. The Openzeppelin default values make sure that no loss of funds can be caused by this. However, the BathBuddy contract is not proxy safe as mentioned in the natspec comments
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L70

Use the [upgradeble counterparts by OZ](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) instead.


similar finding to https://code4rena.com/reports/2022-02-jpyc/#l-03-contracts-are-not-using-their-oz-upgradeable-counterparts

# lastTimeRewardApplicable misses a check for initialization
The function lastTimeRewardApplicable in the BathBuddy contract does not check wether the contract hast been initialized.
For an uninitialized contract the function will return 0.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L112-L119
Add a require check like for the rewardPerToken function
```solidity
require(friendshipStarted, "I have not started a bathToken friendship");
```
# V2Migrator misses length check on bathTokensV1 and bathTokensV2
The V2Migrator contract does not check the length of the bathTokensV1 and bathTokensV2 arrays.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L31
Solution is to add a "require" check like in the migrate function
```solidity
require(bathTokensV1.length == bathTokensV2.length, "V2Migrator: bathTokensV1 and bathTokensV2 must have the same length");
```


# Code does not follow style guide 

https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout

For example events should be named starting with an upper case letter
https://docs.soliditylang.org/en/v0.8.17/style-guide.html#event-names

and not a lower case latter
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L118-L213

Further the order of layout should follow the style guide, where types are defined before 
state variables
https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout

https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L222-L242



# Contract misses address zero checks
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L516-L517

misses the following checks

```solidity
require(owner != address(0)); 
require(recipient != address(0));
```