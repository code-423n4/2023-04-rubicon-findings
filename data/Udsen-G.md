## 1. `address target` variable can be placed before `bytes args` variable in the `CallParams` struct.

This will help to `save a single storage slot`.
	
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L15	

## 2. require() can be checked after `_totalAmount assignment` to save gas in the case of reversion.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115

## 3. `BathHouseV2.createBathToken` require statement `require(underlying != address(0))` should be placed as the first check to save gas in case of reversion due to `underlying == address(0)`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L72-L75

## 4. long revert strings using more than 32bytes - BathHouseV2#L74, #L78, BathBuddy#L238, Position#L593, #L597, #L102, #L116, #L290, RubiconMarket#L376, #L381, #L726

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L74
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L78
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L238
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L593
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L597
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L102
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L116
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L290
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L376
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L381
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L726

## 5. In `Position._borrowLimit()` the variable `uint256 max` can be made outside the while loop.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L545
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L160


## 6. In `Position._savePosition` State variables (`lastPositionId`) should be cached in stack variables rather than re-reading them from storage

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L407
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L416
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L418


## 7. In `SimpleMarket.SimpleMarket`, pre calculated `Hashed` value can be directly assigned without using `keccak256()` to hash the string.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232

## 8. In `SimpleMarket.SimpleMarket.synchronized` modifier, can use 0 and 1 instead of the `false` and `true` boolean values to save gas.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

## 9. In `SimpleMarket.bump` Can get rid of `bump` function to save on the deployment gas fee, since `bump` function has no meaningful implementation.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310

## 10. In `SimpleMarket.buy` function, `spend > _offer.buy_amt` check is redundant, and can be removed to save gas.

Because if `quantity <= _offer.pay_amt` then `spend <= _offer.buy_amt` will always occur. Because of `uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt` calculation. 
Only time `spend > _offer.buy_amt` can occur is if `quantity > _offer.pay_amt`, so if it happens the earlier logic check will capture it.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L319
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L329

## 11. In `SimpleMarket.buy` function, the code snippet can be unchecked. 

        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
        offers[id].buy_amt = sub(_offer.buy_amt, spend);

Due to previous condtional checks in the code shown below, above code block can not overflow or underflow. 

            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L334-L335

## 12. In `SimpleMarket.buy` function, can replace `getRecipient(id)` with `_offer.recipient` and save gas since it replaces a `storage read` with a `memory read`. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349

## 13. In `SimpleMarket.cancel` function, the entire code snippet given below can be replaced

        _offer.owner == address(0) && msg.sender == _offer.recipient
            ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
            : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
			
Above code snippet can be replaced with the following `require statement`, since `can_cancel(id)` modifier checks the above conditinal check.

        require(_offer.pay_gem.transfer(msg.sender, _offer.pay_amt)); 
		
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L459-L461

## 14. In `SimpleMarket.calcAmountAfterFee` function, can use `amount` variable instead of `_amount` variable for calculations and `return amount` at the end. 

This will save a `MSTORE` operation.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L578

## 15. In `RubiconMarket` contract, state bool variables, `buyEnabled` and `matchingEnabled` are set to `true` at the begining of the contract. 
But it is ok to keep them in thier `default values` to save gas, since they are anyway `set in the initialize()` function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675-L676

## 16. In `RubiconMarket._matcho` function, `_best[address(t_buy_gem)][address(t_pay_gem)]` value can be cached before the `while` loop, into `stack variable` and stack variable can be used instead inside the `while` loop.

        while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
            best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1289-L1290

## 17. In `RubiconMarket._matcho` function, `t_buy_amt = sub(t_buy_amt, min(m_pay_amt, t_buy_amt));` This can be unchecked since it can not either underflow or overflow.

This will not underflow or overflow since `t_buy_amt` will be substracted by the lesser of the `m_pay_amt` or `t_buy_amt` values.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1316

## 18. In `RubiconMarket._matcho` function, redundant checks can be omitted. 

            t_buy_amt > 0 &&
            t_pay_amt > 0 &&

The above condition has already been checked by the `if (t_pay_amt == 0 || t_buy_amt == 0)` condtion in the code previously.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1325-L1326