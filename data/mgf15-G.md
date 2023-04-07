## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use assembly to check for `address(0)` | 14 |
| [GAS-2](#GAS-2) | Using bools for storage incurs overhead | 7 |
| [GAS-3](#GAS-3) | Cache array length outside of loop | 6 |
| [GAS-4](#GAS-4) | State variables should be cached in stack variables rather than re-reading them from storage | 2 |
| [GAS-5](#GAS-5) | Use calldata instead of memory for function arguments that do not get mutated | 7 |
| [GAS-6](#GAS-6) | For Operations that will not overflow, you could use unchecked | 450 |
| [GAS-7](#GAS-7) | Use Custom Errors | 36 |
| [GAS-8](#GAS-8) | Don't initialize variables with default value | 8 |
| [GAS-9](#GAS-9) | Long revert strings | 7 |
| [GAS-10](#GAS-10) | Functions guaranteed to revert when called by normal users can be marked `payable` | 3 |
| [GAS-11](#GAS-11) | `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too) | 9 |
| [GAS-12](#GAS-12) | Use shift Right/Left instead of division/multiplication if possible | 4 |
| [GAS-13](#GAS-13) | Use != 0 instead of > 0 for unsigned integer comparison | 21 |
| [GAS-14](#GAS-14) | `internal` functions not called by the contract should be removed | 4 |
### [GAS-1] Use assembly to check for `address(0)`
*Saves 6 gas per instance*

*Instances (14)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

69:             tokenToBathToken[underlying] == address(0),

73:             underlying != address(0),

77:             implementation != address(0),

144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

255:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

459:         _offer.owner == address(0) && msg.sender == _offer.recipient

615:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0))

702:         require(_feeTo != address(0));

725:                 (msg.sender == getRecipient(id) && getOwner(id) == address(0)),

1477:         require(newFeeTo != address(0));

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

97:                 msg.sender != address(0) &&

250:         if (account != address(0)) {

```

### [GAS-2] Using bools for storage incurs overhead
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (7)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

18:     bool private initialized;

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

594:     bool public stopped;

675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!

676:     bool public matchingEnabled = true; //true: enable matching,

679:     bool public initialized;

682:     bool public AqueductDistributionLive;

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

49:     bool public friendshipStarted;

```

### [GAS-3] Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (6)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

122:         for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```

### [GAS-4] State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Saves 100 gas per instance*

*Instances (2)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

422:             feeTo,

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

133:                     .div(IERC20(myBathTokenBuddy).totalSupply())

```

### [GAS-5] Use calldata instead of memory for function arguments that do not get mutated
Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage.

*Instances (7)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

65:         bytes memory becomeImplementationData

116:         address[] memory buddies,

117:         address[] memory rewardsTokens

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

29:         uint256[] memory tokenAmounts,

49:         CallParams memory params

```

### [GAS-6] For Operations that will not overflow, you could use unchecked

*Instances (450)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "./compound-v2-fork/InterestRateModel.sol";

5: import "./compound-v2-fork/InterestRateModel.sol";

5: import "./compound-v2-fork/InterestRateModel.sol";

5: import "./compound-v2-fork/InterestRateModel.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

6: import "./compound-v2-fork/CErc20Delegator.sol";

7: import "./compound-v2-fork/Comptroller.sol";

7: import "./compound-v2-fork/Comptroller.sol";

7: import "./compound-v2-fork/Comptroller.sol";

7: import "./compound-v2-fork/Comptroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

8: import "./compound-v2-fork/Unitroller.sol";

9: import "./periphery/BathBuddy.sol";

9: import "./periphery/BathBuddy.sol";

122:         for (uint256 i = 0; i < buddies.length; ++i) {

122:         for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

31:         require(isAuthorized(msg.sender), "ds-auth-unauthorized");

31:         require(isAuthorized(msg.sender), "ds-auth-unauthorized");

47:         require((z = x + y) >= x, "ds-math-add-overflow");

47:         require((z = x + y) >= x, "ds-math-add-overflow");

47:         require((z = x + y) >= x, "ds-math-add-overflow");

47:         require((z = x + y) >= x, "ds-math-add-overflow");

51:         require((z = x - y) <= x, "ds-math-sub-underflow");

51:         require((z = x - y) <= x, "ds-math-sub-underflow");

51:         require((z = x - y) <= x, "ds-math-sub-underflow");

51:         require((z = x - y) <= x, "ds-math-sub-underflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

74:     uint256 constant WAD = 10 ** 18;

74:     uint256 constant WAD = 10 ** 18;

75:     uint256 constant RAY = 10 ** 27;

75:     uint256 constant RAY = 10 ** 27;

78:         z = add(mul(x, y), WAD / 2) / WAD;

78:         z = add(mul(x, y), WAD / 2) / WAD;

82:         z = add(mul(x, y), RAY / 2) / RAY;

82:         z = add(mul(x, y), RAY / 2) / RAY;

86:         z = add(mul(x, WAD), y / 2) / y;

86:         z = add(mul(x, WAD), y / 2) / y;

90:         z = add(mul(x, RAY), y / 2) / y;

90:         z = add(mul(x, RAY), y / 2) / y;

319:         uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

338:         uint256 fee = mul(spend, feeBPS) / 100_000;

346:             uint256 mFee = mul(spend, makerFee()) / 100_000;

376:             "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"

522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

522:         require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

524:         require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6

569:         last_offer_id++;

569:         last_offer_id++;

583:         _amount -= mul(amount, feeBPS) / 100_000;

583:         _amount -= mul(amount, feeBPS) / 100_000;

586:             _amount -= mul(amount, makerFee()) / 100_000;

586:             _amount -= mul(amount, makerFee()) / 100_000;

675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!

675:     bool public buyEnabled = true; //buy enabled TODO: review this decision!

676:     bool public matchingEnabled = true; //true: enable matching,

676:     bool public matchingEnabled = true; //true: enable matching,

687:         uint256 next; //points to id of next higher offer

687:         uint256 next; //points to id of next higher offer

688:         uint256 prev; //points to id of previous lower offer

688:         uint256 prev; //points to id of previous lower offer

689:         uint256 delb; //the blocknumber where this entry was marked for delete

689:         uint256 delb; //the blocknumber where this entry was marked for delete

691:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

691:     mapping(uint256 => sortInfo) public _rank; //doubly linked lists of sorted offer ids

692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

692:     mapping(address => mapping(address => uint256)) public _best; //id of the highest offer for a token pair

693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

693:     mapping(address => mapping(address => uint256)) public _span; //number of offers stored for token pair in sorted orderbook

694:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers

694:     mapping(address => uint256) public _dust; //minimum sell amount for a token to avoid dust offers

695:     mapping(uint256 => uint256) public _near; //next unsorted offer id

695:     mapping(uint256 => uint256) public _near; //next unsorted offer id

696:     uint256 public _head; //first unsorted offer id

696:     uint256 public _head; //first unsorted offer id

697:     uint256 public dustId; // id of the latest offer marked as dust

697:     uint256 public dustId; // id of the latest offer marked as dust

762:         uint256 pay_amt, //maker (ask) sell how much

762:         uint256 pay_amt, //maker (ask) sell how much

763:         ERC20 pay_gem, //maker (ask) sell which token

763:         ERC20 pay_gem, //maker (ask) sell which token

764:         uint256 buy_amt, //maker (ask) buy how much

764:         uint256 buy_amt, //maker (ask) buy how much

765:         ERC20 buy_gem //maker (ask) buy which token

765:         ERC20 buy_gem //maker (ask) buy which token

781:         uint256 pay_amt, //maker (ask) sell how much

781:         uint256 pay_amt, //maker (ask) sell how much

782:         ERC20 pay_gem, //maker (ask) sell which token

782:         ERC20 pay_gem, //maker (ask) sell which token

783:         uint256 buy_amt, //maker (ask) buy how much

783:         uint256 buy_amt, //maker (ask) buy how much

784:         ERC20 buy_gem, //maker (ask) buy which token

784:         ERC20 buy_gem, //maker (ask) buy which token

785:         uint pos, //position to insert offer, 0 should be used if unknown

785:         uint pos, //position to insert offer, 0 should be used if unknown

803:         uint256 pay_amt, //maker (ask) sell how much

803:         uint256 pay_amt, //maker (ask) sell how much

804:         ERC20 pay_gem, //maker (ask) sell which token

804:         ERC20 pay_gem, //maker (ask) sell which token

805:         uint256 buy_amt, //maker (ask) buy how much

805:         uint256 buy_amt, //maker (ask) buy how much

806:         ERC20 buy_gem, //maker (ask) buy which token

806:         ERC20 buy_gem, //maker (ask) buy which token

807:         uint256 pos, //position to insert offer, 0 should be used if unknown

807:         uint256 pos, //position to insert offer, 0 should be used if unknown

825:         uint256 pay_amt, //maker (ask) sell how much

825:         uint256 pay_amt, //maker (ask) sell how much

826:         ERC20 pay_gem, //maker (ask) sell which token

826:         ERC20 pay_gem, //maker (ask) sell which token

827:         uint256 buy_amt, //maker (ask) buy how much

827:         uint256 buy_amt, //maker (ask) buy how much

828:         ERC20 buy_gem, //maker (ask) buy which token

828:         ERC20 buy_gem, //maker (ask) buy which token

829:         uint256 pos, //position to insert offer, 0 should be used if unknown

829:         uint256 pos, //position to insert offer, 0 should be used if unknown

830:         bool matching, //match "close enough" orders?

830:         bool matching, //match "close enough" orders?

831:         address owner, // owner of the offer

831:         address owner, // owner of the offer

832:         address recipient // recipient of the offer's fill

832:         address recipient // recipient of the offer's fill

882:         return super.cancel(id); //delete the offer.

882:         return super.cancel(id); //delete the offer.

899:         for (uint i = 0; i < payAmts.length; i++) {

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

943:                 _rank[id].delb < block.number - 10

956:         ERC20 pay_gem, //token to assign minimum sell amount to

956:         ERC20 pay_gem, //token to assign minimum sell amount to

957:         uint256 dust //maker (ask) minimum sell amount

957:         uint256 dust //maker (ask) minimum sell amount

966:         ERC20 pay_gem //token for which minimum sell amount is queried

966:         ERC20 pay_gem //token for which minimum sell amount is queried

1039:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1039:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1040:             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1040:             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1047:                 break; //We consider that all amount is sold

1047:                 break; //We consider that all amount is sold

1051:                 fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

1051:                 fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator

1052:                 pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell

1052:                 pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell

1053:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1053:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1057:                     mul(pay_amt, 10 ** 9),

1057:                     mul(pay_amt, 10 ** 9),

1059:                 ) / 10 ** 9;

1059:                 ) / 10 ** 9;

1059:                 ) / 10 ** 9;

1060:                 fill_amt = add(fill_amt, baux); //Add amount bought to acumulator

1060:                 fill_amt = add(fill_amt, baux); //Add amount bought to acumulator

1061:                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

1061:                 take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need

1062:                 pay_amt = 0; //All amount is sold

1062:                 pay_amt = 0; //All amount is sold

1079:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1079:             offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair

1087:                 break; //We consider that all amount is sold

1087:                 break; //We consider that all amount is sold

1091:                 fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator

1091:                 fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator

1092:                 buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1092:                 buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1093:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1093:                 take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer

1099:                         mul(buy_amt, 10 ** 9),

1099:                         mul(buy_amt, 10 ** 9),

1101:                     ) / 10 ** 9

1101:                     ) / 10 ** 9

1101:                     ) / 10 ** 9

1102:                 ); //Add amount sold to acumulator

1102:                 ); //Add amount sold to acumulator

1103:                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

1103:                 take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need

1104:                 buy_amt = 0; //All amount is bought

1104:                 buy_amt = 0; //All amount is bought

1129:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1129:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1131:             fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator

1131:             fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount to buy accumulator

1132:             pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay

1132:             pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to pay

1135:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1135:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1136:                 require(offerId != 0); //Fails if there are not enough offers to complete

1136:                 require(offerId != 0); //Fails if there are not enough offers to complete

1142:                 mul(pay_amt, 10 ** 9),

1142:                 mul(pay_amt, 10 ** 9),

1144:             ) / 10 ** 9

1144:             ) / 10 ** 9

1144:             ) / 10 ** 9

1145:         ); //Add proportional amount of last offer to buy accumulator

1145:         ); //Add proportional amount of last offer to buy accumulator

1162:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1162:         uint256 offerId = getBestOffer(buy_gem, pay_gem); //Get best offer for the token pair

1164:             fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator

1164:             fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount to pay accumulator

1165:             buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1165:             buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy

1168:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1168:                 offerId = getWorseOffer(offerId); //We look for the next best offer

1169:                 require(offerId != 0); //Fails if there are not enough offers to complete

1169:                 require(offerId != 0); //Fails if there are not enough offers to complete

1175:                 mul(buy_amt, 10 ** 9),

1175:                 mul(buy_amt, 10 ** 9),

1177:             ) / 10 ** 9

1177:             ) / 10 ** 9

1177:             ) / 10 ** 9

1178:         ); //Add proportional amount of last offer to pay accumulator

1178:         ); //Add proportional amount of last offer to pay accumulator

1201:             dustId = id; //enable current msg.sender to call cancel(id)

1201:             dustId = id; //enable current msg.sender to call cancel(id)

1262:         uint256 low, //lower priced offer's id

1262:         uint256 low, //lower priced offer's id

1263:         uint256 high //higher priced offer's id

1263:         uint256 high //higher priced offer's id

1274:         uint256 t_pay_amt, //taker sell how much

1274:         uint256 t_pay_amt, //taker sell how much

1275:         ERC20 t_pay_gem, //taker sell which token

1275:         ERC20 t_pay_gem, //taker sell which token

1276:         uint256 t_buy_amt, //taker buy how much

1276:         uint256 t_buy_amt, //taker buy how much

1277:         ERC20 t_buy_gem, //taker buy which token

1277:         ERC20 t_buy_gem, //taker buy which token

1278:         uint256 pos, //position id

1278:         uint256 pos, //position id

1279:         bool rounding, //match "close enough" orders?

1279:         bool rounding, //match "close enough" orders?

1283:         uint256 best_maker_id; //highest maker id

1283:         uint256 best_maker_id; //highest maker id

1284:         uint256 t_buy_amt_old; //taker buy how much saved

1284:         uint256 t_buy_amt_old; //taker buy how much saved

1285:         uint256 m_buy_amt; //maker offer wants to buy this much token

1285:         uint256 m_buy_amt; //maker offer wants to buy this much token

1286:         uint256 m_pay_amt; //maker offer wants to sell this much token

1286:         uint256 m_pay_amt; //maker offer wants to sell this much token

1302:                 mul(t_pay_amt, m_pay_amt) +

1305:                             ? m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt

1305:                             ? m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt

1305:                             ? m_buy_amt + t_buy_amt + t_pay_amt + m_pay_amt

1317:             t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt_old;

1363:         uint256 id, //maker (ask) id

1363:         uint256 id, //maker (ask) id

1364:         uint256 pos //position to insert into

1364:         uint256 pos //position to insert into

1370:         uint256 prev_id; //maker (ask) id

1370:         uint256 prev_id; //maker (ask) id

1400:         _span[address(pay_gem)][address(buy_gem)]++;

1400:         _span[address(pay_gem)][address(buy_gem)]++;

1406:         uint256 id //id of maker (ask) offer to remove from sorted list

1406:         uint256 id //id of maker (ask) offer to remove from sorted list

1413:             _rank[id].delb == 0 && //assert id is in the sorted list

1413:             _rank[id].delb == 0 && //assert id is in the sorted list

1432:         _span[pay_gem][buy_gem]--;

1432:         _span[pay_gem][buy_gem]--;

1433:         _rank[id].delb = block.number; //mark _rank[id] for deletion

1433:         _rank[id].delb = block.number; //mark _rank[id] for deletion

1439:         uint256 id //id of maker offer to remove from unsorted list

1439:         uint256 id //id of maker offer to remove from unsorted list

1441:         uint256 uid = _head; //id of an offer in unsorted offers list

1441:         uint256 uid = _head; //id of an offer in unsorted offers list

1442:         uint256 pre = uid; //id of previous offer in unsorted offers list

1442:         uint256 pre = uid; //id of previous offer in unsorted offers list

1444:         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

1444:         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

1448:             _head = _near[id]; //set head to new first unsorted offer

1448:             _head = _near[id]; //set head to new first unsorted offer

1449:             _near[id] = 0; //delete order from unsorted order list

1449:             _near[id] = 0; //delete order from unsorted order list

1461:         _near[pre] = _near[id]; //set previous unsorted offer to point to offer after offer id

1461:         _near[pre] = _near[id]; //set previous unsorted offer to point to offer after offer id

1462:         _near[id] = 0; //delete order from unsorted order list

1462:         _near[id] = 0; //delete order from unsorted order list

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

6: import "./compound-v2-fork/CTokenInterfaces.sol";

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

5: import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

7: import "@openzeppelin/contracts/security/Pausable.sol";

7: import "@openzeppelin/contracts/security/Pausable.sol";

7: import "@openzeppelin/contracts/security/Pausable.sol";

52:     mapping(address => uint256) public periodFinish; // Token specific

52:     mapping(address => uint256) public periodFinish; // Token specific

53:     mapping(address => uint256) public rewardRates; // Token specific reward rates

53:     mapping(address => uint256) public rewardRates; // Token specific reward rates

54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

54:     mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific

55:     mapping(address => uint256) public lastUpdateTime; //Token specific

55:     mapping(address => uint256) public lastUpdateTime; //Token specific

56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific

56:     mapping(address => uint256) public rewardsPerTokensStored; // Token specific

60:         public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC

60:         public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC

61:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC

61:     mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC

146:             IERC20(myBathTokenBuddy) // Care with this?

146:             IERC20(myBathTokenBuddy) // Care with this?

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "./RubiconRouter.sol";

13:         bytes4 selector; // function selector

13:         bytes4 selector; // function selector

14:         bytes args; // encoded arguments (abi.encode() || abi.encodePacked)

14:         bytes args; // encoded arguments (abi.encode() || abi.encodePacked)

15:         address target; // target contract to call

15:         address target; // target contract to call

20:         address feeToken; // token in the form of which fee paid

20:         address feeToken; // token in the form of which fee paid

21:         uint256 totalAmount; // amount without deducted fee

21:         uint256 totalAmount; // amount without deducted fee

22:         uint256 feeAmount; // amount of feeToken from which fee should be charged

22:         uint256 feeAmount; // amount of feeToken from which fee should be charged

23:         address feeTo; // receiver of the fee

23:         address feeTo; // receiver of the fee

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

41:             fees[i] = (tokenAmounts[i] * feeValue) / feeType;

41:             fees[i] = (tokenAmounts[i] * feeValue) / feeType;

42:             amountsWithFee[i] = tokenAmounts[i] - fees[i];

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

105:         IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));

120:         _msgValue = msg.value - _feeAmount;

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";

7: import "@openzeppelin/contracts/access/Ownable.sol";

7: import "@openzeppelin/contracts/access/Ownable.sol";

7: import "@openzeppelin/contracts/access/Ownable.sol";

8: import "../../compound-v2-fork/Comptroller.sol";

8: import "../../compound-v2-fork/Comptroller.sol";

8: import "../../compound-v2-fork/Comptroller.sol";

8: import "../../compound-v2-fork/Comptroller.sol";

8: import "../../compound-v2-fork/Comptroller.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

9: import "../../compound-v2-fork/PriceOracle.sol";

10: import "../../BathHouseV2.sol";

10: import "../../BathHouseV2.sol";

11: import "../../RubiconMarket.sol";

11: import "../../RubiconMarket.sol";

24:         uint256 borrowedAmount; // how much to borrow in current borrow loop

24:         uint256 borrowedAmount; // how much to borrow in current borrow loop

25:         uint256 initAssetBalance; // initial balance of asset

25:         uint256 initAssetBalance; // initial balance of asset

26:         uint256 limit; // limit of borrowing loops

26:         uint256 limit; // limit of borrowing loops

27:         uint256 lastBorrow; // how much borrow on the last loop (check _borrowLimit)

27:         uint256 lastBorrow; // how much borrow on the last loop (check _borrowLimit)

28:         uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution

28:         uint256 currentBathTokenAmount; // amount of basthTokenAsset in the moment of execution

29:         uint256 currentAssetBalance; // balance of asset in the moment of execution

29:         uint256 currentAssetBalance; // balance of asset in the moment of execution

30:         uint256 toBorrow; // certain perc. to borrow from max amount available to borrow

30:         uint256 toBorrow; // certain perc. to borrow from max amount available to borrow

34:         address asset; // supplied as collateral

34:         address asset; // supplied as collateral

35:         address quote; // borrowed token

35:         address quote; // borrowed token

36:         uint256 borrowedAmount; // amount of borrowed quote

36:         uint256 borrowedAmount; // amount of borrowed quote

37:         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied

37:         uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied

38:         uint256 blockNum; // block number on which position was opened

38:         uint256 blockNum; // block number on which position was opened

39:         bool isActive; // false by default, when active - true

39:         bool isActive; // false by default, when active - true

39:         bool isActive; // false by default, when active - true

158:         for (uint256 i = 0; i < vars.limit; ++i) {

158:         for (uint256 i = 0; i < vars.limit; ++i) {

184:             vars.currentBathTokenAmount += _borrowLoop(

317:         _max = (_liq.mul(10 ** 18)).div(_price);

317:         _max = (_liq.mul(10 ** 18)).div(_price);

327:         uint256 _blockDelta = block.number - _startBlock;

331:         ).div(10 ** 18);

331:         ).div(10 ** 18);

407:         lastPositionId++;

407:         lastPositionId++;

431:         positions[_positionId].bathTokenAmount += _bathTokenAmount;

560:             _assetAmount += _loopBorrowed;

572:                 _limit++;

572:                 _limit++;

576:                 _limit++;

576:                 _limit++;

580:                 _limit++;

580:                 _limit++;

590:         _long // long can't be with 1x leverage

590:         _long // long can't be with 1x leverage

```

### [GAS-7] Use Custom Errors
[Source](https://blog.soliditylang.org/2021/04/21/custom-errors/)
Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

*Instances (36)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

27:         require(msg.sender == admin, "onlyAdmin: !admin");

33:         require(!initialized, "BathHouseV2 already initialized!");

144:         require(_underlying != address(0), "_bathify: ADDRESS ZERO");

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

31:         require(isAuthorized(msg.sender), "ds-auth-unauthorized");

47:         require((z = x + y) >= x, "ds-math-add-overflow");

51:         require((z = x - y) <= x, "ds-math-sub-underflow");

55:         require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");

321:         require(uint128(spend) == spend, "spend is not an int");

322:         require(uint128(quantity) == quantity, "quantity is not an int");

701:         require(!initialized, "contract is already initialized");

720:         require(isActive(id), "Offer was deleted or taken, or never existed.");

834:         require(!locked, "Reentrancy attempt");

859:         require(!locked, "Reentrancy attempt");

874:         require(!locked, "Reentrancy attempt");

879:                 require(_hide(id), "can't hide");

1040:             require(offerId != 0, "0 offerId"); //Fails if there are not more offers

1065:         require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");

1080:             require(offerId != 0, "offerId == 0");

1444:         require(!isOfferSorted(id), "offer sorted"); //make sure offer id is not in sorted offers list

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

41:         require(bathBalance > 0, "migrate: ZERO AMOUNT");

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

76:         require(!friendshipStarted, "I already have a buddy!");

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

115:         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

119: 		require(OK, "ETH transfer failed");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

76:         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

212:         require(pos.isActive, "closePosition: POS ISN'T ACTIVE");

228:         require(pos.isActive, "increaseMargin: POS ISN'T ACTIVE");

312:         require(_err == 0, "_maxBorrow: ERROR");

313:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

314:         require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

318:         require(_max > 0, "_maxBorrow: can't borrow 0");

344:         require(comptroller.exitMarket(_bathToken) == 0, "_exitMarket: ERROR");

```

### [GAS-8] Don't initialize variables with default value

*Instances (8)*:
```solidity
File: 2023-04-rubicon/contracts/BathHouseV2.sol

122:         for (uint256 i = 0; i < buddies.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

1214:         uint256 old_top = 0;

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

40:         for (uint256 i = 0; i < tokenAmounts.length; ++i) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

158:         for (uint256 i = 0; i < vars.limit; ++i) {

```

### [GAS-9] Long revert strings

*Instances (7)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

720:         require(isActive(id), "Offer was deleted or taken, or never existed.");

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

105:         require(msg.sender == bathHouse, "You are not my beloved bath house!");

122:         require(friendshipStarted, "I have not started a bathToken friendship");

143:         require(friendshipStarted, "I have not started a bathToken friendship");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/FeeWrapper.sol

70:         require(_OK, "low-level call to the Rubicon failed");

86:         require(_OK, "low-level call to the router failed");

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

76:         require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");

```

### [GAS-10] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (3)*:
```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

210:     function closePosition(uint256 posId) external onlyOwner {

226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {

242:     function withdraw(address token, uint256 amount) external onlyOwner {

```

### [GAS-11] `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
*Saves 5 gas per loop*

*Instances (9)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

569:         last_offer_id++;

899:         for (uint i = 0; i < payAmts.length; i++) {

911:         for (uint i = 0; i < ids.length; i++) {

924:         for (uint i = 0; i < ids.length; i++) {

1400:         _span[address(pay_gem)][address(buy_gem)]++;

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

407:         lastPositionId++;

572:                 _limit++;

576:                 _limit++;

580:                 _limit++;

```

### [GAS-12] Use shift Right/Left instead of division/multiplication if possible

*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

78:         z = add(mul(x, y), WAD / 2) / WAD;

82:         z = add(mul(x, y), RAY / 2) / RAY;

86:         z = add(mul(x, WAD), y / 2) / y;

90:         z = add(mul(x, RAY), y / 2) / y;

```

### [GAS-13] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (21)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

277:         return offers[id].timestamp > 0;

345:         if (makerFee() > 0) {

521:         require(pay_amt > 0);

523:         require(buy_amt > 0);

581:         require(amount > 0);

585:         if (makerFee() > 0) {

1037:         while (pay_amt > 0) {

1077:         while (buy_amt > 0) {

1133:             if (pay_amt > 0) {

1166:             if (buy_amt > 0) {

1209:         require(id > 0);

1226:         require(id > 0);

1289:         while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {

1325:             t_buy_amt > 0 &&

1326:             t_pay_amt > 0 &&

1410:         require(_span[pay_gem][buy_gem] > 0);

1452:         while (uid > 0 && uid != id) {

```

```solidity
File: 2023-04-rubicon/contracts/V2Migrator.sol

41:         require(bathBalance > 0, "migrate: ZERO AMOUNT");

```

```solidity
File: 2023-04-rubicon/contracts/periphery/BathBuddy.sol

180:         if (reward > 0) {

```

```solidity
File: 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

313:         require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");

318:         require(_max > 0, "_maxBorrow: can't borrow 0");

```

### [GAS-14] `internal` functions not called by the contract should be removed
If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

*Instances (4)*:
```solidity
File: 2023-04-rubicon/contracts/RubiconMarket.sol

62:     function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66:     function imin(int256 x, int256 y) internal pure returns (int256 z) {

70:     function imax(int256 x, int256 y) internal pure returns (int256 z) {

1183:     function _buys(uint256 id, uint256 amount) internal returns (bool) {

```
