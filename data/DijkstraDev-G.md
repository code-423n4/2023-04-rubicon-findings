#### **[G-01]** Split require statements to save gas.
[RubiconMarket#L612](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L612)

#### **[G-02]** `isClosed()` always return false.
- Can be remove. Some modifiers can be simplyfied too if remove isclosed function. Saves 20 gas units per call (4 calls).
[RubiconMarket#L620](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620)