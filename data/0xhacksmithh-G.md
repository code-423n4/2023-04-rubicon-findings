### [Gas-01] ```struct``` Can Be Cached To `memory` and used furthere, Instead Of Calling It Multiple Times.
```solidity
function isOfferSorted(uint256 id) public view returns (bool) {
        return
            _rank[id].next != 0 ||
            _rank[id].prev != 0 ||
-            _best[address(offers[id].pay_gem)][address(offers[id].buy_gem)] == id;
+            Offers _offers = offers[id];
+            _best[address(_offers[id].pay_gem)][address(_offers[id].buy_gem)] == id;
    }
```
```solidity
+           Offers _offers = offers[id];
             if (
                mul(buy_amt, 1 ether) <
-               wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt) 
+               wdiv(_offers[offerId].pay_amt, _offers[offerId].buy_amt)
            ) {
                break; //We consider that all amount is sold
            }
-            if (buy_amt >= offers[offerId].pay_amt) {
+            if (buy_amt >= offers[offerId].pay_amt) {
                //If amount to buy is higher or equal than current offer amount to sell
-               fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator  // @audit
-               buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy  // @audit
-               take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer  // @audit
+                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator  // @audit
+               buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy  // @audit
+               take(bytes32(offerId), uint128(offers[offerId].pay_amt));
            } else {
                //if lower
                fill_amt = add(
                    fill_amt,
                    rmul(
                        mul(buy_amt, 10 ** 9),
-                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt) 
+                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt) // @audit
                    ) / 10 ** 9
```

*Instances(14)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1024
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1058
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1085
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1091-L1093
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1100
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1166
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1266-1267
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1211-1212
```
### [Gas-02] Instead of `abi.encodePacked()` Use `abi.concat()`

*Instances(2)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L369
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L442
```
### [Gas-03] Function Result Could Be Cached In `memory`
```solidity
....
....
 if (makerFee() > 0) {
            uint256 mFee = mul(spend, makerFee()) / 100_000; // @audit makerFee() could be cached
...
...
```
Here We could cached `makerFee()` and use it forther

```solidity
....
....
if (makerFee() > 0) {
            _amount -= mul(amount, makerFee()) / 100_000; // @audit chace makerFee()
        }
...
...
```

```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L345-L346
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L585-L587
```
### [Gas-04] Functions like `isActive()`, `getOwner()`, `getOffer()`, `getRecipient()` Miss Check for `Id` Input Parameter.
Simply If Inputed `Id` Parameter is out of bound i.e it is grater than `last_offer_id` still storage variable read call occurs,
So here we can stop that excess gas consumption by adding a simple check before storage call i.e current inputed id is in range
```solidity
function isActive(uint256 id) public view returns (bool active) {
+       require(id <= last_offer_id);
        return offers[id].timestamp > 0;
    }
```
*Instances(4)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276-L279
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L280-L283
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L284-L287
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288-L293
```
### [Gas-05] Massive 15k per Transaction Gas Can Save By Using 1 and 2 For Re-entrancy Gaurd.
Using `true` and `false` will trigger gas refunds, which after London are 115 of what they used to be, meaning using 1 and 2(keeping the slot non-zero) will cost 5k per change. 
(5k + 5K) vs (20k + 5k) saving you 15k gas per function which use the modifier.

```solidity
    modifier synchronized() {  // @audit refer the gas optimization issue
        require(!locked);
        locked = true;
        _;
        locked = false;
    }
```

*Instances(Multiple Instances)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L834
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L859
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L874
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L938
```
### [Gas-06] Duplicate `require()` Checks Should Be Refactored To A `modifier` or `public` Function
```solidity
            offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
            require(offerId != 0, "0 offerId"); //Fails if there are not more offers 
```
Above code segment used to find out validation of a offerId which get repeated man times in code base so it should wrapped in a modifier or in a public function and used further.

*Instances(2)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1039-L1040
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1079-L1080
```
### [Gas-07] Shift Right or, Left Instead Of Division or Multiplication by 2
Shifting one to right will calculate a division by two `SHR` opcode only requires 3 gas, Where as Division via `DIV` opcode consume 5 gas, Here simply 2 gas saved
Additionally, shifting is used to get around solidity's division operation's division-by-0 prohibition

*Instances(4)*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L78
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L82
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L86
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L90
```
### [Gas-08] `keccak256()` Should Only Need To Be Called On A Specific String Literal Only Once

*Instances()*
```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232
```
### [Gas-09] Use Assembly For Zero Address Check G-66


```
File : contracts/RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L255
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L459
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L522
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L524
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L702
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L725
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1477
```
### [Gas-10] Using `unchecked` Blocks Will Help In Gas Saving G-57
Solidity version 0.8+ comes wit implicit Overflow and Underflow checks on unsigned integers. When an Overflow or an Underflow is not possible(as an example, when a comparison is made before the arithmatic operation), Some gas can be saved by using an `unchecked` block.

*Instances(1)*
```
File :
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L569
```

### [Gas-11] Setting `constructor` To `payable` Will Save Gas

*Instances(All contracts)*


### [Gas-12] With `assembly` ETH Can Be Send To Save Gas
`return` data(bool success, ) has to be stored due to EVM architechture, but in a usage like below, `out` and `outsize` values are given (0,0) this storage disappears and gas optimization is provided.

```solidity
(bool succ, ) = dest.call{value: amount}("");
+ bool succ;
+ assembly{
         succ := call(gas(),dest,amount,0,0)
+    }
+ require(succ); 
```

### [Gas-13] Spliting `require()` Statements That Use `&&` and Save Gas.
Save 16gas per instance.

*Instances(5)*
```
File : RubiconMarket.sol
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L255
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L725
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893-L896
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L941
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1413
```
```
File:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L96-L97
```

### [Gas-14] Use Openzeppelin's Reentrancy-Gaurd Contract, Which is More Gas Optimized Than Using Your Own

*Instances(1)*
```
File :
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol
```
`