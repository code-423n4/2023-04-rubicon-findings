### Non Critical Issues

| Number  | Issue                                  | Instances |
| :-----: | :------------------------------------- | :-------: |
| [NC-01] | Function grouping and ordering         |     6     |
| [NC-02] | Maximum Line Length                    |    13     |
| [NC-03] | Typos                                  |     5     |
| [NC-04] | Remove commented out code              |    19     |
| [NC-05] | Use consistent formatting for comments |    67     |
| [NC-06] | Use consistent indentaton characters   |     4     |
| [NC-07] | File too large                         |     1     |
| [NC-08] | Magic numbers                          |    16     |
| [NC-09] | Number literals > 999                  |     2     |

#### [NC-01] Function grouping and ordering

Functions should be grouped according to their visibility and ordered per the Solidity Style Guide. Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. See [Order of Functions](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-functions).

1. In [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol), in the [SimpleMarket contract](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L218):
   1. The `external` functions, `bump()`, `kill()`, `make()`, `take()`, should come right after the modifiers and before the `public` functions.
   2. The `public` functions `getFeeBPS()` and `calcAmountAfterFee()` should come before the `internal` functions.
1. In [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol), in the [ExpiringMarket contract](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L593):
   1. The `external` `stop()` function should come before the `public` functions.
1. In [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol), in the [RubiconMarket contract](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L674):
   1. The `external` functions, `kill()`, `offer()` (the three `external` ones), `batchOffer()`, `batchCancel()`, `batchRequote()`, `del_rank()`, `setMinSell()`, `getMinSell()`, `getBetterOffer()`, `sellAllAmount()`, `buyAllAmount()`, `getBuyAmountWithFee()`, `setFeeBPS()`, `setMakerFee()`, `setFeeTo()`, `getFeeTo()`, should come right after the modifiers and before the `public` functions.
1. In [BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol):
   1. The `external` functions, `whoIsBuddy()`, `createBathToken()`, `claimRewards()`, `getReward()`, should come before the `public` function `getBathTokenFromAsset()`.
1. In [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol):
   1. The `external` functions, `getRewardForDuration()`, `getReward()`, `notifyRewardAmount()`, `setRewardsDuration()`, should come before any `public` functions.
   2. The `modifier` `updateReward()` should come before any functions.
   3. The events, `RewardAdded()`, `Staked()`, `Withdrawn()`, `RewardPaid()`, `RewardsDurationUpdated()`, `Recovered()`, should come before the modifiers.
1. In [Position.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol):
   1. The `external` functions, `buyAllAmountWithLeverage()`, `sellAllAmountWithLeverage()`, `closePosition()`, `increaseMargin()`, `withdraw()`, should come before any `public` functions.

#### [NC-02] Maximum Line Length

The maximum suggested line length is 120 characters per the Solidity Style Guide. Several lines are longer than this, making the code unnecessarily difficult to read and maintain. See [Maximum Line Length](https://docs.soliditylang.org/en/latest/style-guide.html#maximum-line-length).

The following lines exceed 120 characters.

1. [RubiconMarket.sol L9](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L9)
1. [RubiconMarket.sol L250](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L250)
1. [RubiconMarket.sol L312](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L312)
1. [RubiconMarket.sol L348](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L348)
1. [RubiconMarket.sol L522](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L522)
1. [RubiconMarket.sol L524](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L524)
1. [RubiconMarket.sol L693](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L693)
1. [RubiconMarket.sol L726](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L726)
1. [BathBuddy.sol L28](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L28)
1. [BathBuddy.sol L31](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L31)
1. [BathBuddy.sol L35-L36](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L35-L36)
1. [BathBuddy.sol L66](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L66)
1. [BathBuddy.sol L166-L167](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L166-L167)

#### [NC-03] Typos

There are some typos that should be corrected.

1.  ```solidity
    File: contracts/RubiconMarket.sol

    // Current
    8:    /// @notice Please see the repository for this code at https://github.com/RubiconDeFi/rubicon-protocol-v1;

    // Protocol version should be updated from v1 to v2
    8:    /// @notice Please see the repository for this code at https://github.com/RubiconDeFi/rubicon-protocol-v2;
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    // Current
    250:    // /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it

    // Remove commented out NatSpec (or is this a commented out NatSpec comment that should be removed?)
    250:    /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    // Current
    324:    ///@dev For backwards semantic compatibility.

    // Add space after NatSpec comment forward slashes
    324:    /// @dev For backwards semantic compatibility.
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    // Current
    718:    // // After close, anyone can cancel an offer

    // Remove extra double forward slash.
    324:    // After close, anyone can cancel an offer
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    // Current
    886: /// @notice Batch offer functionality - multuple offers in a single transaction

    // Fix typo in "multiple"
    886: /// @notice Batch offer functionality - multiple offers in a single transaction
    ```

#### [NC-04] Remove commented out code

Commented-out code is distracting and confusing for developers who read the surrounding code, and its significance is often unclear. It will not get compiled or tested when the code around it changes, so it's likely to break over time. For these reasons, commented-out code should be avoided.

1. [RubiconMarket.sol L100-L116](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L100-L116)
1. [RubiconMarket.sol L129-L150](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L129-L150)
1. [RubiconMarket.sol L163-L172](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172)
1. [RubiconMarket.sol L185-L195](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L185-L195)
1. [RubiconMarket.sol L300-L309](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L300-L309)
1. [RubiconMarket.sol L386-L396](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396)
1. [RubiconMarket.sol L409-L417](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417)
1. [RubiconMarket.sol L429-L434](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L429-L434)
1. [RubiconMarket.sol L464-L472](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472)
1. [RubiconMarket.sol L542-L551](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551)
1. [RubiconMarket.sol L661](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L661)
1. [RubiconMarket.sol L663](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L663)
1. [RubiconMarket.sol L666](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L666)
1. [RubiconMarket.sol L838](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L838)
1. [RubiconMarket.sol L850-L851](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L850-L851)
1. [RubiconMarket.sol L1343-L1359](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1343-L1359)
1. [RubiconMarket.sol L1382](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1382)
1. [RubiconMarket.sol L1395](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1395)
1. [Position.sol L263](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L263)

#### [NC-05] Use consistent formatting for comments

Most standard single line comments in this code base use a space after the start of the double-forward slash start of comment. For example:

```solidity
File: contracts/RubiconMarket.sol

573:    // Fee logic
```

However, there are some single line comments that do not have a space after the double-forward slash start of comment. There is no right or wrong way to write these comments. The compiler will ignore anything after the second forward slash. With that being said, it is important to be consistent. It is recommended to write comments one way or the other, but to not use a mixture of both. Using a space after the double-forward slash start of comment is the most common style and is recommended here.

There are dozens of instances of use of the no-space style in contracts/RubiconMarket.sol. Listing all instances in contracts/RubiconMarket.sol would require including an excessive amount of code in this report. Below are a couple snippets.

1.  ```solidity
    File: contracts/RubiconMarket.sol

    675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!
    676:    bool public matchingEnabled = true; //true: enable matching,

    687:    uint256 next; //points to id of next higher offer
    688:    uint256 prev; //points to id of previous lower offer
    689:    uint256 delb; //the blocknumber where this entry was marked for delete
    ```

1.  ```solidity
    File: contracts/periphery/BathBuddy.sol

    55:    mapping(address => uint256) public lastUpdateTime; //Token specific
    ```

#### [NC-06] Use consistent indentaton characters

For a given code base, either spaces or tabs should be used to indent the code. There is no right or wrong here; however, it is very important for readability and maintainability that one or the other indentation method be used consistently. The vast majority of the in-scope contracts use four spaces for indentation, but the following lines have tabs for indentiation. To stay consistent with the rest of the code, the tabs should be replaced by four spaces.

Setting "Render Whitespace" to "all" in Visual Studio Code, or using an equivalent setting in the editor of choice, will make this indentation inconsistency easy to identify.

1. [Position.sol L538](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L538)
1. [Position.sol L543](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L543)
1. [Position.sol L547](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L547)
1. [FeeWrapper.sol L119](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L119)

#### [NC-07] File too large

The file [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol) contains nine contracts and is nearly 1500 lines long. To promote easier maintenance and reduce the chance of merge conflicts when multiple developers are simultaneously changing the contracts in this file, consider splitting some of the contracts into separate files.

#### [NC-08] Magic numbers

Constants should be defined rather than using magic numbers.

1.  ```solidity
    File: contracts/RubiconMarket.sol

    583:    _amount -= mul(amount, feeBPS) / 100_000;

    586:    _amount -= mul(amount, makerFee()) / 100_000;

    649:    foo := calldataload(4)

    711:    feeBPS = 1;

    943:    _rank[id].delb < block.number - 10

    1057:   mul(pay_amt, 10 ** 9),

    1059:   ) / 10 ** 9;

    1099:   mul(buy_amt, 10 ** 9),

    1101:   ) / 10 ** 9;

    1142:   mul(pay_amt, 10 ** 9),

    1144:   ) / 10 ** 9;

    1175:   mul(buy_amt, 10 ** 9),

    1177:   ) / 10 ** 9;
    ```

1.  ```solidity
    File: contracts/utilities/poolUtility/Position.sol

    481:    uint256 _fee = _minFill.mul(_feeBPS).div(10000);

    490:    _fee = _payAmount.mul(_feeBPS).div(10000);

    588:    uint256 _leverageMax = WAD.mul(3);
    ```

#### [NC-09] Number literals > 999

Consider using underscores for number literals greater than 999 to improve readability. Example:

```solidity
// Before
uint256 _fee = _minFill.mul(_feeBPS).div(10000);
```

```solidity
// After
uint256 _fee = _minFill.mul(_feeBPS).div(10_000);
```

1.  ```solidity
    File: contracts/utilities/poolUtility/Position.sol

    481:    uint256 _fee = _minFill.mul(_feeBPS).div(10000);

    490:    _fee = _payAmount.mul(_feeBPS).div(10000);
    ```

### Low Risk Issues List

| Number | Issue                              | Instances |
| :----: | :--------------------------------- | :-------: |
| [L-01] | Unspecific compiler version pragma |     2     |
| [L-02] | Unsafe casting of uints            |    16     |

#### [L-01] Unspecific compiler version pragma

The compiler version specified for a contract should be locked to the version it has been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors. [locking-pragmas](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/)

For example:

```solidity
// bad
pragma solidity ^0.8.9;

// good
pragma solidity 0.8.9;
```

```solidity
File: contracts/RubiconMarket.sol

2:    pragma solidity ^0.8.9;
```

```solidity
File: contracts/periphery/BathBuddy.sol

2:    pragma solidity ^0.8.0;
```

#### [L-02] Unsafe casting of uints

Downcasting from uint256 in Solidity does not revert on overflow. This can easily result in undesired exploitation or bugs, since developers usually assume that overflows raise errors. [OpenZeppelin's SafeCast](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) restores this intuition by reverting the transaction when such an operation overflows. Using this library instead of the unchecked operations eliminates an entire class of bugs, so it’s recommended to use it always.

For example:

```solidity
// Before
info.timestamp = uint64(block.timestamp);
```

```solidity
// After
info.timestamp = toUint64(block.timestamp);
```

1. In [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

```solidity
    File: contracts/RubiconMarket.sol

    321:    require(uint128(spend) == spend, "spend is not an int");
    322:    require(uint128(quantity) == quantity, "quantity is not an int");

    405:    uint128(quantity),
    406:    uint128(spend)

    480:    uint128(_offer.pay_amt),
    481:    uint128(_offer.buy_amt)

    519:    require(uint128(pay_amt) == pay_amt);
    520:    require(uint128(buy_amt) == buy_amt);

    534:    info.timestamp = uint64(block.timestamp);

    559:    uint128(pay_amt),
    560:    uint128(buy_amt)

    625:    return uint64(block.timestamp);

    1053:   take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

    1061:   take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

    1093:   take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

    1103:   take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need
```
