## Table of Contents

- [L-01] Position.sol#_borrowLimit() in Position.sol does not check whether the bathToken is listed
- [L-02] RubiconMarket.sol#isClosed() should be removed because it always return false, making checks redundant
- [L-03] RubiconMarket.sol#stop() does not do anything
- [L-04] Truncation to zero may apply for tokens with low decimals, resulting in no fees paid
- [N-01] Remove commented out code
- [N-02] Use a more recent version of solidity
- [N-03] Non-library/interface files should use fixed compiler versions, not floating ones
- [N-04] Remove onlyBuddy modifier


### [L-01] Position.sol#_borrowLimit() in Position.sol does not check whether the bathToken is listed

Position.sol [Line 532](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L532)

When calling comptroller.markets, three tuple of values will be returned (isListed, collateralFactorMantissa, isComped); isListed represents whether the comptroller recognizes this cToken; collateralFactorMantissa, scaled by 1e18, is multiplied by a supply balance to determine how much value can be borrowed. The isComped boolean indicates whether or not suppliers and borrowers are distributed COMP tokens.

In Position.sol#_borrowLimit, only one value is returned

```
        (, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
```

It is good practice to check whether the bathToken is listed. Check the boolean value of isListed as well
```
-        (, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
+        (bool isListed, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
+	   require(isListed, "bathToken not Listed");
```

Reference: https://docs.compound.finance/v2/comptroller/#collateral-factor

### [L-02] RubiconMarket.sol#isClosed() should be removed because it always return false, making checks redundant

[Lines 620](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620-L622)

```
    function isClosed() public pure returns (bool closed) {
        return false;
    }
```

In Rubicon v1 audit, the comment about this medium issue is that the functionality is intended. However, if the market will forever be open, then this function should not exist at all because every check waste gas. The isClosed() functionality does not need to be checked at all. As such, the can_offer() modifier is not required as well, so it can be taken out for best practice, in accordance to SWC-131: Irrelevant Code. 


[Lines 597](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L597-L618)

```
 -   modifier can_offer() override {
 -       require(!isClosed());
 -       _;
    }


    /// @dev After close, no new buys are allowed.
    modifier can_buy(uint256 id) override {
        require(isActive(id));
-        require(!isClosed());
        _;
    }


    /// @dev After close, anyone can cancel an offer.
    modifier can_cancel(uint256 id) virtual override {
        require(isActive(id));
        require(
            (msg.sender == getOwner(id)) ||
 -               isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );
        _;
    }
```

### [L-03] RubiconMarket.sol#stop() does not do anything


The ExpiringMarket contract instantialized the variable `stopped`, and the default is set to false. There is a function in Line 628 called stop() which sets the stopped variable to true. 

```
    function stop() external auth {
        stopped = true;
    }
}
```

The stopped variable does nothing at all because it is not used in any other functions and also it cannot be set to false, which means `stopped` will forever be true once stop() is called. As the code is irrelevant, best practice is to remove the code to avoid clutter. Kept at low because of SWC-131: Irrelevant Code violation.

### [L-04] Truncation to zero may apply for tokens with low decimals, resulting in no fees paid

Some tokens with low decimal palces can escape fees. In RubiconMarket#calcAmountAfterFee(), the amount is multiplied by feeBPS and then divided by the denominator 100,000. 

```
    function calcAmountAfterFee(
        uint256 amount
    ) public view returns (uint256 _amount) {
        require(amount > 0);
        _amount = amount;
        _amount -= mul(amount, feeBPS) / 100_000;


        if (makerFee() > 0) {
            _amount -= mul(amount, makerFee()) / 100_000;
        }
    }
```

Because Rubicon Market is an order book protocol, any type of tokens is allowed, including those with 0 decimal places. 

For example, [SingularDTV has 0 decimal places](https://etherscan.io/address/0xaec2e87e0a235266d9c5adc9deb4b2e29b54d009#readContract)

Assuming a feeBPS of 1000 (1%), and 99 token amount.

_amount -= mul(amount, feeBPS) / 100_000;
_amount -= mul(99, 1000) / 100_000 = _amount -= 0

When transacting with tokens that has low or zero decimals, the fee can be escaped.

Consider checking the amount of decimal values that a token has in order to prevent truncation to zero errors.

### [N-01] Remove commented out code

Removing commented out code for best practice
```
        /// emit LogTake(
        ///     bytes32(id),
        ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
        ///     _offer.owner,
        ///     _offer.pay_gem,
        ///     _offer.buy_gem,
        ///     msg.sender,
        ///     uint128(quantity),
        ///     uint128(spend),
        ///     uint64(block.timestamp)
        /// );
```


### [N-02] Use a more recent version of solidity

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value. Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)

```
-	pragma solidity ^0.8.9;
+	pragma solidity ^0.8.17;
```

### [N-03] Non-library/interface files should use fixed compiler versions, not floating ones

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively. Lock the pragma version and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.

Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile locally.

```
-	pragma solidity ^0.8.9;
+	pragma solidity 0.8.9;
```

### [N-04] Remove onlyBuddy modifier

onlyBuddy is not in use. Consider removing it to prevent clutter.

```
    // TODO: do we need this?
    // Enforce only soul-bound Bath Token has calling rights
    modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }


```

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L92-L103