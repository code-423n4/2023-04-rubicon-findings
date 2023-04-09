**RubiconMarket.sol**
- L15 - DSAuthEvents contract is created, just for one event, this is too excessive. The event can be created within the inherited contract directly.

- L107/140/163/187/386/409/464/542/851/935/1346 - There is commented code, this makes the contract confusing to understand, therefore it should be removed.

- L578/855/871/937/955/965/974/985/993/998/1010/1016/1020/1028/1069/1157 - NatSpec is incomplete.

- L35 - A better implementation of the validation is directly: "return src == owner;"

- L219/227/230/234 - In the SimpleMarket contract, multiple variables are created in storage but they follow different standards, some follow camel case and others snake case. This should follow a single standard.

- Function ordering does not follow the Solidity style guide
According to the Solidity style guide, functions should be laid out in the following order :constructor(), receive(), fallback(), external, public, internal, private, but the cases below do not follow this pattern.

- L30 - The auth() modifier has a slightly confusing name, since it doesn't refer to what it validates. Perhaps a name like onlyOwner makes the role clearer. This would make the code easier to understand.


**utilities/poolsUtility/Position.sol**
- Function ordering does not follow the Solidity style guide
According to the Solidity style guide, functions should be laid out in the following order :constructor(), receive(), fallback(), external, public, internal, private, but the cases below do not follow this pattern.

- L54 - In the constructor, multiple variables are set that cannot be modified, therefore it would be beneficial to validate that it is != address(0) before setting it, since a DoS would be performed on multiple functions. Perhaps it would be beneficial to create setter functions that can only be modified by onlyOwner.

- L329 - Loss of precision due to rounding - Add scalars so roundings are negligible.
Case: uint256 _interest = ((_borrowedAmount).mul(borrowRate(_bathToken).mul(_blockDelta))).div(10 ** 18);


**periphery/BathBuddy.sol**
- L71 - It is somewhat confusing that the initialize is called spawnBuddy, this should be changed to follow an existing standard.

- L104 - The onlyBathHouse modifier is created, but it is only used once, therefore it could be directly used in the function that is needed.

- Function ordering does not follow the Solidity style guide
According to the Solidity style guide, functions should be laid out in the following order :constructor(), receive(), fallback(), external, public, internal, private, but the cases below do not follow this pattern.

- L146 - Loss of precision due to rounding - Add scalars so roundings are negligible.
Case: IERC20(myBathTokenBuddy).balanceOf(account).mul(rewardPerToken(token).sub(userRewardsPerTokenPaid[token][account])).div(1e18).add(tokenRewards[token][account]);


**utilities/FeeWrapper.sol**
- L28/41 - A division is performed by the input feeType, but it is never checked if it is != 0, this should be validated with a require.


**BathHouseV2.sol**
- Function ordering does not follow the Solidity style guide
According to the Solidity style guide, functions should be laid out in the following order :constructor(), receive(), fallback(), external, public, internal, private, but the cases below do not follow this pattern.

- L12/26/32/137 - NatSpec is incomplete.

- L115/122/124 - A "for" loop is performed going through two arrays that are not validated if they have the same length, this is an error because it can generate exceptions or inconsistencies in the code. A require should be added, validating that.

 
**V2Migrator.sol**
- L8/17/38 - NatSpec is incomplete.

- L30/31/33 - A "for" loop is performed going through two arrays that are not validated if they have the same length, this is an error because it can generate exceptions or inconsistencies in the code. A require should be added, validating that.

- L59/63 - When the bathTokenV2 from address(this) is transferred to the msg.sender, it validates that IERC20(bathTokenV2).balanceOf(address(this)) == 0, but apart from this, another validation should be added that does : require(IERC20(bathTokenV2).balanceOf(msg.sender) >= amountWithdrawn)
to leave 100% assured that the money was withdrawn and that the msg.sender received the money.



