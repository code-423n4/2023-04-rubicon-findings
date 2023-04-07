Protocol won't work with tokens that do not return a `bool` on transfer/transferFrom. This issue is marked as low / non-critical, due to the fact that the protocol will be deployed on L2 ETH EVMs, however a potential deployment on Ethereum as example will block the protocol from working with USDT. 

Other similar tokens to USDT which do not return a `bool` on transfer/transferFrom can't be supported.

Examples:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L339 --> This require will fail on `buy()` function

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L538 --> This require will fail on `offer()` function call