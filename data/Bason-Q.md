## Low and Non-Critical Issues Summary
| Number |Issues Details |
|:--:|:-------|
|[NC-01]| Use stable pragma statement
|[NC-02]| Use latest Solidity version
|[NC-03]| Use named parameters in mapping types for better readability
|[NC-02]| Add more indexed fields to Events for better traceability
|[L-01]| Missing events for important parameter changes
|[L-02]| Remove isClosed() function since it is always returns false
|[L-03]| Create modifier function to validate array length in batchRequote and batchQuote

## |[NC-01]| Use stable pragma statement

Using a floating pragma statement `^0.8.9` is discouraged as code can compile to different bytecodes with different compiler versions. 
Use a stable pragma statement to get a deterministic bytecode.

## |[NC-02] Use latest Solidity version
It is recommended to use the latest Solidity version in all contract - 0.8.19. Currently, the contracts use 0.8.9.

## |[NC-02] Use named parameters in mapping types for better readability
Add named parameters in all mappings for readability. Here are a few examples
```solidity
mapping(uint256 => OfferInfo) public offers => mapping(uint256 offerId => OfferInfo offerInfo);
mapping(uint256 => sortInfo) public _rank; => mapping (uint256 offerId => sortInfo);
mapping(address => mapping(address => uint256)) public _best => mapping(address bestOfferOwner => mapping(address token => uint256 price))
mapping(address => mapping(address => uint256)) public _span; 
    
```

## |[L-01]| MISSING EVENT FOR IMPORTANT PARAMETER CHANGE

Emitting events allows monitoring activities with off-chain monitoring tools.
I would suggest creating custom events and emitting them on critical parameter changes. I am giving an example for a few files. 

```solidity
Define custom events:
event FeeBPSSet(indexed uint256 newFeeBPS)
event MakerFeeSet(indexed uin256 newMakerFee)
event FeeSetTo(indexed address newFeeTo)

function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
feeBPS = _newFeeBPS;
emit FeeBPSSet(indexed uint256 _newFeeBPS);
return true;
}

function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
emit MakerFeeSet(_newMakerFee);
return true;
}

function setFeeTo(address newFeeTo) external auth returns (bool) {
require(newFeeTo != address(0));
feeTo = newFeeTo;
emit FeeSetTo(newFeeTo);
return true;
}
```
***

## |[L-02] Remove isClosed() function since it is always returns false
No need to have this hardcoded function as of now because it can cause bugs. It needs to be parameterized or actually change state.
Currently, it always returns false

```solidity
function isClosed() public pure returns (bool closed) {
        return false;
}
```
***

## |[L-03]| Create modifier function to validate array length in batchRequote and batchQuote
In the batchQuote and batchRequote function we rely that all arrays have the same length. We must validate this.

```solidity
Define modifier 
modifier validateArrayLength(
    uint256 calldata  payAmts[], 
    uint256 calldata  payGems[], 
    uint256 calldata  buyAmts[], 
    uint256 calldata buyGems[]
) {
    require(
        payAmts.length == payGems.length &&
        payAmts.length == buyAmts.length &&
        payAmts.length == buyGems.length,
            "Array lengths do not match"
        );
}

function batchRequote (
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external validateArrayLenghts(payAmts, payGems, buyAmts, buyGems) {
        
        //We can get this out into modifier since we have the exact same check in batchOffer
        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }
    }
```
***
