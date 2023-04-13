
### [L-01] `BathBuddy.onlyBuddy` modifier is not used

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94-L102

```solidity
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

`BathBuddy` has `onlyBuddy` modifier, but it is not used. And there is a comment before it.

```solidity
    // TODO: do we need this?
    // Enforce only soul-bound Bath Token has calling rights
```

If this is not used, it is better to remove and resolve TODO before deploying.

### [L-02] `Position._borrowLimit` can be a view

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L526-L583

`Position.openPosition` gets borrow count and last borrow ratio from `_borrowLimit` for leveraged trading.
```solidity
        (vars.limit, vars.lastBorrow) = _borrowLimit( 
            bathTokenAsset,
            asset,
            initMargin,
            leverage
        );
```
`_borrowLimit` only contains view and pure functions and it doesn't change states, so it can be a view function.

### [L-03] `DSAuth` needs two step authentication

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28

`DSAuth` uses one step authentication, and this is not safe. 
```solidity
    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
```

If _owner is address(0) or an address we don't know the private key, we will lose admin privilege. So it is recommended to use two step authentication.


### [L-04] `SimpleMarket.buy` will not work when `feeTo` is address(0)

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L340


```solidity
    _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee)
```

`SimpleMarket.buy` sends fees to `feeTo`, but the address is not validated and it can be address(0). When `feeTo` is address(0), `SimpleMarket.buy` will revert.

`RubiconMarket` sets a non-zero address to `feeTo` in initialize, so `RubiconMarket` is okay. But `SimpleMarket` is not an abstract contract, so it can be deployed and used. So this issue will have low severity.


### [L-05] `ExpiringMarket` has unused `stopped` state

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L594

```solidity
    function stop() external auth {
        stopped = true;
    }
```
`ExpiringMarket` has `stopped` state and `stop` method to change it. But the state is not used at all. So we can remove stopped logic at the moment.


### [L-06] `V2Migrator.migrate` doesn't update reward states

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L55

```solidity
        require(
            IERC20(bathTokenV2).balanceOf(address(this)) == 0,
            "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
        );
```

`V2Migrator.migrate` mints `bathTokenV2`, so the total supply of `bathTokenV2` will change after `migrate`.
If the total supply is changed, the reward states for all reward tokens should be updated for correct reward amount.
It is recommended to update reward states for all tokens via `BathHouseV2.claimRewards` after `migrate`.


### [L-07] `_borrowLoop` with delta balance amount is meaningless

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L163-L173

```solidity
            // check to prevent underflow!
            if (vars.initAssetBalance == 0) {
                vars.currentAssetBalance = assetBalance;
            } else if (vars.initAssetBalance < assetBalance) {
                vars.currentAssetBalance = assetBalance.sub(
                    vars.initAssetBalance
                );
            } else {
                vars.currentAssetBalance = vars.initAssetBalance.sub(
                    assetBalance
                );
            }
```

`Position.openPosition` calls `_borrowLoop` when asset balance is less than the initial asset balance to prevent underflow.
But during `_borrowLoop`, we don't expect that asset balance is less than the initial asset balance. And if the asset balance goes under the initial balance, it is normal to break the loop. `_borrowLoop` with delta balance amount has no meaning, so it is recommended to break the loop instead of continuing loop with delta balance.



### [L-08] Open TODOs

- https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L675

There are open TODOs in the repository ready for auditing and deploy.

```solidity
    bool public buyEnabled = true; //buy enabled TODO: review this decision!
```

`buyEnabled` is always true in RubiconMarket, so we can remove `buyEnabled` from RubiconMarket, but it is still in TODO list. It is recommended to resolve all TODO list before going live.

