
# Gas Optimization Report

This report focuses on Rubicon v2 contest, in context of various improvements that can be made in terms of gas cost.

Some of the opportunities identified for improving gas efficiency for the code in scope of Rubicon v2 are categorised into 08 main areas; with further multiple instances in each of the category.


# Summary

 [G-01] 0perator assignment is more gas efficient than compound assignment (05 Instances)
 [G-02] Multiple mappings can be combine into a single one (15 Instances)
 [G-03] Public visibility consumes more gas as compared to external in functions (08 Instance)
 [G 04] Declare Public library as internal library (03 Instances)
 [G-05] Use solidity version 0.8.19 to gain some gas boost (06 Instances)
 [G 06] Functions guaranteed to revert when called by normal users can be marked payable (13 Instances)
[G-07] Using delete statement can save gas (05 Instances)
[G-08] Use hardcode address instead address(this) (21 Instances)

 
# [G-01] 0perator assignment is more gas efficient than compound assignment (05 Instances)

Compound assignment operators (+= / -=) are more expensive in terms of gas consumption and needs to be avoided.

Operator assignments (a = a + b / a - b) are preferable in terms of gas optimization.

Link to the Code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L184
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L431
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L560


# [G-02] Multiple mappings can be combine into a single one (15 Instances)

When multiple mappings are used in same function, it’s better to combined them into a single mapping using a struct.

Combined mapping reduces storage slot per mapping and also are cheaper in terms of associated stack operations calculation carried out.

Link to the Code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L222
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L691
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L692
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L693
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L694
6.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L695
7.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L20
8.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L21
9.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52
10.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L53
11.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L54
12.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L55
13.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L56
14.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L59
15.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L61


# [G-03] Public visibility consumes more gas as compared to external in functions (08 Instance)

Functions with public visibility, if not called within the contract needed to be changed external.

Link to the Code:
1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L998
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010
6.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016
7.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1149
8.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L45


# [G 04] Declare Public library as internal library (03 Instances)

External call to a public library function is very expensive for reference checks this article. When library has only internal functions it can be used as internal library.

Link to the code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L12
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L6
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L6

# [G-05] Use solidity version 0.8.19 to gain some gas boost (06 Instances)

Use latest version of solidity i.e., 0.8.19 for getting additional gas saving for reference check this article.

Link to the code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L2
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L2
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L2
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L2
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L2
6.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L2


# [G 06] Functions guaranteed to revert when called by normal users can be marked payable (13 Instances)

If a function uses a modifier for limiting users, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers.

Link to the code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L452
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511
6.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
7.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L855
8.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871
9.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L955
10.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60
11.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191
12.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232
13.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L168


# [G-07] Using delete statement can save gas (05 Instances)

Link to the code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1062
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1104
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1449
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1462
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L181


# [G-08] Use hardcode address instead address(this) (21 Instances)

Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address.

Link to the code:

1.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L538
2.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L104
3.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L44
4.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L61
5.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L64
6.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L216
7.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L67
8.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L140
9.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L143
10.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L160
11.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L233
12.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L286
13.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L310
14.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L356
15.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L364
16.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L392
17.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L487
18.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L495
19.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L502
20.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L537
21.	https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L100

