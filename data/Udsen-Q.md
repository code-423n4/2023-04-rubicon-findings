## 1. NO `address(0)` CHECKS FOR THE PROTOCOL CRITICAL ADDRESSES

    function initialize(address _comptroller, address _pAdmin) external {
        require(!initialized, "BathHouseV2 already initialized!");
        comptroller = Comptroller(_comptroller);
        admin = msg.sender;
        proxyAdmin = _pAdmin;

No `address(0)` check performed for `_comptroller` and `_pAdmin` in the `BathHouseV2.initialize()` function
`proxyAdmin` manges `BathTokens`, hence additional checkes required to verify the `proxyAdmin` address before assignment inside the `BathHouseV2.initialize` function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L34
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L36

There is one more instance of this issue
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L55-L58

## 2. USE EITHER `explicit return` OR `named return` VALUES. USING BOTH CAN BE CONFUSING TO THE BOTH DEV AND AUDITOR. 

In the `BathHouseV2.getBathTokenFromAsset` function the `explicit return` is used.

    ) public view returns (address) {
        return tokenToBathToken[asset];
		
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L47-L48

In the `BathHouseV2.whoIsBuddy` function the `implicit return` is used.

    ) external view returns (address buddy) {
        buddy = bathTokenToBuddy[bathToken];
		
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L53-L54

It is recommended to either use `explicit` or `implicit` return values for simple functions with out mixing them. This will enhance the readability of the code. 

## 3. THE IMPLEMENTATION DOES NOT MATCH THE `NATSPEC` COMMENT

It is mentioned that `BathBuddy.setRewardsDuration()` should be called before `BathBuddy.notifyRewardAmount()` is called. 

    // This must be set before notifying a new rewards program for a given token
    // Must be used before? notifyRewardAmount to set the new period
	
But the `BathBuddy.setRewardsDuration()` should be called manually by the caller before the `BathBuddy.notifyRewardAmount()` is called. But the user can forget to follow this order of function calls.
In that case even after the previous reward duration is over (`block.timestamp > periodFinish[token]`) still the new reward rates (`rewardRates[address(rewardsToken)]`) inside the `BathBuddy.notifyRewardAmount()` function, could be calculated based on the old reward duration.
Becuse the new `rewardDuration`is not set for the `token` by the caller. 

Hence it is recommended to automate the `rewardDuration` update by calling `BathBuddy.setRewardsDuration()` function inside the `BathBuddy.notifyRewardAmount()` at the begining of the function. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L230-L231
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191-L228

## 4. EMIT `events` FOR CRITICAL STATE CHANGING VARIABLES

In `RubiconMarket.setFeeBPS` function, `feeBPS` is changed to a new `feeBPS` value. `feeBPS` is an important `state` variable of the protocol.
Since it determines the trading fee of the order book exchange.
But there is not event emitted for the `setFeeBPS()` operation.

Hence it is recommended to emit `events` for critical state variable changes like above. - RubiconMarket#L1466
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1469

## 5. USE TWO STEP OWNERSHIP TRANSFER INSTEAD OF SINGLE STEP OWNERHSIP TRANSFER

In `RubiconMarket.DSAuth.setOwner()` function, the `owner` of the contract is changed to a new owner. But it uses the single step ownership transfer.

    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }

The `setOwner()` function is only accessible by the `owner` himself, due to the `auth` modifier.
Hence if the `owner` is set to a errorneous address by mistake, then the ownership of the contract will be lost and the `auth` controlled functions will not be accessible and the entire protocol could be broken as a result.

Hence it is recommended to use `Ownable2Step.sol` of `OpenZeppelin` and use the `two-step ownership transfer` pattern.

In the two step ownership process firstly the current owner makes the new owner the `pending owner` of the contract.
Then the `pending owner` completes the ownerhsip transfer by calling the `_transferOwnership()` function as `msg.sender`.
This eliminates the possibility of wrong account being selected as the new owner.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28

## 6. EXPLICITLY DECLARE THE VISIBILITY OF THE CONTSTANTS.

    uint256 constant WAD = 10 ** 18;
    uint256 constant RAY = 10 ** 27;
	
Above should be declared as follows for the improved readability and understanding of the code.

    uint256 public constant WAD = 10 ** 18;
    uint256 public constant RAY = 10 ** 27;
	
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75

There is one more instance of this issue:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224

## 7. `require` IMPLEMENTATION AND `revert` MESSAGES DO NOT SYNC

In `RubiconMarket.buy` the `require()` implementation checks whether the `spend` and `quantity` values are with in the limits of a `uint128` data type.
But the `revert` message says - `quantity is not an int` which is not what is being checked in the `require` statement.

        require(uint128(spend) == spend, "spend is not an int");
        require(uint128(quantity) == quantity, "quantity is not an int");

Hence the revert message should be changed to `value exceeds uint128`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L321-L322

## 8. `abi.encode` SHOULD BE USED INSTEAD OF `abi.encodePacked` WHEN RESULT IS USED IN `keccak256`, TO AVOID HASH COLLISIONS.

In `RubiconMarket.buy`, use `abi.encode` instead of using `abi.encodePacked`.
Since `abi.encodePacked` can create `hash collisions` when result is used in `keccak256()` to the hash value of it.

        keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
				
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L369

There are 4 more instances of this issue:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L400
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L423
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L476
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L555


## 9. CHECK FOR `address(0)` FOR THE `_offer.recipient` BEFORE TRANFERRING `buy_gem` TO IT.

In `SimpleMarket.buy` function, `_offer.recipient` recieves the `spend` amount of tokens from the `taker` of the offer.
But the `_offer.recipient` is never checked for `address(0)`. So in the worst case the `buy_gem` tokens could be sent to the `address(0)` and be burnt.

        require(
            _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
			
Hence it is recommended to check for `address(0)` for the `_offer.recipient` before transferring `buy_gem` to it

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L375

## 10. THE `emitFee` EVENT LOGS `_offer.owner` AS THE RECIEVER OF THE `mFee`, BUT IT COULD BE `_offer.recipient` AS WELL.

In `SimpleMarket.buy` function, in the event of `V1 -> V2 migration`, the `_offer.owner = address(0)`.
Hence in that case the `mFee` is sent to `_offer.recipient`.

But the emit of the event always logs `_offer.owner` as the reciever of the `mFee` which is wrong since it could be `_offer.recipient` as well.

            emit emitFee(
                bytes32(id),
                msg.sender,
                _offer.owner,
                keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
                _offer.buy_gem,
                mFee
            );

Hence additional logic should be added to the function to cache the actual reciever of the `mFee` and the use it in the `emit` to log the correct reciever of the mFee.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L368

## 11. THE `require` STATEMENT SHOULD BE CHANGED FOR BETTER CODE READABILITY

In `SimpleMarket.offer`, the `require` statement to check `address(0)` for `pay_gem` is given as below:

        require(pay_gem != ERC20(address(0)));

For better code readability it can be changed as follows:

		require(address(pay_gem) != address(0));
		
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L522

There is one more instance of this issue:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L524

## 12. WRONG IMPLEMENTATION OF `NON-REENTRANT` FOUND IN THE `RubiconMarket.offer` FUNCTION.

In `RubiconMarket.offer` function, non-reentrant` is implemented erroneously as follows:

        require(!locked, "Reentrancy attempt");
		
The Boolean `locked` value is initially set to `false`. And there is no logic inside the function to change it to `true` once the transaction enters the function.
Hence this does not protect against the reentrancy since the `!locked` will	always result in `true`.

If this condition is used to check whether contract is locked, proper implementation should be in place to change the value of `locked`	variable. But there is no such implementation found with in the contract.

Hence it is recommended to use the `synchronized` modifier instead, to protect against reentrancy, or else after the `require(!locked, "Reentrancy attempt");` statement set `locked = true`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L834

There are 4 more instances found of this issue:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L874
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L938
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1034

## 13. `matchingEnabled` BOOLEAN VALUE IS ALWAY `true`.

In `RubiconMarket.cancel` function, `matchingEnabled` is always `true` and there is no implementation within the code to change the value of it. 
Hence the conditions which uses the value `matchingEnabled` will result in `true`.
 
        matchingEnabled = true;
 
        if (matchingEnabled) {
		
Hence there is no point in using `matchingEnabled` value for conditional checks if the `matchingEnabled` variable is not changing with in the implementation of the contract.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L875-L881


## 14. `delete` KEYWORD SHOULD BE USED INTEAD OF ASSIGNING THE VALUE TO `0` WHEN DELETING STATE VARIABLES IN THE CONTACT.

In `RubiconMarket._hide` function, uses `_near[id] = 0` to delete an `offer` from an `unsorted` offers list.

            _near[id] = 0;

But it is recommended to use the `delete _near[id]` instead of `_near[id] = 0` to delete the state variable value. So it should be corrected as below:

            delete _near[id];

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449

There is one more instance of this issue:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462

## 15. UNBOUNDED LOOPS CAN TRIGGER `block gas limit exceed` ERRORS IN `RubiconMarket` CONTRACT.

In `RubiconMarket.batchOffer` function, unbounded loops can trigger block gas limit exceed error. 
The batch transactions uses unbounded for loops inside the `RubiconMarket` contract.

Most of the transactions with in these `batch` functions are computationally extensive.
Hence it is difficult to estimate the transaction `gas limit` as well.

        for (uint i = 0; i < payAmts.length; i++) {
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }

Hence it is recommended to put inplace a limit for each batch, for which these batch transactions are used.
So the number of iterations in the `for` loops will be limited.

Hence the batch transaction will not exceed the block gas limit.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L899-L906

There are two more instances of this issue:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L924-L932 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L911-L913

## 16. `payable` KEYWORD IS MISSING IN THE FUNCTION.

The `FeeWrapper._rubicallPayable()` and `FeeWrapper._chargeFeePayable()` functions accept `Eth` via `msg.value`.
But these functions are not defined with the `payable` keyword to allow them to recive `Eth`.

Hence implement `payable` keyword in the function signatures of those two functions.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L108


## 17. WHEN USING MULTIPLE `arrays` WITHIN A `for` LOOP, MAKE SURE TO CHECK THAT THIER ARRAY LENGHTS ARE EQUAL.

`BathHouseV2.claimRewards()` function uses `for` loop to get the `rewards` for the `buddies` array for each of the reward tokens given in the `rewardTokens` array.
But the array lenghts of `buddies` and `rewardsTokens` are never checked for equality.

    function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);
        // get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }

Hence if one array lenght is different from the other, this will break the functionality of the `for` loop thus giving unexpected results.

Check the array lengths as below:

     require(buddies.length == rewards.length, "Array length mismatch");

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115-L128

There is 1 more instance of this issue:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933

## 18. CHECK FOR `contract existence` WHEN SENDING `Eth` TO AN EXTERNAL CONTRACT ADDRESS.

In the `FeeWrapper._rubicallPayable()` function, `Eth` is sent to the `_params.target` address by calling the low level `call` function.
But the contract existence of `_params.target` is never checked. If there is no contract at the `_params.target` the low level `call` function will return `true` and the sent `Eth` will be lost.

Hence it is recommended to check the contract existence of the `_params.target` address.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L82

## 19. IN `Position._leverageCheck` FUNCTION, THE `_leverageMax` IS HARDCODED TO `3`

In Position._leverageCheck the `_leverageMax` can not be changed and is hardcoded to `3`, as shown below:

        uint256 _leverageMax = WAD.mul(3);

So in the future if there is a requirement to increase the `max leverage` times, the `Position` contract will have to redeployed with the new `max leverage` value.
This could be difficult change to execute.

Hence the it is recommended to keep the `max leverage` value as a `settable` value. And new function should be implemented to set the `max leverage` which is only accessible by the `owner`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L588

## 20. TYPOs FOUND IN THE FOLLOWING `NATSPEC` COMMENTS

Typos found in the `natspec` comments have been highlighted below: 

@notice `defaull` call to the router functions
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L59
	
// amount of feeToken `from which fee should be charged`
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L22 

// `guy` who manages BathTokens
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L16

"createBathToken: BATHTOKEN WITH THIS ERC20 EXIST `ALDREADY`" 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L70

// amount of `basthTokenAsset` in the moment of execution
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L28

/// @dev save initial balance of asset to calculate `then` amount to borrow
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L139

/// @notice Modifier that `insures` an order exists and is properly in the orderbook - RubiconMarket#L244
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L244
