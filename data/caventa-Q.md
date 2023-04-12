1.

initialize functions and spawnBuddy function can be front-run. The initialize function that initializes important contract state can be called by anyone.

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

The attacker can call the function before the legitimate deployer, hoping that the victim continues to use the same contract. In the best case for the victim, they notice it and have to redeploy their contract costing gas.

Recommend using the constructor to initialize non-proxied contracts. For initializing proxy contracts, recommend deploying contracts using a factory contract that immediately calls function after deployment, or make sure to call it immediately after deployment and verify the transaction succeeded.

2.

See FeeWrapper.sol,

Feetype maybe 0 which will always cause the function always revert at this line of code
```fees[i] = (tokenAmounts[i] * feeValue) / feeType;```

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

To fix this, add a require function. (See above).

3. 

Fee maybe 0

4.

SafeMath library is not required in the solidity code which has version > 0.8.0 because the fixation of uint overflow and underflow problem is automatically applied.

Hence, 

add keyword should be replaced by +
sub keyword should be replaced by -
mod keyword should be replaced by *
div keyword should be replaced by /

For eg, 

rewardsPerTokensStored[token].add(
lastTimeRewardApplicable(token)
.sub(lastUpdateTime[token])
.mul(rewardRates[token])
.mul(1e18)
.div(IERC20(myBathTokenBuddy).totalSupply())
);

can be rewrite as

rewardsPerTokensStored[token] + (
lastTimeRewardApplicable(token)
- lastUpdateTime[token]
* rewardRates[token]
* 1e18
/ IERC20(myBathTokenBuddy).totalSupply())
);

5.

If the soidity contract has function parameter asset and quote, we should check both address should be different. Like in the following function of Position.sol

```
function buyAllAmountWithLeverage(
        address quote,
        address asset,
        uint256 quotePayAmount,
        uint256 leverage
    ) external onlyOwner {
        +++ require(quote != asset);
        //@audit what happened if quote and asset are the same
        _leverageCheck(leverage, true);
        require(
            openPosition(quote, asset, quotePayAmount, leverage),
            "buyAllAmountWithLeverage: FAILED TO OPEN POSITION"
        );
    }
```