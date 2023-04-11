QA1. The constructor fails to verify that that underlying tokens should be the same for corresponding pools:

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30-L35](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30-L35)

Mitigation:
 constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
        for (uint256 i = 0; i < bathTokensV1.length; ++i) {
            // set v1 to v2 bathTokens
+           if(bathTokensV1[i].underlyingToken() != bathTokensV2[i].underlyingToken()) 
+                 revert DifferentUnderlyingTokensForPools();  
            v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
        }
    }

QA2. migrate() fails to check that both bathTokenV1 is whitelisted in the migration list, ``v1ToV2Pools``. One needs to check that ``v1ToV2Pools[address(bathTokenV1)] != 0``.

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L38-L74](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L38-L74)

Mitigation: check that ``v1ToV2Pools[address(bathTokenV1)] != 0`` in ``migrate()``.

QA3. The condition of `` msg.sender != address(0)`` can be eliminated since it is impossible for msg.sender to be zero. 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L97](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L97)

QA4. checking zero address for these two inputs are important

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32-L39](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32-L39)


