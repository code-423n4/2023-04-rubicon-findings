### Gas Optimizations List
| Number | Optimization Details | Context | Saved gas |
|:--:|:-------| :-----:| :-----:|
| [G-01] | Functions guaranteed to revert when called by normal users can be marked payable | 14 | -120 |
| [G-02] | Setting the constructor to payable | 2 | -24 |
| [G-03] | Variable declaration outside the loop to save gas | 1 | -600 |
| [G-04] | Multiple accesses of a mapping/array should use a local variable cache | 36 | -2181 |
| [G-05] | Use nested if and, avoid multiple check combinations | 6 | -470 |
| [G-06] | Use assembly to write address storage values | 7 | -5868 |
| [G-07] | use x = x + y cost less than x += y | 3 | -428 |

Total 7 issues
### Note
The gas estimations presented in this report are based on the default testing script given by the sponsor.

### [G-01] Functions guaranteed to revert when called by normal users can be marked payable (~120 gas)

If a function modifier or require such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are ``CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2)`` which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

There are 14 instances of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L958
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1476
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L60
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L232
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L93
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L107
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L226
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L242


**Recommendation:**
I suggest that you make all functions protected by the onlyOwner or auth or onlyAdmin modifier ``payable``.


### [G-02] Setting the constructor to payable (24 gas)

You can cut out 10 opcodes in the creation-time EVM bytecode if you declare a constructor payable. Making the constructor payable eliminates the need for an initial check of ``msg.value == 0`` and saves 13 gas on deployment with no security risks.

There are 2 instances of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L54


**Recommendation:**
set all the constructors to ``payable``.


### [G-03] Variable declaration outside the loop to save gas (~600 gas)

By putting the variable declaration inside the loop the EVM will need to redeclare the variable every iteration. Therefore, it is preferable to declare it outside the loop and use it inside to save gas.

There is 1 instance of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L160

**Recommendation:**
Declare the variable outside the loop and use it inside to save gas.



### [G-4] Multiple accesses of a mapping/array should use a local variable cache (2181)
Caching a mapping’s value in a storage pointer when the value is accessed multiple times saves ~40 gas per access due to not having to perform the same offset calculation every time. Help the Optimizer by saving a storage variable’s reference instead of repeatedly fetching it.

There are 36 instances of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L334
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L335
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L942
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L943
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1045
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1049
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1051
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1052
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1053
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1058
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1085
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1089
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1091
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1092
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1093
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1100
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1130
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1131
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1132
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1143
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1163
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1164
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1165
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1176
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1185
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1199
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1211
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1212
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1413
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1419
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1420
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1423
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1426
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1428
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1429
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1433


**Recommendation:**
Here is an example of the kind of changes you need to make:
File: \contracts\RubiconMarket.sol

Change from this:
```solidity
    function _unsort(
        uint256 id //id of maker (ask) offer to remove from sorted list
    ) internal returns (bool) {
        address buy_gem = address(offers[id].buy_gem);
        address pay_gem = address(offers[id].pay_gem);
        require(_span[pay_gem][buy_gem] > 0);

        require(
            _rank[id].delb == 0 && //assert id is in the sorted list
                isOfferSorted(id)
        );

        if (id != _best[pay_gem][buy_gem]) {
            // offers[id] is not the highest offer
            require(_rank[_rank[id].next].prev == id);
            _rank[_rank[id].next].prev = _rank[id].prev;
        } else {
            //offers[id] is the highest offer
            _best[pay_gem][buy_gem] = _rank[id].prev;
        }

        if (_rank[id].prev != 0) {
            //offers[id] is not the lowest offer
            require(_rank[_rank[id].prev].next == id);
            _rank[_rank[id].prev].next = _rank[id].next;
        }

        _span[pay_gem][buy_gem]--;
        _rank[id].delb = block.number; //mark _rank[id] for deletion
        return true;
    }
```

to the following
```Solidity
    function _unsort(
        uint256 id //id of maker (ask) offer to remove from sorted list
    ) internal returns (bool) {
        address buy_gem = address(offers[id].buy_gem);
        address pay_gem = address(offers[id].pay_gem);
        require(_span[pay_gem][buy_gem] > 0);
        sortInfo storage r = _rank[id];
        require(
            r.delb == 0 && //assert id is in the sorted list
                isOfferSorted(id)
        );

        if (id != _best[pay_gem][buy_gem]) {
            // offers[id] is not the highest offer
            require(_rank[r.next].prev == id);
            _rank[r.next].prev = r.prev;
        } else {
            //offers[id] is the highest offer
            _best[pay_gem][buy_gem] = r.prev;
        }

        if (r.prev != 0) {
            //offers[id] is not the lowest offer
            require(_rank[r.prev].next == id);
            _rank[r.prev].next = r.next;
        }

        _span[pay_gem][buy_gem]--;
        r.delb = block.number; //mark _rank[id] for deletion
        return true;
    }
```


### [G-5] Use nested if and, avoid multiple check combinations (470)
Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

There are 6 instances of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L349
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L893
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L940
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1324
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L95
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L392

**Recommendation:**
Change this:
```solidity
if(test1 && test2){
    ...
}
```
To the following:
```solidity
if(test1){
    if(test2){
        ...
    }
}
```

whenever it is possible in the code to save gas.

### [G-6] Use assembly to write address storage values (5868)

There are 7 instances of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L705
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L707
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1478
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L35
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L36
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L78
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L79


**Recommendation:**
Example of changes you should put in place:

File: BathHomuseV2
```solidity
admin = msg.sender;
proxyAdmin = _pAdmin;
```
To:
```solidity
assembly {                      
    sstore(admin.slot, caller())
    sstore(proxyAdmin.slot, _pAdmin)
}
```

### [G-7] The use of x = x + y cost less than x += y (428)

There are 3 instances of the topic:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L583
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L586
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L184

