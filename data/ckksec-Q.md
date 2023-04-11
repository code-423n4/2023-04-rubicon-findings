## Impact
The [FeeWrapper](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol) contract allows anyone to arbitrarily call `rubicall()`. If any contract or user has approved the FeeWrapper to spend more tokens than needed to pay fees or by accident leaves tokens, it allows anyone else to transfer the remaining amount out.

## Proof of Concept
The `rubicall()` function allows any call. Following scenario can happen:
Example swap contract (ES) interacts with the `FeeWrapper`. UserA now swaps tokens via ES and ES calls `FeeWrapper` to handle the fees. The fee is 100 tokens but due to a wrong calculation in ES, 110 tokens are actually forwarded. Since the `FeeWrapper` now still holds 10 tokens, anyone now can call `rubicall()` with initiating a `transfer()` call on the target token. An attacker would therefore make sure that `FeeParams` parameters basically return zero and not charging a fee and make the "exploit" via the `CallParams` parameters by setting `selector` to `transfer()`, `args` to the attacker address and amount and the target to the tokens that are remaining on `FeeWrapper`. [Despite the allowance being reduced](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L105), it will still work since it's a simple transfer initiated by the `FeeWrapper` itself. An other attack vector would be if `msg.sender` set an approval higher than the fees. This could be abused via a `transferFrom()` This case would also be fixed with the check below since the fee payer is always msg.sender.

## Tools Used
Manual research

## Recommended Mitigation Steps
I recommend to add a balance check and an approval check that checks that the `FeeWrapper` is no longer allowed to move tokens, e.g. above [line 52](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L52). It could look like this:
```solidity
require(IERC20(params.feeParams.feeToken).allowance(msg.sender, address(this)) == 0, "Token approval higher than expected");
require(IERC20(params.feeParams.feeToken).balanceOf(address(this)) == 0, "Token balance higher than expected");
```
It seems like the ETH transfer is properly secured as it [requires](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L115) the amount to be exactly as the required amount and then [forwards](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L82) the remaining amount.

I am aware about the fact that projects themself have the responsibility to correctly make such checks and decided to therefore report it as low/non-critical. However, the `FeeWrapper` is anyway not supposed to have any tokens after a call so if a check like suggested breaks the interacting contract, it ultimately might secure funds anyway. Also due to allowance settings there is another attack vector easily possible.