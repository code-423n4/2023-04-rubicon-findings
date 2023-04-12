### Q-01 batchoffer does not return offer IDs
The [batchOffer function](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L887-L907) does not return the IDs of the created offers. This makes it hard to use this function from other contracts. It is recommended to return the IDs of the newly created offers
```diff
    function batchOffer(
        uint[] calldata payAmts,
        address[] calldata payGems,
         uint[] calldata buyAmts,
         address[] calldata buyGems
-    ) external {
+    ) external returns (uint256[] memory)  {
         require(
             payAmts.length == payGems.length &&
                 payAmts.length == buyAmts.length &&
                 payAmts.length == buyGems.length,
             "Array lengths do not match"
         );
+
+        uint256[] memory offerIDs = new uint256[](payAmts.length);
         for (uint i = 0; i < payAmts.length; i++) {
-            this.offer(
+            offerIDs[i] = offer(
                 payAmts[i],
                 ERC20(payGems[i]),
                 buyAmts[i],
                 ERC20(buyGems[i])
             );
         }
+        return offerIDs;
     }
```


### Q-02 BatchRequote does not return offer IDs

The [batchRequote function](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L887-L907) does not return the IDs of the created offers. This makes it hard to use this function from other contracts. It is recommended to return the IDs of the newly created offers

```diff 
    function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
         address[] calldata buyGems
-    ) external {
+    ) external returns (uint256[] memory) {
+        uint256[] memory offerIDs = new uint256[](payAmts.length);
         for (uint i = 0; i < ids.length; i++) {
             cancel(ids[i]);
-            this.offer(
+            offerIDs[i] = offer(
                 payAmts[i],
                 ERC20(payGems[i]),
                 buyAmts[i],
                 ERC20(buyGems[i])
             );
         }
+        return offerIDs;
     }

```
