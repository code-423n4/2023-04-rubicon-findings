# 1. Order of Functions

Some external functions are between public, and some external functions follow after public.

### Instances
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628

### Recommendation
According to the Solidiity Style guide, functions should be ordered as follows:
- constructor
- receive
- fallback
- external
- public
- internal

# 2. DSnote contract is missing commentary explaining its function.
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L633

### Recommendation
Add commentary.

# 3. Bump funciton is not being used
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297

### Recommendation
Remove.

# 4. Open todos
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L299
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L428
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L661
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675

### Recommendation
Remove.
