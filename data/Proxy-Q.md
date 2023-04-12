## [N-01] `modifier onlyBuddy()` is not used anywhere

Code Snippet [BathBuddy.sol#L94-L102](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L94-L102)

```js
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

### Recommendation

Consider removing it

## [N-02] Unlocked pragma solidity

Code Snippet [BathBuddy.sol#L2](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/BathBuddy.sol#L2)

```js
pragma solidity ^0.8.0;
```

### Recommendation

Consider using a locked pragma solidity verison across all contracts as this will improve test predictability

## [N-03] Use scientific notation

Use scientific notation `10e9` instead of `10**9`

Code Snippet

- [RubiconMarket.sol#L1057](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1057)
- [RubiconMarket.sol#L1059](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1059)
- [RubiconMarket.sol#L1099](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1099)
- [RubiconMarket.sol#L1101](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1101)
- [RubiconMarket.sol#L1142](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1142)
- [RubiconMarket.sol#L1144](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1144)
- [RubiconMarket.sol#L1175](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1175)
- [RubiconMarket.sol#L1177](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L1177)

Make sure to check for any other instances if any were missed.

## [N-04] Use CapWords for event names

- [RubiconMarket.sol#L118](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L118)
- [RubiconMarket.sol#L152](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L152)
- [RubiconMarket.sol#L174](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L174)
- [RubiconMarket.sol#L198](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L198)
- [RubiconMarket.sol#L209](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L209)

## [N-05] Rename modifier `synchronized()` to something like `noReenetrancy()` for clarity

Modifier `synchronized()` is used to prevent reentrancy but its name does not convey that.
Rename it to `noReenetrancy()` or `nonReentrant()` or use Reentrancy guard from OpenZeppelin.

## [L-01] Import interface instead of making a new one with the same name

There is no need to make a new interface, instead it should be imported since it already exists.
It's better to not make a new interface, because if you do, you have to make sure that both interfaces stay the same when something changes and it increases the amount of code one has to review

Code Snippet [V2Migrator.sol#L8-L14](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/V2Migrator.sol#L8-L14)

### Recommendation

Remove:

```js
interface IBathToken is IERC20 {
    function withdraw(
        uint256 shares
    ) external returns (uint256 amountWithdrawn);

    function underlyingToken() external view returns(IERC20);
}
```

Add:

```js
import "./interfaces/IBathToken.sol";
```

## [L-02] Input array lengths may differ causing unwanted behavior

Code Snippet

- [V2Migrator.sol#L30](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/V2Migrator.sol#L30)
- [BathHouseV2.sol#L115-L128](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/BathHouseV2.sol#L115-L128)
- [RubiconMarket.sol#L917-L933](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L917-L933)

### Recommendation

Use a `require` statement to check all array lengths are the same

## [L-03] Useless casting of `pay_amt` and `buy_amt` in `require` statement could lead to unwanted behavior

The casting in the `require` statement is only applicable to the `require` statement and nowhere else.
The function may revert because of unsafe casting or because `pay_amt` and `buy_amt` are bigger values than `type(uint128).max` which could lead to unwanted behavior.

Code Snippet [RubiconMarket.sol#L519-L520](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/RubiconMarket.sol#L519-L520)

```js
require(uint128(pay_amt) == pay_amt);
require(uint128(buy_amt) == buy_amt);
```

### Recommendation

Use `uint256` for all `pay_amt` and `buy_amt` instances
