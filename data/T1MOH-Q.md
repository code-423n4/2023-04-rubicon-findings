## 01. Misleading comment
encodePacked will not work
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L14

## 02. Make function pure to hide warning
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28

## 03. Replace bytes.concat with abi.encodeWithSelector() to improve code readability
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L67
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L83

## 04. Typo in comment
rotuer -> router https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L75

## 05. Typo in revert string
INVLAID -> INVALID
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L593
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L597

## 06. Misleading string in revert
replace with "not enough or higher ETH sent"
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L115

## 07. Misleading comment
owner will be not BathHouse, but owner of BathHouse
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L44

## 08. Remove unused modifier
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L92-L102

## 09. Remove unused function to improve readability
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310

## 10. Remove commented code to improve readability
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L100
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L129
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L187
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L300
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L429
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1343

## 11. Remove unnecessary variable name
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L496

## 12. Use buyEnabled instead of true
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L774
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L818

## 13. Remove strange comment
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1270

## 14. Add require to check max value of fee
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1467
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1472
