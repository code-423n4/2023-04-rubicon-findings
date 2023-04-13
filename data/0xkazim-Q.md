| ID     | Title                     | Severity |
| ------ | ------------------------- | -------- |
| [L-01] | use 2 step transfer owner | low      |

# [L-01] use 2 step transfer owner

## Description

the position contract use ownable contract that inherited from OZ, it's better to use 2 step transfer owner rather than ownable.sol to avoid remove the owner by mistake or set owner to malicious address by mistake

## context

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L15

## Recommendations

use the safe ownable from OZ packages

