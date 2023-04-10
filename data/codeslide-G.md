### Gas Optimizations

| Number | Issue                                              | Instances |
| :----: | :------------------------------------------------- | :-------: |
| [G-01] | Use scientific notation                            |    11     |
| [G-02] | Mark functions as payable                          |     7     |
| [G-03] | Use short circuiting to save gas                   |     1     |
| [G-04] | Change function visibility from public to external |    10     |
| [G-05] | State variables should be cached                   |     6     |
| [G-06] | Cache array length outside of loop                 |     1     |
| [G-07] | Using bools for storage incurs overhead            |     1     |

#### [G-01] Use scientific notation

Use scientific notation like `1e18` rather than `10 ** 18` to avoid an unnecessary arithmetic operation and save gas.

1.  ```solidity
    File: contracts/RubiconMarket.sol

    74:    uint256 constant WAD = 10 ** 18;
    75:    uint256 constant RAY = 10 ** 27;
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1057:    mul(pay_amt, 10 ** 9),
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1059:    ) / 10 ** 9;
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1099:    mul(buy_amt, 10 ** 9),
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1101:    ) / 10 ** 9
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1142:    mul(pay_amt, 10 ** 9),
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1144:    ) / 10 ** 9
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1175:    mul(buy_amt, 10 ** 9),
    ```

1.  ```solidity
    File: contracts/RubiconMarket.sol

    1177:    ) / 10 ** 9
    ```

1.  ```solidity
    File: contracts/utilities/poolUtility/Position.sol

    317:    _max = (_liq.mul(10 ** 18)).div(_price);
    ```

1.  ```solidity
    File: contracts/utilities/poolUtility/Position.sol

    331:    ).div(10 ** 18);
    ```

#### [G-02] Mark functions as payable

Functions guaranteed to revert when called by normal users can be marked `payable`. If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

1. [BathBuddy.sol L191](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191) `notifyRewardAmount()`
1. [BathBuddy.sol L232](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232) `setRewardsDuration()`
1. [Position.sol L93](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L93) `buyAllAmountWithLeverage()`
1. [Position.sol L107](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L107) `sellAllAmountWithLeverage()`
1. [Position.sol L210](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210) `closePosition()`
1. [Position.sol L226](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L226) `increaseMargin()`
1. [Position.sol L242](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L242) `withdraw()`

#### [G-03] Use short circuiting to save gas

Use short circuiting to save gas by putting the cheaper comparison first.

```solidity
File: contracts/utilities/poolUtility/Position.sol

// before - always performing arithmetic
176:    if (i.add(1) == vars.limit && vars.lastBorrow != 0) {

// after - only performing arithmetic if local variable is not zero
176:    if (vars.lastBorrow != 0 && i.add(1) == vars.limit) {
```

#### [G-04] Change function visibility from public to external

Change function visibility from `public` to `external` if the function is never called from within the contract. For public functions, the input parameters are copied to memory automatically, which costs gas. If the function is only called externally, then it should be marked as external since the parameters for an external function are not copied into memory but are read from calldata directly.

1. [BathHouseV2.sol Line L45](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L45) `getBathTokenFromAsset()`
1. [RubiconMarket.sol Line L288](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288) `getOffer()`
1. [RubiconMarket.sol Line L574](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574) `getFeeBPS()`
1. [RubiconMarket.sol Line L624](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624) `getTime()`
1. [RubiconMarket.sol Line L700](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L700) `initialize()`
1. [RubiconMarket.sol Line L733](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733) `make()`
1. [RubiconMarket.sol Line L998](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L998) `getOfferCount()`
1. [RubiconMarket.sol Line L1010](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010) `getFirstUnsortedOffer()`
1. [RubiconMarket.sol Line L1016](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016) `getNextUnsortedOffer()`
1. [RubiconMarket.sol Line L1149](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1149) `getPayAmountWithFee()`

#### [G-05] State variables should be cached

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

Saves 100 gas per instance

```solidity
File: contracts/periphery/BathBuddy.sol

// periodFinish[token]
118:    : periodFinish[token];

// periodFinish[address(rewardsToken)]
200:    uint256 remaining = periodFinish[address(rewardsToken)].sub(

// rewardsDuration[address(rewardsToken)]
207:    rewardsDuration[address(rewardsToken)]

// rewardsDuration[address(rewardsToken)]
219:    balance.div(rewardsDuration[address(rewardsToken)]),

// rewardsDuration[address(rewardsToken)]
225:    rewardsDuration[address(rewardsToken)]
```

```solidity
File: contracts/utilities/poolsUtility/Position.sol

// lastPositionId
418:    emit PositionOpened(lastPositionId, pos);
```

#### [G-06] Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

```solidity
File: contracts/utilities/poolsUtility/Position.sol

158:    for (uint256 i = 0; i < vars.limit; ++i) {
```

#### [G-07] Using bools for storage incurs overhead

Use `uint256(1)` and `uint256(2)` for `true`/`false` to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from `false` to `true`, after having been `true` in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

```solidity
File: contracts/RubiconMarket.sol

224:    bool locked;
```
