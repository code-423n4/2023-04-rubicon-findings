# QA Report:
## Function setFeeBPS: _feeBPS is a very sensitive parameter and should be validated, it can also be used as a rugpull vector.
RubiconMarket.sol#L1466
Add `require(_newFeeBPS <= 100, "Incorrect fees");`
## Match the length of all entries on `batchRequote`
   To avoid error when Updating outstanding offers to new offers check the length of the argument arrays to be equal.
Instance:
```solidity
RubiconMarket.sol#L917
function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
```
check the length of the arrays to match the length of the ids array.

## Remove/re-implement obsolete functions
in `RubiconMarket.sol` some function should act like a toggle but the returns a constant value.
e.g The `stop` function when used cannot be unstopped.
Instances:
```solidity
RubiconMarket.sol#L628 function stop() external auth {
RubiconMarket.sol#L620 function isClosed() public pure returns (bool closed) {
```
## Unused event may be unused code or indicative of missed emit/logic
   consider removing unused events
   ```solidity
   BathBuddy.sol#L262 event Staked(address indexed user, uint256 amount);
   BathBuddy.sol#L263 event Withdrawn(address indexed user, uint256 amount);
   BathBuddy.sol#L266 event Recovered(address token, uint256 amount);
   ```
## using old solidity version
   solidity version too old consider updating to version 0.8.17 as in position.sol contract.
   ```solidity
   BathBuddy.sol#L2  pragma solidity ^0.8.0;
   RubiconMarket#L2 pragma solidity ^0.8.9;
```
## Contract initializes safe library but doesn't use safe functions.
   if safe versions of IERC20 was meant to be used consider implementing them in the contract not still using the unsafe version.
   `FeeWrapper#L10 using SafeERC20 for IERC20;`
## Approval from non-zero would revert if token is USDT.
   Also `safeApprove` would [revert](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/utils/SafeERC20.sol#L50) from non-zero allowance.
   Approve zero first.
   Instances:
   ```solidity
   Position.sol#L297 IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
   Position.sol#L358 IERC20(_token).safeApprove(_bathToken, _amount);
   Position.sol#L465 IERC20(_quote).approve(address(rubiconMarket), _maxFill);
   Position.sol#L500 IERC20(_asset).approve(
   ```

## Improper reward balance checks can make some users unable to withdraw their rewards.
Affected lines:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L216-L221
Impact:
The contract only checks if balanceOf rewardsToken is greater than or equal to the future rewards. The current checks are not sufficient to make sure the contract has enough amount of rewardsToken.
```solidity
uint256 balance = rewardsToken.balanceOf(address(this));
        require(
            rewardRates[address(rewardsToken)] <=
                balance.div(rewardsDuration[address(rewardsToken)]),
            "Provided reward too high"
        );
```
Recommendation:
consider implementing the solution provided [here](https://github.com/code-423n4/2022-02-concur-findings/issues/209)

## Lack of event emission after sensitive actions
Instances:
```
RubiconMarket.sol#L1466 function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
RubiconMarket.sol#L1476 function setFeeTo(address newFeeTo) external auth returns (bool) {
RubiconMarket.sol#L1471 function setMakerFee(uint256 _newMakerFee) external auth returns (bool)
```