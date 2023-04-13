# Low Risk and Non-Critical Issues

## L-01 initialize() can be front-run
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L32
There is nothing preventing another account from calling the initializer before the contract owner. In the best case, the owner is forced to waste gas and re-deploy. In the worst case, the owner does not notice that his/her call reverts, and everyone starts using a contract under the control of an attacker

```solidity
File: contracts\BathHouseV2.sol
32:     function initialize(address _comptroller, address _pAdmin) external {
33:         require(!initialized, "BathHouseV2 already initialized!");
34:         comptroller = Comptroller(_comptroller);
35:         admin = msg.sender;
36:         proxyAdmin = _pAdmin;
37: 
38:         initialized = true;
39:     }


```

## L-02 v1ToV2Pools mapping is initialized only in the constructor, which could prevent users to migrate a v1 pool

This mapping will allow users to migrate their v1 pools to v2. However, if admin set a wrong address for a pool, users will not be able to execute the migration. Either add a check to verify the 2 pools are compatible, or add a function to update the mapping

```solidity
File: contracts\V2Migrator.1.sol
30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {
32:             // set v1 to v2 bathTokens
33:             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
34:         }
35:     }
```