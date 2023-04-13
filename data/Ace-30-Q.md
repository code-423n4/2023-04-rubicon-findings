## Impact
It's a good practice to validate the input aaray parameters in the constructor. In this case, you should ensure that bathTokensV1 and bathTokensV2 arrays have the same length, as they are intended to represent corresponding pools:
```solidity
constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
    require(bathTokensV1.length == bathTokensV2.length, "V2Migrator: ARRAY LENGTHS DON'T MATCH");
    ...
}
```
## Proof of Concept
```solidity
constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
        for (uint256 i = 0; i < bathTokensV1.length; ++i) {
            // set v1 to v2 bathTokens 
            v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
        }
    }
```

## Tools Used
vscode
## Recommended Mitigation Steps
```solidity
constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
    require(bathTokensV1.length == bathTokensV2.length, "V2Migrator: ARRAY LENGTHS DON'T MATCH");
    ...
}
```