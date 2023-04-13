| ID     | Title                                    | Severity |
| ------ | ---------------------------------------- | -------- |
| [G-01] | can save gas by using custom errors      | Gas      |
| [G-02] | set constructor as payable will save gas | Gas      |
| [G-03] | modifier created but never used          | Gas      |

# [G-01] can save gas by using custom errors

## Description

you can save gas by using custom error rather that using long error message.

## context

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L374-L376

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L379-L382

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L721-L727

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L68-L79

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L100-L104

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L114-L117

## Recommendations

add custom error instead of the long error strings.

# [G-02] set constructor as payable will save gas

## Description

adding `payable` to the constructor will save some gases

## context

add payable to these constructors :

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L54
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30

## Recommendations

add `payable` to the constructors

# [G-03] modifier created but never used

## Description

this modifier was created but never used and cause more gas at deploying time

## context

remove this modifier that did not used in the contract

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94

## Recommendations

remove the modifier that not used in the contract
