
### Low and QA

#### 1 Potential incorrect use of RDIV in buyAllAmount and sellAllAmount

The `ds-math` function `rdiv` performs rounding depending on whether the last digit is greater than or equal to 5. If it is greater than 5, it will round up; otherwise, it will round down. It is more suitable to have a clear direction to always round up or down.

See: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1100

```solidity
         fill_amt = add(
                    fill_amt,
                    rmul(mul(buy_amt, 10 ** 9), rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)) / 10 ** 9
                ); //Add amount sold to acumulator
```

Here for example: 

```
      uint offer_buy_amt = 200e18;
      uint offer_pay_amt = 3e18;
```

The result of an `rdiv` operation would be:
```
// round-up
66666666666666666666666666667
```

This could lead to problems if a payout always occurs with a better price than available tokens.

Currently, this is not causing problems since a round-down is happening with the / 10 ** 9 division.

However, it is recommended to make a clear decision for each division to always round up or always round down.

If the idea is to always round-down it makes sense to also use a `div` function which reflects this decision. 

**Round Down**

```diff=
-     rdiv(a,b)
+     // always round-down
+     div(mul(a, RAY), b)
```
-

#### 2. General Lack of using events
The codebase has a general lack of events. Whenever a system state variable changes, such as a `fee` always `emit` an event. This should apply to any other changes or actions as well.

#### 3. More Tests and Fuzz Tests
The codebase could benefit from more test and fuzz tests.

#### 4. No need for DS-Math for add and mul 
The DS-Math library is not required for basic arithmetic operations like `add, mul, sub, div`, as Solidity version 0.8.0 includes underflow/overflow protection.

#### 5. DSAuth only for one address
- The DSAuth libary is typical used for a multi-owner pattern, It is suggested to use a different name to indicate a different usage of a common library, such as `DSOwnerAuth` or just `onlyOwner`.

#### 6. No consistence underscore for internal functions
- Not all `internal` functions using an `_` undercore pattern.
#### 7. Not using setOwner function in auth
Some functions having a setOwner function which is not be used in the constructor. Here, the method could be used with a deticated event. 

#### 8. Indicate with storage slots can't be changed because of the deployed V1 Market

It is currently not clear, which slots are not allowed to be modfied because of the v1 version the contracts.

After an analysis the only new variable added to the V2 RubiconMarket is:
```solidity=
 uint256 public dustId; // id of the latest offer marked as dust
```
This should be clearly indicated with comments.

#### 9. remove uncommented code snippets
For improving the readablility it is recommend to clean up the codebase and remove uncommmented code-snippets.

#### 10. Consistence use of NatSpec
The consistent and correct usage of NatSpec-type comments is recommended.


#### 11. `locked` variable in RubiconMarket not marked as public/internal/private
```solidity
// add public/internal or private
 bool locked;
```

#### 12. Substract minus 1 from storage slot variable in Rubicon Market

Follow best practices for strage slot variables

```solidity
// change to the following
bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE")-1;
```
See: https://ethereum-magicians.org/t/eip-1967-standard-proxy-storage-slots/3185/4?u=frangio
