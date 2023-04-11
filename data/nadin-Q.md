## Low Risk Issues List
### Issue 
## [L-01] getReward() should be usable while the BathBuddy contract is paused 
## [L-02] Unsafe use of transfer()/transferFrom() with IERC20 ( 04 instances )
### Total:  02 issues
## [L-01] getReward() should be usable while the BathBuddy contract is paused 
### Description:
The BathBuddy.getReward function is only callable when the BathBuddy contract is not paused. This prevents a holderRecipient from get all rewards accrued for provided token. The admin of the BathBuddy  contract can potentially pause the contracts at any time, locking users out of their honestly earned funds.
```
File: contracts/periphery/BathBuddy.sol
    function getReward(
        IERC20 rewardsToken,
        address holderRecipient
    )
        external
        override
        nonReentrant
        whenNotPaused
        updateReward(holderRecipient, address(rewardsToken))
        onlyBathHouse
    {
        uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
        if (reward > 0) {
            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
            rewardsToken.safeTransfer(holderRecipient, reward);
            emit RewardPaid(holderRecipient, reward);
        }
    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L168-L185)

### Recommended mitigation steps:
Consider removing the whenNotPaused modifier to allow collecting funds while the BathBuddy contract is paused.

## [L-02] Unsafe use of transfer()/transferFrom() with IERC20 ( 04 instances )
Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens. For example Tether (USDT)'s transfer() and transferFrom() functions do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to IERC20, their function signatures do not match and therefore the calls made, revert. Use OpenZeppelin's SafeERC20's safeTransfer()/safetransferFrom() instead
```
File: contracts/V2Migrator.sol
        IERC20(bathTokenV2).transfer(
            msg.sender,
            IERC20(bathTokenV2).balanceOf(address(this))
        );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59-L62)

```
File: contracts/utilities/poolsUtility/Position.sol
            IERC20(_asset).transferFrom(
                msg.sender,
                address(this),
                _payAmount.sub(_assetBalance).add(_fee)
            );
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L493-L497)

```
File: contracts/utilities/FeeWrapper.sol
100:        IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);
102:        IERC20(_feeToken).transfer(_feeTo, _feeAmount);
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L100-L102)

## Non-Critical Issues List
### Issue 
## [N-01] USE OF BYTES.CONCAT() INSTEAD OF ABI.ENCODEPACKED(,) ( 06 instances)
## [N-02] USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONSTANTS ( 03 instances )
## [N-03] FOR MODERN AND MORE READABLE CODE; UPDATE IMPORT USAGES ( All Contracts)
## [N-04] Include return parameters in NatSpec comments ( All Contracts )
## [N-05] NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS
## [N-06] Constants in comparisons should appear on the left side ( 25 instances)
## [N-07] Use standard decimals interpretations using 1eX ( 12 instances)
## [N-08] Use bytes.concat() INSTEAD OF String ( 04 instances)
## [N-09] Missing Event for initialize ( 05 instances )
## [N-10] Use SMTChecker
## [N-11] Mark visibility of initialize(...) functions as external ( 01 instances)
## [N-12] Large or complicated code bases should implement fuzzing tests
## [N-13] Deprecated library used for Solidity >= 0.8 : SafeMath
## [N-14] Solidity compiler optimizations can be problematic (01 instances)
### Total:  14 issues

## [N-01] USE OF BYTES.CONCAT() INSTEAD OF ABI.ENCODEPACKED(,) ( 06 instances)
Rather than using abi.encodePacked for appending bytes, since version 0.8.4, bytes.concat() is enabled
Since version 0.8.4 for appending bytes, bytes.concat() can be used instead of abi.encodePacked(,).
```
File: contracts/RubiconMarket.sol
369:                keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
400:            keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
423:            keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
442:                keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
476:            keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
555:            keccak256(abi.encodePacked(pay_gem, buy_gem)),
```
[369](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L369)
[400](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L400)
[423](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L423)
[442](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L442)
[476](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L476)
[555](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L555)

## [N-02] USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONSTANTS ( 03 instances )
There are many addresses and constants used in the system. It is recommended to put the most used ones in one file (for example constants.sol, use inheritance to access these values):

This will help with readability and easier maintenance for future changes.
constants.sol
Use and import this file in contracts that require access to these values. This is just a suggestion, in some use cases this may result in higher gas usage in the distribution
```
File: contracts/RubiconMarket.sol
74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;
232:    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
[74](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74)
[75](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L75)
[232](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L232)

## [N-03] FOR MODERN AND MORE READABLE CODE; UPDATE IMPORT USAGES ( All Contracts)
### Description:
Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct polluted the source code with an unnecessary object we were not using because we did not need it.
This was breaking the rule of modularity and modular programming: only import what you need Specific imports with curly braces allow us to apply this rule better.
### Recommendation:
import {contract1 , contract2} from "filename.sol";
A good example from the ArtGobblers project;
```
import {Owned} from "solmate/auth/Owned.sol";
import {ERC721} from "solmate/tokens/ERC721.sol";
import {LibString} from "solmate/utils/LibString.sol";
import {MerkleProofLib} from "solmate/utils/MerkleProofLib.sol";
import {FixedPointMathLib} from "solmate/utils/FixedPointMathLib.sol";
import {ERC1155, ERC1155TokenReceiver} from "solmate/tokens/ERC1155.sol";
import {toWadUnsafe, toDaysWadUnsafe} from "solmate/utils/SignedWadMath.sol";
```

## [N-04] Include return parameters in NatSpec comments ( All Contracts )
### Description
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html
### Recommendation
Include return parameters in NatSpec comments

## [N-05] NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html
Recommendation:
NatSpec comments should be increased in contracts

## [N-06] Constants in comparisons should appear on the left side ( 25 instances)
Doing so will prevent [typo bugs](https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html).
```
File: contracts/RubiconMarket.sol
55:        require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");
326:            quantity == 0 ||
327:            spend == 0 ||
436:        if (offers[id].pay_amt == 0) {
1233:        if (pos == 0) {
1319:            if (t_pay_amt == 0 || t_buy_amt == 0) {
1372:        pos = pos == 0 ||
1413:            _rank[id].delb == 0 && //assert id is in the sorted list
```
[55](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L55)
[326](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L326)
[327](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L327)
[436](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L436)
[1233](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1233)
[1319](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1319)
[1372](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1372)
[1413](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1413)

```
File: contracts/V2Migrator.sol
55:            CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
64:            IERC20(bathTokenV2).balanceOf(address(this)) == 0,
```
[55](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L55)
[64](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L64)

```
File: contracts/periphery/BathBuddy.sol
124:        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
```
[124](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L124)

```
File: contracts/utilities/poolsUtility/Position.sol
163:            if (vars.initAssetBalance == 0) {
274:            CErc20Interface(_cToken).borrow(_amount) == 0,
299:            CErc20Interface(_bathTokenQuote).repayBorrow(_amountToRepay) == 0,
312:        require(_err == 0, "_maxBorrow: ERROR");
313:        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
314:        require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");
340:        require(_errs[0] == 0);
344:        require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");
360:            CErc20Interface(_bathToken).mint(_amount) == 0,
386:            CErc20Interface(_bathTokenAsset).redeem(_bathTokenAmount) == 0,
392:            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
393:            borrowBalance(_bathTokenAsset) == 0
542:            if (_limit == 0) {
```
[163](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L163)
[274](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L274)
[299](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L299)
[312-314](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L312-L314)
[340](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L340)
[344](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L344)
[360](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L360)
[386](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L386)
[392-393](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L392-L393)
[542](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L542)

```
File: contracts/utilities/FeeWrapper.sol
51:        if (msg.value == 0) {
```
[51](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L51)

## [N-07] Use standard decimals interpretations using 1eX ( 12 instances)
Use 1eX instead of 1**X
```
File: RubiconMarket.sol
74:    uint256 constant WAD = 10 ** 18;
75:    uint256 constant RAY = 10 ** 27;
1057:                    mul(pay_amt, 10 ** 9),
1059:                ) / 10 ** 9;
1099:                        mul(buy_amt, 10 ** 9),
1101:                    ) / 10 ** 9
1142:                mul(pay_amt, 10 ** 9),
1144:            ) / 10 ** 9
1175:                mul(buy_amt, 10 ** 9),
1177:            ) / 10 ** 9
```
[74-75](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74-L75)
[1057](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1057)
[1059](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1059)
[1099](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1099)
[1101](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1101)
[1142](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1142)
[1144](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1144)
[1175](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1175)
[1177](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1177)

```
File: contracts/utilities/poolsUtility/Position.sol
317:        _max = (_liq.mul(10 ** 18)).div(_price);
331:        ).div(10 ** 18);
```
[317](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317)
[331](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L331)

## [N-08] Use bytes.concat() INSTEAD OF String ( 04 instances)
### Description
Since Solidity version 0.8.4 introduces bytes.concat() (vs abi.encodePacked(<bytes>,<bytes>))
```
File: contracts/BathHouseV2
82:        (string memory name, string memory symbol, uint8 decimals) = _bathify(
142:        returns (string memory _name, string memory _symbol, uint8 _decimals)
146:        _name = string.concat("bath", ERC20(_underlying).symbol());
147:        _symbol = string.concat(_name, "v2");
```
[82](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L82)
[142](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L142)
[146-147](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L146-L147)

## [N-09] Missing Event for initialize ( 05 instances )
### Description: Events help non-contract tools to track changes, and events prevent users from being surprised by changes Issuing event-emit during initialization is a detail that many projects skip
```
File: contracts/RubiconMarket.sol
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
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700-L716)

```
File: contracts/BathHouseV2.sol
    function initialize(address _comptroller, address _pAdmin) external {
        require(!initialized, "BathHouseV2 already initialized!");
        comptroller = Comptroller(_comptroller);
        admin = msg.sender;
        proxyAdmin = _pAdmin;


        initialized = true;
    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32-L39)

```
File: contracts/V2Migrator.sol
    constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
        for (uint256 i = 0; i < bathTokensV1.length; ++i) {
            // set v1 to v2 bathTokens
            v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
        }
    }
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30-L35)

```
File: contracts/periphery/BathBuddy.sol
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
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71-L85)

```
File: contracts/utilities/poolsUtility/Position.sol
    constructor(address _oracle, address _rubiconMarket, address _bathHouseV2) {
        oracle = PriceOracle(_oracle);
        rubiconMarket = RubiconMarket(_rubiconMarket);
        bathHouseV2 = BathHouseV2(_bathHouseV2);
        comptroller = bathHouseV2.comptroller();
    }
```
[Lik to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54-L59)
### Recommendation: Add Event-Emit

## [N-10] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.
https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19

## [N-11] Mark visibility of initialize(...) functions as external ( 01 instances)
It is good practice to mark functions as external instead of public if they are not called by the contract where they are defined.
```
File: contracts/RubiconMarket.sol
700:    function initialize(address _feeTo) public {
```
[700](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700)

## [N-12] Large or complicated code bases should implement fuzzing tests
Large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts, should implement [fuzzing tests](https://medium.com/coinmonks/smart-contract-fuzzing-d9b88e0b0a05). Fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold. Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and fuzzers, with properly and extensively-written invariants, can close this testing gap significantly.

## [N-13] Deprecated library used for Solidity >= 0.8 : SafeMath
```
File: contracts/periphery/BathBuddy.sol
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

File: contracts/utilities/poolsUtility/Position.sol
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```
[BathBuddy.sol#6](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L6)
[Position.sol#6](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L6)

## [N-14] Solidity compiler optimizations can be problematic (01 instances)
```
File: hardhat.config.ts
    compilers: [
      {
        version: "0.8.17",
        settings: {
          optimizer: {
            enabled: true,
            runs: 5,
          },
```
[Link to code](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/hardhat.config.ts#L12-L19)

### Description: Protocol has enabled optional compiler optimizations in Solidity. There have been several optimization bugs with security implications. Moreover, optimizations are actively being developed. Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them.

Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past. A high-severity bug in the emscripten-generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG.

Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. More recently, another bug due to the incorrect caching of keccak256 was reported. A compiler audit of Solidity from November 2018 concluded that the optional optimizations may not be safe. It is likely that there are latent bugs related to optimization and that new bugs will be introduced due to future optimizations.

Exploit Scenario A latent or future bug in Solidity compiler optimizations—or in the Emscripten transpilation to solc-js—causes a security vulnerability in the contracts.

### Recommendation: Short term, measure the gas savings from optimizations and carefully weigh them against the possibility of an optimization-related bug. Long term, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.

## [S-02] Include the project name and development team information in the contract to increase the popularity and trust of users in the project
### Recommendation: Use form like FraxFinance project
https://github.com/FraxFinance/frxETH-public/blob/7f7731dbc93154131aba6e741b6116da05b25662/src/sfrxETH.sol#L4-L24