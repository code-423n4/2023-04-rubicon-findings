# Audit Report

## Summary

### Low Risk Issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | The mutex variable is not used correctly | 1 | 
| [L&#x2011;02] | Critical ```onlyOwner``` privileges can cause damage of the project in a possible  | 2 | 
| [L&#x2011;03] | The Contract Should Approve(0) first | 3 | 
| [L&#x2011;04] | The returned value of ```approve()``` is not checked  | 4 | 
| [L&#x2011;05] | Unbounded loop length potentially leads to DOS | 5 | 
| [L&#x2011;06] | Shadowed local variable  | 6 | 
| [L&#x2011;07] | Discouraged method is used | 7 | 
| [L&#x2011;08] | The modifier does not do any checks  | 8 | 



### Non-critical Issues
| |Issue|Instances|
|-|:-|:-:|
| [N&#x2011;01] | Named return variable is not used in the function | 1 | 
| [N&#x2011;02] | Prefer named imports over just importing files | 2 | 
| [N&#x2011;03] | Interfaces should be in a seperate file | 3 | 
| [N&#x2011;04] | Unnecessary usage of SafeMath | 4 | 
| [N&#x2011;05] | Unused import | 5 | 
| [N&#x2011;06] | Check the balance of the contract before transfering | 6 | 
| [N&#x2011;07] | Unnecessary address check | 7 | 
| [N&#x2011;08] | Unused events | 8 | 


### [L&#x2011;01]  The mutex variable is not used correctly

#### Impact
Although there's already ```synchronized()``` modifier which check reentrancy with a mutex variable, it has not been used in multiple important functions. Instead there's a require statement which only checks if ```locked``` is not false, which is not enought to prevent reentrancy.

```solidity
File: /contracts/RubiconMarket.sol

function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        bool matching, //match "close enough" orders?
        address owner, // owner of the offer
        address recipient // recipient of the offer's fill
    ) public can_offer returns (uint256) {
        require(!locked, "Reentrancy attempt");
        require(_dust[address(pay_gem)] <= pay_amt);

        /// @dev currently matching is perma-enabled
        // if (matchingEnabled) {
        return
            _matcho(
                pay_amt,
                pay_gem,
                buy_amt,
                buy_gem,
                pos,
                matching,
                owner,
                recipient
            );
        // }
        // return super.offer(pay_amt, pay_gem, buy_amt, buy_gem);
    }
```

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L834
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L874
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L938
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1034
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1075



#### Proof of Concept
If the payment token would be an ERC777 token (or another token that has callbacks), then an reentrancy attack could be tried.
But since there are no external calls to the functions mentioned, the funds are not at risk.


#### Recommended Mitigation Steps
use the ```synchronized()``` modifier in the functions mentioned.

### [L&#x2011;02]  Critical ```onlyOwner``` privileges can cause damage of the project in a possible privateKey exploit
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L242

Typically, the contract’s onlyOwner is the account that deploys the contract. As a result, the onlyOwner is able to perform certain privileged activities.

However, onlyOwner privileges are numerous and there is no timelock structure in the process of using these privileges.

The onlyOwner is assumed to be an EOA, since the documents do not provide information on whether the onlyOwner will be a multisign structure.

In parallel with the private key thefts of the project onlyOwners, which have increased recently, this vulnerability has been stated as Medium.

Similar vulnerability;

Private keys stolen:

Hackers have stolen cryptocurrency worth around €552 million from a blockchain project linked to the popular online game Axie Infinity, in one of the largest cryptocurrency heists on record. Security issue: PrivateKey of the project officer was stolen: https://www.euronews.com/next/2022/03/30/blockchain-network-ronin-hit-by-552-million-crypto-heist

#### Recommended Mitigation Steps
1. A timelock contract should be added to use onlyOwner privileges. In this way, users can be warned in case of a possible security weakness.
2. onlyOwner can be a Multisign wallet and this part is specified in the documentation.

### [L&#x2011;03] The Contract Should Approve(0) first

#### Impact
Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L53
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L105
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L297
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L358
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L465
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L500

#### Recommended Mitigation Steps
Approve with a zero amount first before setting the actual amount.

```solidity
IERC20(token).approve(address(operator), 0);
IERC20(token).approve(address(operator), amount);
```

### [L&#x2011;04] The returned value of ```approve()``` is not checked

#### Impact
Some tokens do not revert when ```approve()``` fails, instead they return a ```false``` value.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L53
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L105
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L297
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L358
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L465
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L500

#### Recommended Mitigation Steps
Consider adding a check to see if ```approve()``` ever returns a false value for failure. 

### [L&#x2011;05] Unbounded loop length potentially leads to DOS

## Impact
Loops that do not have a fixed number of iterations, for example, loops that depend on storage values, have to be used carefully: Due to the block gas limit, transactions can only consume a certain amount of gas. Either explicitly or just due to normal operation, the number of iterations in a loop can grow beyond the block gas limit which can cause the complete contract to be stalled at a certain point.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L158


### [L&#x2011;06] Shadowed local variable

### Impact
```RubiconMarket``` contract is inherited from ```DSAuth``` contract which has an ```owner``` variable. When we call ```offer()``` passing ```owner``` argument, it can get complicated. Although they do not pose any immediate risk to the contract, incorrect usage of the variables is possible and can cause serious issues if the developer does not pay close attention. 
[swcregistry](https://swcregistry.io/docs/SWC-119)

```solidity
    offer(
        pay_amt,
        pay_gem,
        buy_amt,
        buy_gem,
        pos,
        true,
        owner,
        recipient
    );
```

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L819
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L847

#### Recommended Mitigation Steps
Consider using a different naming when calling ```offer()```.

### [L&#x2011;07] Discouraged method is used

### Impact 
according to [OasisDex](https://oasisdex.com/docs/references/smart-contract#offeruint-erc20-uint-erc20), using ```offer(uint, ERC20, uint, ERC20)``` is not recommended because this offer will not end up in a sorted list and might be lost.
#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L900

#### Recommended Mitigation Steps
Consider using another overloading of ```offer()```.

### [L&#x2011;08]  The modifier does not do any checks

#### Impact
The can_offer modifier does not check for any failures and offer() is using this modifier.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260


```solidity
File: /contracts/RubiconMarket.sol

260:          modifier can_offer() virtual {

```
#### Recommended Mitigation Steps
Consider either writting some checks or removing it.

### [N&#x2011;01] Named return variable is not used in the function

#### Impact
The named return variables are not used anywhere in the functions, which can be confusing.
#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L280
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L454
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L496
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L873

#### Recommended Mitigation Steps
It's better to either use the named variable or remove it.


### [N&#x2011;02] Prefer named imports over just importing files

#### Impact
In every file there are file imports. Consider naming imports for better readability.


### [N&#x2011;03] Interfaces should be in a seperate file

#### Impact
Move the interfaces to seperate files for better readability.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L9
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L8

### [N&#x2011;04] Unnecessary usage of SafeMath

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L39
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L18

#### Impact
No need to use SafeMath for ^0.8.0 pragma versions because the overflows and underflows are already taken care of.

### [N&#x2011;05] Unused import

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L4

#### Recommended Mitigation Steps
Consider either removing or using it.

### [N&#x2011;06] Check the balance of the contract before transfering
The require statement is done after transfering. Move the `require` before the trasnfer.
#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L63

### [N&#x2011;07] Unnecessary address check

```solidity
    modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) && // @audit Unnecessary
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }
```
If ```msg.sender == myBathTokenBuddy``` then it is definitely not ```address(0)```.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L97

### [N&#x2011;08] Unused events

These events are not used. Consider removing them.

#### Findings
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L262
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L263