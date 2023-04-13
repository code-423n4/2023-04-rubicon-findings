# Have not check whether the length of input parameter is equal

`V2Migrator.constructor` fucntions has not check whether the length of input parameter is equal. It will set a 0 address (v1ToV2Pools[bathTokensV1[i]] = address(0);) if the length of `bathTokensV2` is higher than `bathTokensV1`.

V2Migrator.constructor, Line 30-35