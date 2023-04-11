## 1 . Remove unused modifier :-

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L260

## 2 . Lock the pragma 

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

## 3 .  Include return parameters in NatSpec comments

Solidity contracts can use a special form of comments to provide rich documentation for functions, return variables and more. This special form is named the Ethereum Natural Language Specification Format (NatSpec).

reference :-
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1033
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1074
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L273
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L277
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L281
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L285
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L292
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L497
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L575
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L570

## 4 . Zero check must be first before conversion :-
If pay_amt and buy_amt is zero it first convert it to uint128 then zero check . But it's a good practice zero check first then conversion 

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L519-L523

Change To :-
require(buy_amt > 0);
require(pay_amt > 0);
require(uint128(pay_amt) == pay_amt);
require(uint128(buy_amt) == buy_amt);

## 5 . No Zero check after getting offerId :-
It's better to check zero after getting offerId .

code snippet:-
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1129
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1162
      