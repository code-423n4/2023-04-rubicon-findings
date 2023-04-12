## 2023-04-rubicon Low and Quality Assurance Report
---

### [QA-1] Modified DSAuth Leaves isAuthorized() Pointless
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
---

### [QA-2] Implement 2-Step Auth
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
---

### [QA-3] isClosed() Always Returns False
Function ```isClosed()``` of contract ```ExpiringMarket``` in [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L593) will always return ```false``` suggesting that all positions are in essence always open. ```isClosed()``` has direct implications to modifiers ```can_offer()``` ```can_buy()``` ```can_cancel()``` of [ExpiringMarket](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L593) and ```can_cancel()``` of [RubiconMarket](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L674). ```isClosed()``` given it's name and logical purpose, should check if the position is closed and return a bool in this case.

#### Potential Change Utilising isActive()
    ```solidity
    function isClosed(uint id) public pure returns (bool closed) {
        if(isActive(id)) {
            return false;
        } else {        
            return true;
        }
    }
    ```

#### ExpiringMarket
- [modifier can_offer()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L597-L600) has redundant ```require(!isClosed())```. ```isClosed()``` always returns false fullfilling this requirement logically.
- [modifier can_buy()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L603-L607) has redundant ```require(!isClosed())```. ```isClosed()``` always returns false fullfilling this requirement logically.
- [modifier can_cancel()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L610-L618) has redundant ```require(isClosed()```, where ```isClosed()``` will always returns false, always invalidating this require check, and moving the logic on to other checks to validate through the logical ORs. Therefore isClosed() can be removed from this modifier.

#### RubiconMarket
- [modifier can_cancel()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L719-L729) has redundant ```require(isClosed()```, where ```isClosed()``` will always returns false, always invalidating this require check, and moving the logic on to other checks to validate through the logical ORs. Therefore isClosed() can be removed from this modifier.
---

### [QA-4] Modifier can_offer() has no ustilisation
- SimpleMarket [modifier can_offer()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L260-L262)
- ExpiringMarket [modifier can_offer()](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L597-L600) 
Neither ```can_offer()``` iteration is utilised.
---

### [QA-5] Unspecific Compiler Version Pragma - pragma ^
A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

Solidity 0.8.17 is spicified in other in scoped contracts which is not bleeding edge yet suffieciently tested. Avoid Solidity 0.8.13 and 0.8.14 because they contain some encoding and assembly errors. Really it is a  simple change to bring the two files in question into alignment with the rest of the project at 0.8.17.

- [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)```pragma solidity ^0.8.9;```
- [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)```pragma solidity ^0.8.0;```