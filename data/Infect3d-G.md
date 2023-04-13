# Gas Optimizations

## G-01 Requirement statement could be split to reduce gas in some cases
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L251
There's no need to check both case fo ``msg.sender``, either its the owner of the id, or not.
This could be refactored as shown in the code snippet below.
```solidity
File: contracts\RubiconMarket.sol
251:     modifier can_cancel(uint256 id) virtual {
252:         
253:         if(getOwner(id) == address(0))  
254:             require(msg.sender == getRecipient(id));
255:         else require(getOwner(id) == msg.sender);
256: 
257:         // require(isActive(id));
258:         // require(
259:         //     (msg.sender == getOwner(id)) ||
260:         //         (msg.sender == getRecipient(id) && getOwner(id) == address(0))
261:         // );
262:         _;
263:     }


```

## G-02 Cheaper to use uint256 as boolean values for `locked` state variable in RubiconMarket.sol
When you want to store a true/false value, the first instinct is naturally to store it as a boolean. However, because the EVM stores boolean values as a uint8 type, which takes up two bytes, it is actually more expensive to access the value. This is because it EVM words are 32 bytes long, so extra logic is needed to tell the VM to parse a value that is smaller than standard.The most efficient way to store true/false values, then, is to use 1 for false and 2 for true.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L225
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L594
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L679
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L18

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L49


```solidity
File: contracts\RubiconMarket.sol
225:     bool locked;

594:     bool public stopped;

679:     bool public initialized;

682:     bool public AqueductDistributionLive;

```

```solidity
File: contracts\BathHouseV2.sol
18:     bool private initialized;

```

```solidity
File: contracts\periphery\BathBuddy.1.sol
49:     bool public friendshipStarted;
```


## G-03 Using storage instead of memory for structs/arrays saves gas
When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L291
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L318
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L455


```solidity
File: contracts\RubiconMarket.sol
291:         OfferInfo memory _offer = offers[id];

```
```solidity
File: contracts\RubiconMarket.sol
318:         OfferInfo memory _offer = offers[id];

```

```solidity
File: contracts\RubiconMarket.sol
455:         OfferInfo memory _offer = offers[id];


```



## G-04 Two comparisons could be simplified to only one
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L459

There's two possible cases for _offer owner and recipient:
- msg.sender == _offer.owner 
- msg.sender == _offer.recipient && _offer.owner == address(0)

Depending on that, either its L460 or L461 that will be executed.

```solidity
File: contracts\RubiconMarket.sol
459:         _offer.owner == address(0) && msg.sender == _offer.recipient
460:             ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
461:             : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

```
We can change the check to this (and inverse the result of check), which will remove one zero check and one comparison:
```solidity
        msg.sender == _offer.owner
            ? require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt))
            : require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt)),

```

## G-05 Unused modifier
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297
Remove unused code to reduce deployment cost

##### 
```solidity
File: contracts\RubiconMarket.sol
296: 
297:     function bump(bytes32 id_) external can_buy(uint256(id_)) {
298:         uint256 id = uint256(id_);

```

## G-06 msg.sender == address(0) check is unnecessary, msg.sender cannot be 0
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L97

```solidity
File: contracts\periphery\BathBuddy.sol
97:                 msg.sender != address(0) &&

```
## G-06 Compare array length to avoid out of bounds
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30
This will prevent gas loss in case of an out of bounds error

```solidity
File: contracts\V2Migrator.1.sol
30:     constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
31:         for (uint256 i = 0; i < bathTokensV1.length; ++i) {
32:             // set v1 to v2 bathTokens
33:             v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
34:         }
35:     }

```