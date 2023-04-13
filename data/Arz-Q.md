## Issue 1 - User gets less tokens in V2Migrator.sol than he deposits

Contract: V2Migrator.sol
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L55

When the user withdraws tokens from bathTokenV1 he pays a fee. The withdraw function then returns the (deposit amount - fee) which is then set as a variable amountWithdrawn in migrate(). Then the new bathTokenV2 are minted to the contract, but the amount that is minted is the amountWithdrawn so the user gets less than he deposits.

## Recommended Mitigation Steps
Mint the amount that he deposits

## Issue 2 - Front-Running Attacks on Initializable Contracts

Contracts: V2Migrator.sol, RubiconMarket.sol, BathHouseV2.sol

We have multiple contracts where the initializer sets important variables like admin,comptroller,proxyAdmin etc. When deploying a proxy, the implementation contract is deployed and then it calls the initializer to set important variables 

An attacker can create a transaction that calls the initializer with malicious values and front-run the transaction from the proxy so the attacker’s malicious values will be set and the transaction from the proxy will fail.

## Impact

By setting malicious values for the important variables in the initializer, the attacker could potentially gain control over the proxy contract and the underlying implementation contract


## Issue 3 - safeERC20 isnt used, Some tokens do not revert if we try to transfer them using regular transfer functions

Some tokens do not revert if we try to transfer them using regular transfer functions and the transactions fails.  V2Migrator.sol or RubiconMarket.sol doesnt check this return value, so on tx failure this can be an issue. SafeERC20 ensures the safety of normal ERC20 function calls by checking the boolean return values of ERC20 operations and revert the transaction if they fail, at the same time allowing you to support some non-standard ERC20 tokens that don’t have boolean return values


## Recommended Mitigation Steps

Consider using safeERC20


## Issue 4 - Fake tokens can be created in RubiconMarket.sol

Contract: RubiconMarket.sol

Anyone can create a new offer, however there is no logic to check that the token contract supplied is a valid ERC20 token, so a malicious user can create a fake token with fake transfer function that doesnt even transfer the tokens into the contract and only returns true when the function is called. There are no direct assets in risk unless some users get tricked into buying that fake token.

## Impact

The users of the Rubicon may be vulnerable to financial loss if they are tricked into buying a fake token that has no actual value

## Recommended Mitigation Steps

Verification system on the website




