## Summary

### Gas Optimizations

|               | Issue                                                                                                                | Instances | Total Gas Saved |
| ------------- | :------------------------------------------------------------------------------------------------------------------- | :-------: | :-------------: |
| [G&#x2011;01] | With assembly,  `.call (bool success)`  transfer can be done gas-optimized                                           |     3     |        -        |
| [G&#x2011;02] | Use assembly to write `address storage` values                                                                       |    10     |        -        |
| [G&#x2011;03] | Avoid `contract existence` checks by using solidity version 0.8.10 or later                                          |     5     |       500       |
| [G&#x2011;04] | Use nested if and, avoid multiple check combinations                                                                 |     5     |        -        |
| [G&#x2011;05] | Not Using the named `return` variables when a function returns, `WASTES DEPLOYMENT GAS`                              |    37     |        -        |
| [G&#x2011;06] | Expressions for constant values such as a call to `keccak256`,should be immutable rather than constant gas           |     1     |        -        |
| [G&#x2011;07] | Internal functions only called once can be `INLINED` to save GAS                                                     |    22     |       660       |
| [G&#x2011;08] | Using `private` rather than public for `constants`, saves gas                                                        |     2     |        -        |
| [G&#x2011;09] | Functions guranteed to `revert` when called by normal users can be marked `payable`                                  |    13     |       273       |
| [G&#x2011;10] | `<X> += <Y>`costs more gas than `<X> = <X> + <Y>`for state variables                                                 |     2     |        -        |
| [G&#x2011;11] | Public functions not called by the contract should be declared `external` instead                                    |     8     |        -        |
| [G&#x2011;12] | Setting the `constructor` to `payable`                                                                               |     2     |       26        |
| [G&#x2011;13] | Use solidity version 0.8.19 to gain some gas boost                                                                   |     2     |       176       |
| [G&#x2011;14] | Multiple address/id mappings can be combined into a `single mapping` of and address/id to a struct where appropriate |    13     |        -        |
| [G&#x2011;15] | State variables should be cached in `stack variables` rather than re-reading them from `storage`                     |    18     |      1746       |
| [G&#x2011;16] | Stack variable used as a cheaper cache for `state variables` is `only used once`                                     |     1     |        3        |
| [G&#x2011;17] | Can make the `variable` outside the loop to save gas                                                                 |     2     |        -        |
| [G&#x2011;18] | Should use `arguments` instead of state variable                                                                     |     4     |        -        |
| [G&#x2011;19] | Before some functions, we should `check some variables` for possible gas save                                        |     5     |        -        |
| [G&#x2011;20] | State variables can be packed into fewer storage slots                                                               |     3     |      6000       |
| [G&#x2011;21] | The result of function calls should be cached rather than re-calling the function                                    |    14     |        -        |
| [G&#x2011;22] | Reorder structure layout                                                                                             |     1     |        -        |
| [G&#x2011;23] | State variable only set in the `constructor` should be declared `immutable`                                          |     4     |      8388       |
| [G&#x2011;24] | `Duplicated` require()/if() checks should be refactored to a modifier or function                                    |    24     |        -        |
| [G&#x2011;25] | Multiple accesses of a `mapping/array` should use a local variable cache                                             |    34     |      1428       |
| [G&#x2011;26] | A modifier used only once and not being `inherited` should be inlined to save gas                                    |     1     |        -        |
| [G&#x2011;27] | Use `Modifiers` Instead of Functions To Save Gas                                                                     |     9     |       270       |
| [G&#x2011;28] | Help the optimizer by saving a storage variable's reference instead of repeatedly fetching it                        |     8     |        -        |
| [G&#x2011;29] | Using > 0 costs more gas than != 0 when used on a uint in a require() and assert statement                           |     8     |        -        |
| [G&#x2011;30] | Copying struct to memory can be more expensive than just reading from storage                                        |     7     |        -        |
| [G&#x2011;31] | `block.timeStamp` directly call instead of a function                                                                |     1     |        -        |
| [G&#x2011;32] | require() or revert() statements that check input arguments should be at the top of the function                     |     2     |        -        |
| [G&#x2011;33] | Use double `require` instead of using `&&`                                                                           |    13     |       39        |

Total: 284 instances over 33 issues

## Gas Optimizations

## [G-01] With assembly, .call (bool success)  transfer can be done gas-optimized

### Summary

return data (bool success,) has to be stored due to EVM architecture, but in a usage like below, ‘out’ and ‘outsize’ values are given (0,0), this storage disappears and gas optimization is provided.

https://twitter.com/pashovkrum/status/1607024043718316032?t=xs30iD6ORWtE2bTTYsCFIQ&s=19

### Details

Did you know that the normal "(bool success, bytes memory returnData) = http://target.call()" automatically copies the return data to memory even if you omit the returnData variable? If a relayer executes transactions with such calls it can lead to a gas-griefing attack.

There are **3** instances of this issue.

### Github Permalinks

```solidity
File: contracts/utilities/FeeWrapper.sol

66         (bool _OK, bytes memory _data) = _params.target.call(
67            bytes.concat(_params.selector, _params.args)
68        );


82         (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
            bytes.concat(_params.selector, _params.args)
        );

118        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

### Recommendation Code

```solidity
File: /contracts/utilities/FeeWrapper.sol

-  118        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");

+    bool OK;
+    assembly {
+       OK :=  call(gas(), payable(_feeTo), _feeAmount, 0, 0)
+      }

119        require(OK, "ETH transfer failed");
```

## [G-02] Use assembly to write address storage values

### Details

There are **10** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

26         owner = owner_;

532        info.recipient = recipient;

533         info.owner = owner;

705         feeTo = _feeTo;

1478        feeTo = newFeeTo;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol#

34        comptroller = Comptroller(_comptroller);

36        proxyAdmin = _pAdmin;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#

```solidity
File: /contracts/periphery/BathBuddy.sol

77        owner = _owner;

78        myBathTokenBuddy = newBud;

79        bathHouse = _bathHouse;

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

### Recommendation Code

```solidity
File: /contracts/periphery/BathBuddy.sol

71    function spawnBuddy(
72        address _owner,
73        address newBud,
74        address _bathHouse
75    ) external {
76        require(!friendshipStarted, "I already have a buddy!");
- 77        owner = _owner;

+         assembly {
+               sstore(owner.slot, _owner)
+            }
```

## [G-03] Avoid contract existence checks by using low level calls

### Summary

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

### Details

There are **5** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

340            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),

360                    _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),

380            _offer.pay_gem.transfer(msg.sender, quantity),

461            : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

538        require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-04] Use nested if and, avoid multiple check combinations

### Summary

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

### Details

There are **5** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

349             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

1197        if (
            isActive(id) &&
            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
        )

1324        if (
            t_buy_amt > 0 &&
            t_pay_amt > 0 &&
            t_pay_amt >= _dust[address(t_pay_gem)]
        )
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

176            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {


391        if (
            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
            borrowBalance(_bathTokenAsset) == 0
        )
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

-176            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {


+              if (i.add(1) == vars.limit) {
+                    if (vars.lastBorrow != 0) {}
+                  }
```

## [G-05] NOT USING THE NAMED RETURN VARIABLES WHEN A FUNCTION RETURNS, WASTES DEPLOYMENT GAS

Do not use return at the end of the function:

### Details

There are **36** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

46     returns (uint256 z) {

50     returns (uint256 z) {

54     returns (uint256 z) {

58      returns (uint256 z) {

62    returns (uint256 z) {

66     returns (int256 z) {

70     returns (int256 z) {

77    returns (uint256 z) {

81     returns (uint256 z) {

85     returns (uint256 z) {

89     returns (uint256 z) {

276    returns (bool active) {

280     returns (address owner) {

284     returns (address owner) {

454    ) public virtual can_cancel(id) synchronized returns (bool success) {

496    ) external virtual returns (bytes32 id) {

518    ) public virtual can_offer synchronized returns (uint256 id) {

580    ) public view returns (uint256 _amount) {

620    function isClosed() public pure returns (bool closed) {

873    ) public override can_cancel(id) returns (bool success) {

1074    ) external returns (uint256 fill_amt) {

1119    ) external view returns (uint256 fill_amt) {

1128    ) public view returns (uint256 fill_amt) {

1153    ) public view returns (uint256 fill_amt) {

1161    ) public view returns (uint256 fill_amt) {

1282    ) internal returns (uint256 id) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

53    ) external view returns (address buddy) {

142        returns (string memory _name, string memory _symbol, uint8 _decimals)
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/V2Migrator.sol

11    ) external returns (uint256 amountWithdrawn);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

65     returns (uint256 balance) {

86     returns (uint256 rate) {

130    ) internal returns (bool OK) {

258    ) internal returns (uint256 _bathTokenAmount) {

308    ) internal view returns (uint256 _max) {

326    ) internal view returns (uint256 _debt) {

354    ) internal returns (uint256 _bathTokenAmount) {

531    ) internal returns (uint256 _limit, uint256 _lastBorrow) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: /contracts/utilities/FeeWrapper.sol

35        returns (uint256[] memory amountsWithFee, uint256[] memory fees)
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

### Recommendation Code

```solidity
File: /contracts/utilities/FeeWrapper.sol

-35        returns (uint256[] memory amountsWithFee, uint256[] memory fees)

+  returns (uint256[] memory, uint256[] memory)
```

## [G-06]   EXPRESSIONS FOR CONSTANT VALUES SUCH AS A CALL TO KECCAK256(), SHOULD USE IMMUTABLE RATHER THAN CONSTANT

### Details

There are **1** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

232    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232

### Recommendation Code

```solidity
File:
```

## [G-07]  INTERNAL FUNCTIONS ONLY CALLED ONCE CAN BE INLINED TO SAVE GAS

### Summary

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

### Details

There are **22** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

35    function isAuthorized(address src) internal view returns (bool) {

568    function _next_id() internal returns (uint256) {

1225    function _findpos(uint256 id, uint256 pos) internal view returns (uint256) {

1282    ) internal returns (uint256 id) {

1365    ) internal {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

137    function _bathify(
138        address _underlying
139    )
140:        internal
141        view
142        returns (string memory _name, string memory _symbol, uint8 _decimals)

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

258    ) internal returns (uint256 _bathTokenAmount) {

272    function _borrow(address _cToken, uint256 _amount) internal {

280    function _repay(address _asset, address _quote, uint256 _posId) internal {

326    ) internal view returns (uint256 _debt) {

336    function _enterMarkets(address _bathToken) internal {

343    function _exitMarket(address _bathToken) internal {

382    function _exitMarket(address _bathToken) internal {

406    ) internal {

421    function _removePosition(uint256 _positionId) internal {

458    ) internal {

479    ) internal {

531    ) internal returns (uint256 _limit, uint256 _lastBorrow) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: /contracts/utilities/FeeWrapper.sol

62    ) internal returns (bytes memory) {

76    ) internal returns (bytes memory) {

93    function _chargeFee(FeeParams memory _feeParams, address _target) internal {

110    ) internal returns (uint256 _msgValue) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

### Recommendation Code

```solidity
File: /contracts/utilities/FeeWrapper.sol

-93    function _chargeFee(FeeParams memory _feeParams, address _target) internal {

+  function  _chargeFee(FeeParams memory _feeParams, address _target) inline {
```

## [G-08]  Using private rather than public for constants, saves gas

### Summary

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

### Details

There are **2** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

74     uint256 constant WAD = 10 ** 18;

75    uint256 constant RAY = 10 ** 27;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75

### Recommendation Code

```solidity
File: /contracts/RubiconMarket.sol

-74     uint256 constant WAD = 10 ** 18;

+   uint256 private constant WAD = 10 ** 18;
```

## [G-09] FUNCTIONS GUARANTEED TO REVERT WHEN CALLED BY NORMAL USERS CAN BE MARKED PAYABLE

### Summary

The auth,onlyAdmin,onlyOwner modifier makes a function revert if not called by the address registered as the owner

### Details

There are **13** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

628     function stop() external auth {

955    function setMinSell(
956        ERC20 pay_gem, //token to assign minimum sell amount to
957        uint256 dust //maker (ask) minimum sell amount
958:    ) external auth note returns (bool) {


1466    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476    function setFeeTo(address newFeeTo) external auth returns (bool) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

60    function createBathToken(
61        address underlying,
62        InterestRateModel interestRateModel,
63        uint256 initialExchangeRateMantissa,
64        address implementation,
65        bytes memory becomeImplementationData
66:    ) external onlyAdmin {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

191    function notifyRewardAmount(
192       uint256 reward,
193        IERC20 rewardsToken
194:    ) external onlyOwner updateReward(address(0), address(rewardsToken)) {


232    function setRewardsDuration(
233        uint256 _rewardsDuration,
234        address token
235:   ) external onlyOwner {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

93    ) external onlyOwner {

107    ) external onlyOwner {

210    function closePosition(uint256 posId) external onlyOwner {

226    function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242     function withdraw(address token, uint256 amount) external onlyOwner {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

210 function closePosition(uint256 posId) external onlyOwner {

+   function closePosition(uint256 posId) external `payable` onlyOwner {
```

## [G-10] `<X> += <Y>`COSTS MORE GAS THAN`<X> = <X> + <Y>`FOR STATE VARIABLES

### Summary

AVOID COMPOUND ASSIGNMENT OPERATOR IN STATE VARIABLES

Using compound assignment operators for state variables (like State += X or State -= X …) it’s more expensive than using operator assignment (like State = State + X or State = State - X …).

### Details

There are **2** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

184            vars.currentBathTokenAmount += _borrowLoop(

431        positions[_positionId].bathTokenAmount += _bathTokenAmount;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

- 431        positions[_positionId].bathTokenAmount += _bathTokenAmount;

+  positions[_positionId].bathTokenAmount = positions[_positionId].bathTokenAmount + _bathTokenAmount;
```

## [G-11] PUBLIC FUNCTIONS NOT CALLED BY THE CONTRACT SHOULD BE DECLARED EXTERNAL INSTEAD

Contracts are allowed to override their parents' functions and change the visibility from external to public and can save gas by doing so.
public functions not called in the contract itself should be declared externals.

### Details

There are **8** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

290    ) public view returns (uint256, ERC20, uint256, ERC20) {

574    function getFeeBPS() public view returns (uint256) {

624    function getTime() public view returns (uint64) {

1001    ) public view returns (uint256) {

1010    function getFirstUnsortedOffer() public view returns (uint256) {

1016    function getNextUnsortedOffer(uint256 id) public view returns (uint256) {

1153    ) public view returns (uint256 fill_amt) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

47    ) public view returns (address) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

## [G-12] SETTING THE CONSTRUCTOR TO PAYABLE

### Details

There are **2** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/V2Migrator.sol

30    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

54    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {

```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File: /contracts/V2Migrator.sol

- 30    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

+  constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) payable {
```

## [G-13] Use solidity version 0.8.19 to gain some gas boost

### Summary

Upgrade to the latest solidity version 0.8.19 to get additional gas savings.

See latest release for reference: https://blog.soliditylang.org/2023/02/22/solidity-0.8.19-release-announcement/

### Details

There are **2** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

2 pragma solidity ^0.8.9;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2

```solidity
File: /contracts/periphery/BathBuddy.sol

2 pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2

## [G-14] MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE

### Summary

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

### Details

There are **13** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

692    mapping(address => mapping(address => uint256)) public _best;

693    mapping(address => mapping(address => uint256)) public _span;

694    mapping(address => uint256) public _dust;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/BathHouseV2.sol

20    mapping(address => address) private tokenToBathToken;

21    mapping(address => address) private bathTokenToBuddy;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol

```solidity
File: /contracts/V2Migrator.sol

19    mapping(address => address) public v1ToV2Pools;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

52    mapping(address => uint256) public periodFinish; // Token specific

53    mapping(address => uint256) public rewardRates; // Token specific reward rates

54    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

55    mapping(address => uint256) public lastUpdateTime; //Token specific

56    mapping(address => uint256) public rewardsPerTokensStored; // Token specific

59    mapping(address => mapping(address => uint256))

61    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

## [G-15] STATE VARIABLES SHOULD BE CACHED IN STACK VARIABLES RATHER THAN RE-READING THEM FROM STORAGE

### Summary

The instances below point to the second+ access of a state variable within a function.
Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read.
Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.
Most of the times this if statement will be true and we will save 100 gas at a small possibility of 3 gas loss

### Details

There are **27** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

27        emit LogSetOwner(owner);

943                _rank[id].delb < block.number - 10

1049            if (pay_amt >= offers[offerId].buy_amt) {

1051                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

1052                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell

1053                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1058                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1089            if (buy_amt >= offers[offerId].pay_amt) {

1092                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1093                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1100                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

1132            pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay

1143                rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1164            fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator

1165            buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1199            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]

1446        if (_head == id) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-16] STACK VARIABLE USED AS A CHEAPER CACHE FOR A STATE VARIABLE IS ONLY USED ONCE

### Summary

if a state variable used once use instead stack variable

If the variable is only accessed once, it's cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend:

### Details

There are **1** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

594    bool public stopped;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L594

## [G-17]  CAN MAKE THE VARIABLE OUTSIDE THE LOOP TO SAVE GAS

### Summary

Consider making the stack variables before the loop which gonna save gas

### Details

There are **2** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

160             uint256 assetBalance = IERC20(asset).balanceOf(address(this));

545                    uint256 _max = _maxBorrow(_bathToken);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-18] SHOULD USE ARGUMENTS INSTEAD OF STATE VARIABLE

### Summary

state variables should not used in emit

### Details

There are **4** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

27        emit LogSetOwner(owner);

557            pay_gem,

558            buy_gem,
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

418        emit PositionOpened(lastPositionId, pos);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-19] BEFORE SOME FUNCTIONS, WE SHOULD CHECK SOME VARIABLES FOR POSSIBLE GAS SAVE 

### Summary

Before transfer, we should check for amount being 0 so the function doesn't run when its not gonna do anything:

### Details

There are **5** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

143        IERC20(asset).safeTransferFrom(msg.sender, address(this), initMargin);

233        IERC20(asset).safeTransferFrom(msg.sender, address(this), amount);

243        IERC20(token).safeTransfer(msg.sender, amount);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
File: /contracts/utilities/FeeWrapper.sol

100        IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);

102        IERC20(_feeToken).transfer(_feeTo, _feeAmount);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

## [G-20] STATE VARIABLES CAN BE PACKED INTO FEWER STORAGE SLOTS

### Summary

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas).
Reads of the variables are also cheaper.

Consider making this state var near one of the address types (doesnt matter which because its not used in the same functions)

### Details

There are **3** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

219    uint256 public last_offer_id;

675    bool public buyEnabled = true;

676    bool public matchingEnabled = true;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-21] The result of function calls should be cached rather than re-calling the function

### Summary

The instances below point to the second+ call of the function within a single function

### Details

There are **16** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

345        if (makerFee() > 0) {

349            if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

585        if (makerFee() > 0) {

876            if (isOfferSorted(id)) {

1043                mul(pay_amt, 1 ether) <

1044                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

1057                    mul(pay_amt, 10 ** 9),

1058                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1083            if (
                mul(buy_amt, 1 ether) <
                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
            )
1099                        mul(buy_amt, 10 ** 9),

1100                     rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

1186            if (isOfferSorted(id)) {

1190                _hide(id);

1198            isActive(id) &&

1239            if (_isPricedLtOrEq(id, pos)) {

1300            if (
                mul(m_buy_amt, t_buy_amt) >
                mul(t_pay_amt, m_pay_amt) +
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

124        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

393            borrowBalance(_bathTokenAsset) == 0
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-22] REORDER STRUCTURE LAYOUT

### Summary

The following structures could be optimized moving the position of certain values in order to save a lot slots:

https://ethdebug.github.io/solidity-data-representation

Enums are represented by integers; the possibility listed first by 0, the next by 1, and so forth.
An enum type just acts like uintN, where N is the smallest legal value large enough to accomodate all the possibilities.

https://docs.soliditylang.org/en/v0.8.17/internals/layout_in_storage.html#storage-inplace-encoding

### Details

There are **1** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

33    struct Position {
34        address asset; // supplied as collateral
35        address quote; // borrowed token
36        uint256 borrowedAmount; // amount of borrowed quote
37        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
38        uint256 blockNum; // block number on which position was opened
39        bool isActive; // false by default, when active - true
40    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File:

33    struct Position {
34        address asset; // supplied as collateral
35        address quote; // borrowed token
36        bool isActive; // false by default, when active - true
37        uint256 borrowedAmount; // amount of borrowed quote
38        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
39        uint256 blockNum; // block number on which position was opened
40            }
```

## [G-23] STATE VARIABLES ONLY SET IN THE CONSTRUCTOR SHOULD BE DECLARED IMMUTABLE

### Summary

Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

While strings are not value types, and therefore cannot be immutable/constant if not hard-coded outside of the constructor, the same behavior can be achieved by making the current contract abstract with virtual functions for the string accessors, and having a child contract override the functions with the hard-coded implementation-specific values.

### Details

There are **4** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

55        oracle = PriceOracle(_oracle);
56        rubiconMarket = RubiconMarket(_rubiconMarket);
57        bathHouseV2 = BathHouseV2(_bathHouseV2);
58        comptroller = bathHouseV2.comptroller();
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-24] Duplicated require()/if() checks should be refactored to a modifier or function

### Details

There are **24** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

345        if (makerFee() > 0) {
585        if (makerFee() > 0) {

876             if (isOfferSorted(id)) {
1186            if (isOfferSorted(id)) {


246         require(isActive(id));
252         require(isActive(id));
604         require(isActive(id));
611         require(isActive(id));
1366        require(isActive(id));


265         require(!locked);
938         require(!locked);
1034        require(!locked);
1075        require(!locked);


598        require(!isClosed());
605        require(!isClosed());


565        require(buy(uint256(id), maxTakeAmount));
753        require(buy(uint256(id), maxTakeAmount));

488        require(cancel(uint256(id)));
757        require(cancel(uint256(id)));

834        require(!locked, "Reentrancy attempt");
859        require(!locked, "Reentrancy attempt");
874        require(!locked, "Reentrancy attempt");

1209        require(id > 0);
1226        require(id > 0);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

### Recommendation Code

```solidity

modifer check (uint256 id) {
    require(isActive(id));
    _;
}
```

## [G-25] Multiple accesses of a mapping/array should use a local variable cache

### Summary

The instances below point to the second+ access of a value inside a mapping/array, within a function. Caching a mapping’s value in a local storage or calldata variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations. Caching an array’s struct avoids recalculating the array offsets into memory/calldata.

### Details

There are **34** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

436        if (offers[id].pay_amt == 0) {

943                _rank[id].delb < block.number - 10

1045                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

1049            if (pay_amt >= offers[offerId].buy_amt) {

1051                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

1052                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell

1053                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1058                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1085                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1089            if (buy_amt >= offers[offerId].pay_amt) {

1091                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator

1092                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1093                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1100                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)

1131            fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator

1143                rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1164            fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator

1199            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]

1230            pos = _rank[pos].prev;

1266            mul(offers[low].buy_amt, offers[high].pay_amt) >=

1267            mul(offers[high].buy_amt, offers[low].pay_amt);

1292            m_pay_amt = offers[best_maker_id].pay_amt;

1372        pos = pos == 0 ||

1384            _rank[pos].prev = id;

1385            _rank[id].next = pos;

1419            require(_rank[_rank[id].next].prev == id);

1420            _rank[_rank[id].next].prev = _rank[id].prev;

1423            _best[pay_gem][buy_gem] = _rank[id].prev;

1426        if (_rank[id].prev != 0) {

1428            require(_rank[_rank[id].prev].next == id);

1429            _rank[_rank[id].prev].next = _rank[id].next;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-26] A modifier used only once and not being inherited should be inlined to save gas

### Summary

### Details

There are **1** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

643    modifier note() {
644        bytes32 foo;
645        bytes32 bar;
646        uint256 wad;
647
648        assembly {
649            foo := calldataload(4)
650            bar := calldataload(36)
651            wad := callvalue()
652        }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L643

The above modifer is only used in the following

```solidity
File: /contracts/RubiconMarket.sol

955    function setMinSell(
956        ERC20 pay_gem, //token to assign minimum sell amount to
957        uint256 dust //maker (ask) minimum sell amount
958:    ) external auth note returns (bool) {
959        _dust[address(pay_gem)] = dust;
960        emit LogMinSell(address(pay_gem), dust);
961        return true;
962    }
```

## [G-27] Use Modifiers Instead of Functions To Save Gas

### Summary

This with 0.8.9 compiler and optimization enabled. it will save you about 30 gas. But sometimes modifiers increase code size of the contract.

### Details

There are **9** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

46     function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x + y) >= x, "ds-math-add-overflow");
    }

50    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x - y) <= x, "ds-math-sub-underflow");
    }

54    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");
    }

487    function kill(bytes32 id) external virtual {
        require(cancel(uint256(id)));
    }

564    function take(bytes32 id, uint128 maxTakeAmount) external virtual {
        require(buy(uint256(id), maxTakeAmount));
    }

752    function take(bytes32 id, uint128 maxTakeAmount) public override {
        require(buy(uint256(id), maxTakeAmount));
    }

756    function kill(bytes32 id) external override {
        require(cancel(uint256(id)));
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

272    function _borrow(address _cToken, uint256 _amount) internal {
        require(
            CErc20Interface(_cToken).borrow(_amount) == 0,
            "_borrow: BORROW FAILED"
        );
    }

343    function _exitMarket(address _bathToken) internal {
        require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

- 343     function _exitMarket(address _bathToken) internal {
        require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");
     }


+       modifier _exitMarket(address _bathTokwn) {
+            require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");
+                _;
+           }
```

## [G-28] Help the optimizer by saving a storage variable's reference instead of repeatedly fetching it

### Summary

To help the optimizer, declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. The effect can be quite significant.
As an example, instead of repeatedly calling someMap[someIndex], save its reference like this: SomeStruct storage someStruct = someMap[someIndex] and use it.
Instances include:

### Details

There are **9** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol#

436         if (offers[id].pay_amt == 0) {

1049            if (pay_amt >= offers[offerId].buy_amt) {

1085                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)

1089            if (buy_amt >= offers[offerId].pay_amt) {

1185        if (amount == offers[id].pay_amt) {

1199            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]

1417        if (id != _best[pay_gem][buy_gem]) {

1426        if (_rank[id].prev != 0) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#

```solidity
File: /contracts/periphery/BathBuddy.sol

195        if (block.timestamp >= periodFinish[address(rewardsToken)]) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

## [G-29] Using > 0 costs more gas than != 0 when used on a uint in a require()

### Summary

> 0 is less efficient than != 0 for unsigned integers.
> != 0 costs less gas compared to > 0 for unsigned integers in require and assert statements with the optimizer enabled (6 gas).

### Details

There are **7** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol


521        require(pay_amt > 0);

523        require(buy_amt > 0);

1209        require(id > 0);

1226        require(id > 0);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/V2Migrator.sol

41        require(bathBalance > 0, "migrate: ZERO AMOUNT");
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sols

313        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

318        require(_max > 0, "_maxBorrow: can't borrow 0");
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-30]  Copying struct to memory can be more expensive than just reading from storage

### Summary

I suggest using the storage keyword instead of the memory one, as the copy in memory is wasting some MSTOREs and MLOADs.

### Details

There are **7** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

291        OfferInfo memory _offer = offers[id];

318        OfferInfo memory _offer = offers[id];

455        OfferInfo memory _offer = offers[id];
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

75        Position memory pos = positions[posId];

211        Position memory pos = positions[posId];

227        Position memory pos = positions[posId];

339        uint256[] memory _errs = comptroller.enterMarkets(_bathTokens);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-31] block.timeStamp directly call instead of a function

### Details

There are **1** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

624:     function getTime() public view returns (uint64) {
        return uint64(block.timestamp);
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624

## [G-32]  require() or revert() statements that check input arguments should be at the top of the function

### Details

There are **2** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

322        require(uint128(quantity) == quantity, "quantity is not an int");

835        require(_dust[address(pay_gem)] <= pay_amt);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-33] Use double require instead of using &&

### Summary

Instead of using operator && on a single require. Using a two require can save more gas.

### Details

There are **13** instances of this issue.

### Github Permalinks

```solidity
File: /contracts/RubiconMarket.sol

253        require(
            (msg.sender == getOwner(id)) ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );

612        require(
            (msg.sender == getOwner(id)) ||
                isClosed() ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
        );

725        require(
            isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );

893        require(
            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
            "Array lengths do not match"
        );

940     require(
            !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );

1412        require(
            _rank[id].delb == 0 && //assert id is in the sorted list
                isOfferSorted(id)
        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
File: /contracts/periphery/BathBuddy.sol

95        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

591            ? require(
                _leverage > _wad && _leverage <= _leverageMax,
                "_leverageCheck{Long}: INVLAID LEVERAGE"
            )
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

### Recommendation Code

```solidity
File: /contracts/periphery/BathBuddy.sol

95        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );

+   require(msg.sender == myBathTokenBuddy)
+   require(msg.sender != address(0))
+   require(friendshipStarted)

```
