## [N-01] TokenWithFaucet contract can't create tokens with a personalized decimal, it always creates tokens with 18 decimals
This contract is out of scope, but is used for the creation of tokens on all the tests, so, since I found out this issue on this contract I believe it might be worth it to disclose it and let know the team about the issue, so, they can be aware of it and plan accordingly for when doing future testing.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/TokenWithFaucet.sol#L33
When calling the ERC20 constructor, the _decimals are not forwarded to the ERC20 contract, thus, the token will end up being created using the default value for decimals (18)

- By default, the ERC20 constructor only receives the name and symbol, make sure to update the ERC20 contract and validate that the decimals were set correctly.

