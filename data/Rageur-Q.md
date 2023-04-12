## LOW-1: State variable visibility not set

### Code

```
uint256 constant WAD = 10 ** 18;
uint256 constant RAY = 10 ** 27;
bool locked;
```

### Affected file

* RubiconMarket.sol (Line: 74, 75, 224)

## LOW-2: Floating Pragma

### Affected file

* BathBuddy.sol Line(2)
* RubiconMarket.sol Line(2)

## LOW-3: State variable could be better packed

### Code

Actual:
```
uint256 public last_offer_id;
mapping(uint256 => OfferInfo) public offers;
bool locked;
uint256 internal feeBPS;
address internal feeTo;
bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

Better:
```
uint256 public last_offer_id;
mapping(uint256 => OfferInfo) public offers;
address internal feeTo;
bool locked;
uint256 internal feeBPS;
bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

### Affected file

* RubiconMarket.sol

## LOW-4: SafeMath' when compiler is > 0.8.0

### Code

```
using SafeMath for uint256;
```

### Affected file

* BathBuddy.sol (Line: 39)
* Position.sol (Line: 18)