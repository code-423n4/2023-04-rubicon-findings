
# [G-1] Use nested if and avoid multiple check combinations
##### using nested is cheaper than using && multiple check combinations.There are more advantages,such as easier to read code and better coverage reports.


4 results -2 files:

##### instances
```solidity

contracts/RubiconMarket.sol

349:    if (_offer.owner == address(0) && getRecipient(id) != address(0))

1197:   if (isActive(id) &&offers[id].pay_amt < _dust[address(offers[id].pay_gem)])

3124:   if (t_buy_amt > 0 && t_pay_amt > 0 && t_pay_amt >= _dust[address(t_pay_gem)])

```
###### https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349

```solidity
contracts/utilities/poolsUtility/Position.sol;

if ( IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 && borrowBalance(_bathTokenAsset) == 0)

```

#### Remomended code:

```solidity
contracts/RubiconMarket.sol
- 349:    if (_offer.owner == address(0) && getRecipient(id) != address(0))
+         if (_offer.owner == address(0)){
+            if (paymasterDeadline < deadline){
+             }
+          }
```


## G[2] Functions guarenteed to revert when called by normal users can be marked  payable
####### If the function modifier or require such as onlyowner is used ,the function will revert if a normal user tries to pay the function.Marking the function as payable will lower the gas cost for legitimite callers beacause the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

```solidity
628:    function stop() external auth {

955:    function setMinSell( ERC20 pay_gem, uint256 dust ) external auth note returns (bool) {

1466:   function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {

1471:   function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {

1476:   function setFeeTo(address newFeeTo) external auth returns (bool) {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476


```solidity
contracts/BathHouseV2.sol

function createBathToken(address underlying, InterestRateModel interestRateModel, uint256 initialExchangeRateMantissa, address implementation, bytes memory becomeImplementationData) external onlyAdmin {
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60


# G[3] Avoid using state variable in emit (130 gas)
##### using a state varibale in LogSetOwner emits wastes gas
1 result - 1 file:

``` solidity
rubicon/contracts/RubiconMarket.sol

25:    function setOwner(address owner_) external auth {
26:        owner = owner_;
27:        emit LogSetOwner(owner);
28:    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25
### Recomendation code:

```solidity
25:    function setOwner(address owner_) external auth {
+27:   emit LogSetOwner(owner_);
26:        owner = owner_;
-27:        emit LogSetOwner(owner);
28:    }
```

# [G-04] State variables can be packed into fewer storage slots
If variables occupying the same slot are both written by the same function or by the constructor, avoids a seperate Gsset(20000 gas).Read of the variables can also be cheaper.
1 Instance:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L219

```solidity
rubicon/contracts/RubiconMarket.sol

219:    uint256 public last_offer_id;

220:    /// @dev The mapping that makes up the core orderbook of the exchange
221:    mapping(uint256 => OfferInfo) public offers;
222:
223:    bool locked;

224:    /// @dev This parameter is in basis points
225:    uint256 internal feeBPS;

226:    /// @dev This parameter provides the address to which fees are sent
227:    address internal feeTo;

228:    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```
## Recomended :
```solidity
219:     bool locked;
220:     address internal feeTo;
221:     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
222:     uint256 internal feeBPS;
223:     uint256 public last_offer_id;
224:     mapping(uint256 => OfferInfo) public offers;
```

# [G-05] Structs can be packed based on the data sizes.
To make a struct more gas efficient ,it is recomended to arrange the variables based on their data sizes from smallest to largest.

##### Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L234

```solidity
234:    struct OfferInfo {
235:        uint256 pay_amt;
236:        ERC20 pay_gem;
237:        uint256 buy_amt;
238:        ERC20 buy_gem;
239:        address recipient;
240:        uint64 timestamp;
241:        address owner;
242:    }
```
### Recomended method:

```solidity
234:    struct OfferInfo {
235:        address recipient
236:        address owner
237:        ERC20 pay_gem;
238:        ERC20 buy_gem;
239:        uint64 timestamp;
240:        uint256 pay_amt;
241:        uint256 buy_amt;
242:    }
```

# [G-06] 10 ** 9 can be changed to 1e9 to avoid arithmetic operations and save gas.
Instances:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1057
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1059
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1099
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1101
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1142
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1144
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1175
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1177


#[G-07] Use double require instead of using &&
Use double require instead of operator && can save more gas.
When having a require statement with 2 or more expressions needed,place the expression that costs less gas first.
```solidity
253:        require(
254:            (msg.sender == getOwner(id)) ||
255:                (msg.sender == getRecipient(id) && getOwner(id) == address(0))
256:        );
257:        _;
258:    }
```
##### Instances noticied:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L612
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L721
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L940
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1412

#### Recomended method:
```solidity
253:        require((msg.sender == getOwner(id)||(msg.sender == getRecipient(id));
254:            require(getOwner(id) == address(0));

      );
      _;
    }
```