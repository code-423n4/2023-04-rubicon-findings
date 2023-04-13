[Issue 1]

initialize functions and spawnBuddy function can be front-run. The functions that initialize important contract state can be called by anyone.

Occurences:

RubiconMarket.initialize

```solidity
function initialize(address _feeTo) public {
        require(!initialized, "contract is already initialized");
        require(_feeTo != address(0));

        /// @notice The market fee recipient
        feeTo = _feeTo;

        owner = msg.sender;
        emit LogSetOwner(msg.sender);

        /// @notice The starting fee on taker trades in basis points
        feeBPS = 1;

        initialized = true;
        matchingEnabled = true;
        buyEnabled = true;
    }
```

BathHouseV2.initialize

```solidity
   function initialize(address _comptroller, address _pAdmin) external { // @audit everyone can call this function
        require(!initialized, "BathHouseV2 already initialized!");
        comptroller = Comptroller(_comptroller);
        admin = msg.sender;
        proxyAdmin = _pAdmin;

        initialized = true;
    }
```

BathBuddy.spawnBuddy

```solidity
    function spawnBuddy(
        address _owner,
        address newBud,
        address _bathHouse
    ) external {
        require(!friendshipStarted, "I already have a buddy!");
        owner = _owner;
        myBathTokenBuddy = newBud;
        bathHouse = _bathHouse;

        // Note, rewards duration must be set by admin

        // Constructor pattern
        friendshipStarted = true;
    }
```

The attacker can call the functions before the legitimate deployer, hoping that the victim continues to use the same contract. In the best case for the victim, they notice it and have to redeploy their contract costing gas.

Recommend using the constructor to initialize non-proxied contracts. For initializing proxy contracts, recommend deploying contracts using a factory contract that immediately calls function after deployment, or make sure to call it immediately after deployment and verify the transaction succeeded.


[Issue 2]

See FeeWrapper.sol,

Feetype maybe 0 which will always cause the function always revert at this line of code
```fees[i] = (tokenAmounts[i] * feeValue) / feeType;```. 
To fix this, add a require function to ensure the value will be never be zero (See below).

```
    function calculateFee(
        uint256[] memory tokenAmounts,
        uint256 feeType,
        uint256 feeValue
    )
        external
        view
        returns (uint256[] memory amountsWithFee, uint256[] memory fees)
    {
        +++ require(feeType >0, 'Fee Type should not be 0');
        amountsWithFee = new uint256[](tokenAmounts.length);
        fees = new uint256[](tokenAmounts.length);

        for (uint256 i = 0; i < tokenAmounts.length; ++i) {
            fees[i] = (tokenAmounts[i] * feeValue) / feeType;
            amountsWithFee[i] = tokenAmounts[i] - fees[i];
        }
    }
```


[Issue 3]

SafeMath library is not required in version 0.8.
Overflow and underflow problems which could happen in uint arithmetic operations in previous versions is automatically checked in version 0.8.

Hence, 

add keyword should be replaced by +
sub keyword should be replaced by -
mod keyword should be replaced by *
div keyword should be replaced by /

For eg, 

```solidity
rewardsPerTokensStored[token].add(
lastTimeRewardApplicable(token)
.sub(lastUpdateTime[token])
.mul(rewardRates[token])
.mul(1e18)
.div(IERC20(myBathTokenBuddy).totalSupply())
);
```

can be rewritten as

```solidity
rewardsPerTokensStored[token] + (
lastTimeRewardApplicable(token)
- lastUpdateTime[token]
* rewardRates[token]
* 1e18
/ IERC20(myBathTokenBuddy).totalSupply())
);
```

Also, SafeMath should not be imported

in BathBuddy.sol and Position.sol


[Issue 4]

In position.sol, 

buyAllAmountWithLeverage and sellAllAmountWithLeverage are the external functions which have asset and quote parameters.

The addresses for asset and quote could be the same and this should be prevented.

To fix this, we should add a require statement in any LOC of both functions

+++ ```require(quote != asset);```


[Issue 5]

Reentrancy is possible in _rubicallPayable and _rubicall functions which could send all fees without performing any actual meaningful targeted rubicall contract.

See 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L60-L73
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76-L89

In the _rubicall function, the first two LOCs are

```solidity
_chargeFee(_params.feeParams, _params.target); // line 1

(bool _OK, bytes memory _data) = _params.target.call( // line 2
  bytes.concat(_params.selector, _params.args)
);
```

If _params.target is set to own _rubicall function, both line 1 and line 2 will be executed recursively and drain all fee

In the _rubicallPayable function, the first two LOCs are

```solidity
uint256 _msgValue = _chargeFeePayable(_params.feeParams); // line 1

(bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}( // line 2
  bytes.concat(_params.selector, _params.args)
);
```

If _params.target is set to own _rubicallPayable function, both line 1 and line 2 will be executed recursively and drain all msg.value as fee

To fix this potential reentrancy problem, add ReentrancyGuard to parent function (which is rubicall)
See https://docs.openzeppelin.com/contracts/4.x/api/security#ReentrancyGuard

