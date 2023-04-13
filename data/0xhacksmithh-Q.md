### [Low-01] Absence Of Check That Both Input Arrays Are Of Same Length or Not
```solidity
  function claimRewards(
        address[] memory buddies, 
        address[] memory rewardsTokens 
    ) external {
+       require(buddies.length == rewardsToken.length, "error");
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
```
Should check Both array have same length.

*Instances(1)*
```
File : contracts/BathHouseV2.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L117-L130
```
### [Low-02] `modifier` May Be Not Working As Intended.
```solidity
 function isClosed() public pure returns (bool closed) {  // @audit-issue
        return false;
    }
```
```solidity
modifier can_offer() override { // @audit-issue as 
        require(!isClosed());
        _;
    }
```
isClosed always return falsese so, This modifier is always true irrespective of situations.

*Instances(1)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L597-L600
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L623
```
### [Low-03] Double Initializing state variables
```solidity
        initialized = true;
        matchingEnabled = true; // @audit already initialized
        buyEnabled = true; // @audit double initialization
```
These both variables are already initialized with there current value during defining time. 
*Instances(2)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L714-L715
```

### [Low-04] Absence Of Access Controls On Function ```del_rank()```
Code comment saying that ```//  Function should be called by keepers```
But there is no any access control present
```slidity
    function del_rank(uint256 id) external returns (bool) {  // @audit-issue absence of access control
        require(!locked); // @audit

        require(
            !isActive(id) &&  // @audit
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );
        delete _rank[id];
        emit LogDelete(msg.sender, id);
        return true;
    }
``` 

*Instances(1)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L937-L948
```

### [Low-05] Unnecessary `checks`
```solidity
while (pay_amt > offers[offerId].buy_amt) { // @audit can be cached
            fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator
            pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay
-            if (pay_amt > 0) { // @audit unnecessary condition as it allways true
                //If we still need more offers
                offerId = getWorseOffer(offerId); //We look for the next best offer
                require(offerId != 0); //Fails if there are not enough offers to complete
            }
```
As above `while` checks ```pay_amt > offers[offerId].buy_amt```
and then substarct ```sub(pay_amt, offers[offerId].buy_amt)```

So ```pay_amt > 0``` will always gonna be true, no need for checks. 

*Instances(1)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1133
```

### [Low-06] No Need To Check ```_underlying != address(0)``` In internal function ```_bathify()``` As It is already checked in parent function

```solidity
    function _bathify(
        address _underlying
    )
        internal
        view
        returns (string memory _name, string memory _symbol, uint8 _decimals)
    {
-       require(_underlying != address(0), "_bathify: ADDRESS ZERO"); 

        _name = string.concat("bath", ERC20(_underlying).symbol());
        _symbol = string.concat(_name, "v2");
        _decimals = ERC20(_underlying).decimals();
    }
```

*Instances(1)*
```
File : contracts/BathHouseV2.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L146
```

### [NC-01] Floating and Old Pragma(Solidity Varsion) Used

*Instances(6)*
```
File : All Contracts
```

### [NC-01] Use Scientific Notetions
Instead of 10**18 try to use ```10e18```

*Instances(2)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L75
```
