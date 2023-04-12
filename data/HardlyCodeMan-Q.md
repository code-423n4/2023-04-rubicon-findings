## 2023-04-rubicon Low and Quality Assurance Report

### Modified DSAuth Leaves isAuthorized() Pointless
Use of a modified vesion of the [DSAuth](https://github.com/dapphub/ds-auth) source has been added to the project in [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L15-L42). A key part of the autorization is function [isAuthorized()](https://github.com/dapphub/ds-auth/blob/a51e7caa1f03eeffed66d67f3b38605e0f1f881d/src/auth.sol#L58). 
Because the main authorization check logic has been heavily modified in [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L35-L41) to the point where ```isAuthorized()``` is rendered esentially useless, with the logic being able to be moved to the modifier [auth()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L30-L33) to both save gas and have the same authorization effect.

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..1e3bcfa 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
 
@@ -28,17 +28,9 @@ contract DSAuth is DSAuthEvents {
     }
 
     modifier auth() {
-        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
+        require(msg.sender == owner, "unauthorized");
         _;
     }
-
-    function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
-    }
 }
```

### Implement 2-Step Auth
Given the use of a heavily modified vesion of the [DSAuth](https://github.com/dapphub/ds-auth) source has been added to the project in [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L15-L42), a best practice would be to implement a 2-step authorized user address change along with the modifications already suggested for the ```isAuthorized()``` function.
A 2-Step authorization process allows the project an oppertunity to catch and recover from an incorrect/malicious address input to the ```owner``` value utilized in auth checks, which could otherwise lockout the legitimate owner EOA in a form of DoS.

```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..b497060 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -21,24 +21,27 @@ contract DSAuthEvents {
 /// @dev Provides the auth modifier for authenticated function calls
 contract DSAuth is DSAuthEvents {
     address public owner;
+    address public pending;
 
     function setOwner(address owner_) external auth {
-        owner = owner_;
+        pending = owner_;
+    }
+
+    function clearAuthClaim() external auth {
+        pending = address(0);
+    }
+
+    function claimOwner() external {
+        require(msg.sender == pending, "Not the pending owner.");
+        owner = pending;
+        
         emit LogSetOwner(owner);
     }
 
     modifier auth() {
-        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
+        require(msg.sender == owner, "unauthorized");
         _;
     }
-
-    function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
-    }
 }
```

### Unspecific Compiler Version Pragma - pragma ^
A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

Solidity 0.8.17 is spicified in other in scoped contracts which is not bleeding edge yet suffieciently tested. Avoid Solidity 0.8.13 and 0.8.14 because they contain some encoding and assembly errors. Really it is a  simple change to bring the two files in question into alignment with the rest of the project at 0.8.17.

- [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)```pragma solidity ^0.8.9;```
- [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)```pragma solidity ^0.8.0;```