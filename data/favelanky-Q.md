## [L-1] Lack Of Zero Address Check

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.

There is 1 instances of this issue:

```solidity
File: /contracts/BathHouseV2.sol

32:  function initialize(address _comptroller, address _pAdmin) external {
33:         require(!initialized, "BathHouseV2 already initialized!");
34:         comptroller = Comptroller(_comptroller);
35:         admin = msg.sender;
36:         proxyAdmin = _pAdmin;
37: 
38:         initialized = true;
39:  }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/BathHouseV2.sol#L32-L38

```solidity
File: /contracts/V2Migrator.sol

33:             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L33

## [L-2] Array Length Not Validated

It's crucial to validate array length because it might cause unexpected behavior.

There is 1 instances of this issue:

```solidity
File: /contracts/V2Migrator.sol

30:  constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/V2Migrator.sol#L30

## [L-3] Constant Values Should Used To Immutable Rather Than Constant

There is a difference between constant variables and immutable variables, and they should each be used in their appropriate contexts.

While it doesn’t save any gas because the compiler knows that developers often make this mistake, it’s still best to use the right tool for the task at hand.

There is 1 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

231:  bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

```

## [L-4] Floating Pragma

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

https://swcregistry.io/docs/SWC-103

There are 2 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

1: pragma solidity ^0.8.9;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1

```solidity
File: /contracts/periphery/BathBuddy.sol

1: pragma solidity ^0.8.0;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L1

## [L-5] Critical Address Changes Should Use Two-step Procedure

The critical procedures should be a two-step process.

There is 1 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

25:  function setOwner(address owner_) external auth {
26:         owner = owner_;
27:         emit LogSetOwner(owner);
28:  }
```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L25-L28

## [N-1] Remove Unused Variables 

In the code exists a lot of unused variables and code logic related to them. The variables above are always `true`. Remove these variables and branch logic related to them.

There are 2 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

675:  bool public buyEnabled = true; //buy enabled TODO: review this decision!
676:  bool public matchingEnabled = true; //true: enable matching,

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L675-L676

```solidity
File: /contracts/RubiconMarket.sol

863:  function(uint256, uint256) returns (bool) fn = matchingEnabled

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L863

## [N-2] Use A More Recent Version Of Solidity

Solidity 0.8.10 has a useful change that reduced gas costs of external calls which expect a return value.

In 0.8.15 the conditions necessary for inlining are relaxed. Benchmarks show that the change significantly decreases the bytecode size (which impacts the deployment cost) while the effect on the runtime gas usage is smaller.

In 0.8.17 prevent the incorrect removal of storage writes before calls to Yul functions that conditionally terminate the external EVM call; Simplify the starting offset of zero-length operations to zero. More efficient overflow checks for multiplication.

There are 2 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

1: pragma solidity ^0.8.9;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L1

```solidity
File: /contracts/periphery/BathBuddy.sol

1: pragma solidity ^0.8.0;

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L1

## [N-3] Avoid Shadowing

The struct name is the same as a contract name. Try to avoid shadowing to best practice.

There is 1 instances of this issue:

```solidity
File: /contracts/utilities/poolsUtility/Position.sol

33:  struct Position {

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/utilities/poolsUtility/Position.sol#L33


## [N-4] Remove Testing and Debugging code

It’s used to print the values of variables while running tests to help debug and see what’s happening inside your contracts But since it’s a development tool, it serves no purpose on mainnet.

There is 1 instances of this issue:

```solidity
File: /contracts/RubiconMarket.sol

4: // Uncomment this line to use console.log
5: // import "hardhat/console.sol";

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/RubiconMarket.sol#L4-L5

## [N-5] Incorrect Comments

Refactor the comments in the code. They should comply with the second version of the protocol. 

There are 3 instances of this issue:

```solidity
File: /contracts/periphery/BathBuddy.sol

28:  * @dev The ~only~ external entrypoint used in the system is the getReward(token) function called by the Bath Token. Extra care should be taken to make sure only the beneficiary can ever access the funds and send them to the withdrawer (and fee to self)

35: /// @dev NOTE: There is an implicit assumption that having bathToken shares means you are staked. This BathBuddy is paired with a SINGLE bathToken

165:  // IDEA: add this core logic to bathTokens and extrapolate to potentially multiple bath tokens if possible
166:  // Msg.sender calls BathToken (becomes holderRecipient here) which calls any time to get all rewards accrued for provided token
167:  /// @param holderRecipient allows the BathToken to pass through correct reward amounts to callers of the function @ BATH TOKEN LEVEL

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L28

## [N-6] Unused Events

There are 2 instances of this issue:

```solidity
File: /contracts/periphery/BathBuddy.sol

263:  event Withdrawn(address indexed user, uint256 amount);

266:  event Recovered(address token, uint256 amount);

```

https://github.com/code-423n4/2023-04-rubicon/tree/main/contracts/periphery/BathBuddy.sol#L263
