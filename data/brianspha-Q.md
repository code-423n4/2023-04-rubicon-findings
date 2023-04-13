## DSAuth

According to the documentation on the contract this contract is used for setting the owner of any contract. It does this by providing functionality and modifiers calls.

#### [L-1] Failure to check for zero address in setOwner

There is a lack of checking if the current owner is zero address this could be dangerous in the instance where the inheriting contract does not perform the appropriate checks.

##### Risk

The risk here could lead to the contract being owned by the zero address.

##### Mitigation

```diff
-     function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
+     function setOwner(address owner_) external auth {
        if (owner_ == address(0)) revert ZeroAddress();
        //@dev check for zero address for new owner
        owner = owner_;
        emit LogSetOwner(owner);
    }
```

## BathHouseV2

#### [L-2] Failure to check for matching array lengths in claimRewards function

There is a missing check to ensure that the `buddies` and `rewardsTokens` have the same length which will result in a revert with an `Index out of bounds` message

##### Risk

The risk I foresee here given the `buddies` array being of length 100 and the `rewardsTokens` array being of length 50 would require multiple tx to complete the claims function call

##### Mitigation

```diff
-    function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {
        //Claim rewards from comptroller
        comptroller.claimComp(msg.sender);
        //Get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }
+ function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {
        if(buddies.length != rewardsTokens.length) revert SomeCustomError();
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);
        //Get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }

```

#### [L-3] Failure to check for zero address in initialize

There is a failure to check for zero address in the initialize function.

##### Risk

Since the \_comptroller and \_pAdmin is used to set the CErc20Delegator admin, failure to check for the zero address will lead to all functions that call the compcontroller reverting, subsequently, the CErc20Delegator will be owned by the zero address.

##### Mitigation

Use low-level calls or custom errors to check for zero addresses.

#### [L-4] Race condition risk with initialise function

There is a risk of a race condition with the initialization function when the deployment script is not designed to create and initialize contracts atomically, or when the factory contracts fail to create and initialize correctly.

If this process is not executed correctly, it can create an opportunity for attackers to front-run and initialize contracts with their own parameters. If this happens, it could lead to the need for a redeployment of contracts, which could result in potential denial-of-service (DoS) attacks and damage to the reputation of the contract.

There is a failure to check for zero address in the initialize function.

##### Risk

Dos attack and potential damage to reputation.

##### Mitigation

Ensure the deployment script is robust and has been tested and ensure that the contract is initialized properly.

#### [QA-1] Unnescesary assignment inside notifyRewardAmount function

There is an additional assignment made inside the notifyRewardAmount function specifically the line that has this piece of code

```
lastUpdateTime[address(rewardsToken)] = block.timestamp;
```

##### Risk

No risks at all

#### [QA-2] Inconsistent argument naming

There seems to be an inconsistency with how variables are named throughout the contract some variables are named with a leading underscore some aren't.

##### Risk

Inconsistent coding style.

##### Mitigation

I suggest a style for variable naming be picked and applied throughout the contract/s

#### [QA-3] Returning named variables

Initialising the variable inside the `returns (variable type name)` is slightly more expensive than returning the data inside of the function without assigning it to the named return variable.

##### Risk

Slightly higher gas cost per function

##### Mitigation

Avoid naming return variables, although the costs are insignificant they could add
e.g. Consider the following 2 functions which return a position given an id

## BathBuddy

#### [L-5] Failure to check for zero address in spawnBuddy

There is a failure to check for zero addresses in the spawnBuddy function.

##### Risk

Since the \_owner argument is used to set the owner of the BathBuddy contract in the instance where the argument passed in is the zero address the contract will be owned by the zero address off course a new contract can be deployed but this might not always be the case in the instance where the deployer is not aware of the error. The same applies to the other arguments newBud and_bathHouse, with the \_bathHouse argument being used to assign the bathHouse address getting rewards will fail since this address will be the zero address since the value is used by the `onlyBathHouse` modifier. The new newBud argument is also used to calculate the `rewardPerToken` should the address be zero address this function call will fail.

##### Mitigation

Use low-level calls or custom errors to check for zero addresses.

#### [L-6] Race condition risk with spawnBuddy function

There is a risk of a race condition with the spawnBuddy function when the deployment script is not designed to create and initialize contracts atomically, or when the factory contracts fail to create and initialize correctly.

If this process is not executed correctly, it can create an opportunity for attackers to front-run and initialize contracts with their own parameters. If this happens, it could lead to the need for a redeployment of contracts, which could result in potential denial-of-service (DoS) attacks and damage to the reputation of the contract.

There is a failure to check for zero address in the initialize function.

##### Risk

Dos attack and potential damage to reputation.

##### Mitigation

Ensure the deployment script is robust and has been tested and ensure that the contract is initialized properly.

## Position

A contract allows the opening of long and short positions.

#### [L-7] Failure to check for zero address in setOwner

There is a failure to check for zero address in the spawnBuddy function.

##### Risk

Failure to check for the zero address inside the constructor will lead to the rest of the function calls where the `oracle`, `rubiconMarket`, `bathHouseV2`, and `comptroller` will fail.

##### Mitigation

Use low-level calls or custom errors to check for zero addresses.

#### [L-8] Potential division by zero in the \_maxBorrow function

There is a failure to check for zero after the `getUnderlyingPrice` call is made this could cause an error since on line 317 the `_max` variable is divided by price which could be zero.

##### Risk

This will cause reverts on functions that depend on the `_maxBorrow` function

##### Mitigation

```diff

- uint256 \_price = oracle.getUnderlyingPrice(CToken(\_bathToken));
  \_max = (\_liq.mul(10 \*\* 18)).div(\_price);

+ uint256 \_price = oracle.getUnderlyingPrice(CToken(\_bathToken));
  if(\_price ==0) revert SomeCustomError();
  \_max = (\_liq.mul(10 \*\* 18)).div(\_price);
```

#### [L-9] Docstring in \_borrowLoop

There is a mismatch in the documentation of the function inside the doc string only the `_toBorrow` param is described

##### Risk

Misleading doc string

##### Mitigation

Ensure that the docstring matches the parameters and describes what the parameters are used for. The same applies in other areas of the code there is a lack of documentation of what each function and its arguments do or are used for and or if the function returns anything.

## RubiconMarket

#### [L-10] Race condition risk with initialise function

There is a risk of a race condition with the initialization function when the deployment script is not designed to create and initialize contracts atomically, or when the factory contracts fail to create and initialize correctly.

If this process is not executed correctly, it can create an opportunity for attackers to front-run and initialize contracts with their own parameters. If this happens, it could lead to the need for a redeployment of contracts, which could result in potential denial-of-service (DoS) attacks and damage to the reputation of the contract.

There is a failure to check for zero address in the initialize function.

##### Risk

Dos attack and potential damage to reputation.
