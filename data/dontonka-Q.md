https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30
```diff
diff --git a/contracts/V2Migrator.sol b/contracts/V2Migrator.sol
index e226785..e97e6f7 100644
--- a/contracts/V2Migrator.sol
+++ b/contracts/V2Migrator.sol
@@ -28,6 +28,7 @@ contract V2Migrator {
     /// @dev underlying tokens should be the same for corresponding pools
     /// i.e. USDC -> USDC, WETH -> WETH, etc.
     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
+        //@audit (QA) would add a require bathTokensV1.length == bathTokensV2.length
         for (uint256 i = 0; i < bathTokensV1.length; ++i) {
             // set v1 to v2 bathTokens
             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L119
```diff
diff --git a/contracts/BathHouseV2.sol b/contracts/BathHouseV2.sol
index 5ff6d4c..844a072 100644
--- a/contracts/BathHouseV2.sol
+++ b/contracts/BathHouseV2.sol
@@ -116,6 +116,8 @@ contract BathHouseV2 {
         address[] memory buddies,
         address[] memory rewardsTokens
     ) external {
+        // @audit QA: add a require to ensure arrays have the same lenght
+
         // claim rewards from comptroller
         comptroller.claimComp(msg.sender);
         // get rewards from bathBuddy
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L144
```diff
diff --git a/contracts/BathHouseV2.sol b/contracts/BathHouseV2.sol
index 5ff6d4c..844a072 100644
--- a/contracts/BathHouseV2.sol
+++ b/contracts/BathHouseV2.sol
@@ -141,7 +143,7 @@ contract BathHouseV2 {
         view
         returns (string memory _name, string memory _symbol, uint8 _decimals)
     {
-        require(_underlying != address(0), "_bathify: ADDRESS ZERO");
+        require(_underlying != address(0), "_bathify: ADDRESS ZERO"); //@audit (QA) redundant: this is already verified in createBathToken and its the only func that calls _bathify

         _name = string.concat("bath", ERC20(_underlying).symbol());
         _symbol = string.concat(_name, "v2");
```
