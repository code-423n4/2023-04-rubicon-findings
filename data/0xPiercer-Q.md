# Order of Functions

Some external functions are between public, and some external functions follow after public.

# Instances
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628

# Recommendation
According to the Solidiity Style guide, functions should be ordered as follows:
- constructor
- receive
- fallback
- external
- public
- internal
