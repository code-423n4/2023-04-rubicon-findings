Dead code. This function is not used. Remove or implement it.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297-L310

DSMath is no more required because solidity version 0.8.x is used. Remove it and change the code where its functions are used for better readability and save some gas. Same thing for OZ SafeMath in Position.sol and ButhBuddy.sol.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L44-L92

_asset parameter is not used. Remove it or add logic. Function state mutability can be restricted to view.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L515-L531

Dead code. This modifier is not used. Remove it.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L92-L102

Best practice is to use modifiers only for checks. Use a function instead.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L246-L257

Try to use recommendations from solidity guidelines for contract layout.
https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-layout
https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions