Two missing checks:

Missing check  for array lengths in function batchRequote of contract RubiconMarket.
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L917-L933

mitigation steps which are required :
Put this check in the function above
require(
      payAmts.length == payGems.length &&
        payAmts.length == buyAmts.length &&
        payAmts.length == buyGems.length,
      "Array lengths do not match"
    );

Missing check for safeTransfer in getReward function in BathBuddy.sol contract
https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/periphery/BathBuddy.sol#L168-L185
since at line 181 //tokenRewards[address(rewardsToken)][holderRecipient] = 0;
we are setting the rewards to zero so if unfortunately the call fails then the user will end up having no rewards.