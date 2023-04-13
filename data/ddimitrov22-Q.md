# Overview

Rubicon v2 is an upgraded version of the Rubicon protocol, which has been live on Optimism for over a year. It replaces the old Pools system with a Compound V2 fork, enabling cross-margin long/short trading. The core of the protocol is RubiconMarket, an open order book contract that allows for limit and market orders for any arbitrary ERC20 trading pair of tokens. It embodies two core primitives of the new financial era: efficient modality of exchange paired with a robust multi-collateral money market.

# Findings Summary

| ID     | Title                                                                                                             | Severity      |
| ------ | ----------------------------------------------------------------------------------------------------------------- | ------------- |
| [L-01] | Initializer can be called by anybody and is front-runnable | Low          |
| [L-02] | Use a two-step ownership transfer approach | Low          |
| [L-03] | Functions with the same name | Low          |
| [L-04] | Missing array arguments length check | Low          |
| [N-01] | Separate contracts into different files | Non-Critical         |
| [N-02] | Solidity safe pragma best practices are not used | Non-Critical         |
| [N-03] | Mismatch between comments and code | Non-Critical         |
| [N-04] | Redundant code | Non-Critical         |
| [N-05] | Clean the code from unnecessary comments | Non-Critical         |
| [N-06] | NatSpecs are incomplete | Non-Critical         |
| [N-07] | Contract can be deleted | Non-Critical         |
| [N-08] | Already initialized variables | Non-Critical         |

# [L-01] Initializer can be called by anybody and is front-runnable

The `initialize` functions can be called by anybody because there is no access control implemented in the method. Thus, a malicious user can front-run the call and initialize the contracts. If the owner doesn't spot that a contract is already initialized, the malicious user will have access to restricted functions. In the best case scenario, the owner will spot the issue immediately and will redeploy the contract which will lead to wasted gas.

There are 2 instances of that:

    6 results - 2 files

    contracts/BathHouseV2.sol:
    32:     function initialize(address _comptroller, address _pAdmin) external {
    33:         require(!initialized, "BathHouseV2 already initialized!");

    contracts/RubiconMarket.sol:
    700:     function initialize(address _feeTo) public {
    701:         require(!initialized, "contract is already initialized");

**Recommendation**

Add `onlyOwner` modifier to `initialize()`.

# [L-02] Use a two-step ownership transfer approach

There are 5 methods with the `onlyOwner` modifier inside the `Position` contract which shows that the `owner` role is an important one. 

**Recommendation**

Make sure to use a two-step ownership transfer approach by using `Ownable2Step` from OpenZeppelin as opposed to `Ownable` as it gives you the security of not unintentionally sending the `owner` role to an address you do not control.

# [L-03] Functions with the same name

Inside `RubiconMarket` there are 5 external/public functions with the same name - `offer`:

    5 results - 1 file

    contracts/RubiconMarket.sol:
    510      /// @notice Key function to make a new offer. Takes funds from the caller into market escrow.
    511:     function offer(
    512          uint256 pay_amt,

    760      // simplest offer entry-point
    761:     function offer(
    762          uint256 pay_amt, //maker (ask) sell how much

    779  
    780:     function offer(
    781          uint256 pay_amt, //maker (ask) sell how much

    801      // Make a new offer. Takes funds from the caller into market escrow.
    802:     function offer(
    803          uint256 pay_amt, //maker (ask) sell how much

    823  
    824:     function offer(
    825          uint256 pay_amt, //maker (ask) sell how much

The functions take different arguments but their purpose is quite the same. This could be frustrating for user as they may not be sure which function is the correct one as the functions can be called by anybody. It also makes the contract much harder to read

**Recommendation**

Consider combining the functions into a single one or make one external `offer` function which calls an internal `_offer` function.

# [L-04] Missing array arguments length check

In `RubiconMarket: batchRequote` different arrays are taken as input arguments and are used in a `for()` loop. However, there is no check that the lengths of the arrays are the same which can lead to unexpected results. The same applies for the `constructor` inside `V2Migrator`.

**Recommendation**

Add checks to verify that all array lengths are equal.

# [N-01] Separate contracts into different files

There are contracts that contain several contracts inside them. For example, `RubiconMarket.sol` consists of 9 different contracts. This makes it more complex to follow the logic and harder to read. Consider separating the contracts in different files.

# [N-02] Solidity safe pragma best practices are not used

Some of the contracts in scope use floatable solidity compiler version. Always use a stable pragma to be certain that you deterministically compile the Solidity code to the same bytecode every time. Furthermore, consider using the same version for all the files as different version are used across the contracts.

# [N-03] Mismatch between comments and code

```solidity
    //============================= VIEW =============================

    /// @return balance - the user’s current borrow balance (with interest) in units of the underlying asset.
    /// not view function!
    function borrowBalance(address bathToken) public returns (uint256 balance) {
```

Inside `Position` the functions are categorized by view and main functions. The first function after the `view` comment statement is not view. Consider moving it under the correct category.

# [N-04] Redundant code

The `safeMath` imports are redundant as the compiler versions above 0.8.0 prevent arithmetic operations from underflow/overflow. Also, there are unused events in `BathBuddy`. There are also unused variable (`bool public AqueductDistributionLive` , `address public AqueductAddress`). Consider removing them to make the code cleaner.

# [N-05] Clean the code from unnecessary comments

There are a lot of events across the contracts that are commented out. Also, there are comments like [those](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L4-L5) which are not needed. Remove them to reduce the size of the contracts and make the code more readable.

# [N-06] NatSpecs are incomplete

`@notice`, `@param` and `@return` fields are missing throughout all of the contracts. NatSpec documentation is essential for a better understanding of the code by developers and auditors and is strongly recommended. Please refer to the [NatSpec format](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html) and follow the guidelines outlined there.

# [N-07] Contract can be deleted

The `DSAuthEvents` contracts inside the `RubiconMarket.sol` file consists of a single event. Consider moving the event to a contract where it is emitted and delete the contract.

# [N-08] Already initialized variables

The `matchingEnable` and `buyEnabled` variables are already set to `true` in `RubiconMarket`. There is no need to set them to `true` again inside the `initialize` function.