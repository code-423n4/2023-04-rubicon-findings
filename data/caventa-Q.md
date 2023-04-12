1.

initialize functions can be front-run. The initialize function that initializes important contract state can be called by anyone.

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

The attacker can initialize the contract before the legitimate deployer, hoping that the victim continues to use the same contract. In the best case for the victim, they notice it and have to redeploy their contract costing gas.

Recommend using the constructor to initialize non-proxied contracts. For initializing proxy contracts, recommend deploying contracts using a factory contract that immediately calls initialize after deployment, or make sure to call it immediately after deployment and verify the transaction succeeded.

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

In the 

6.

Asset and quote can be the same

7.

spawnBuddy function can be frontrunned

8.

msg.sender != address(0) && // @audit msg.sender is impossible to be address(0)

9.

2 contracts may have same name symbol decimal

Should add token address as unique identifier