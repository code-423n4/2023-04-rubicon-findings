## 1. Should use minimal proxy pattern to save a huge amount of gas
Current structure of protocol newly deploys [`Position`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol) contract each time someone wants to create it.

This pattern can be replaced by a minimal proxy pattern, where Implementation is deployed once and then only a minimal proxy is created which point to that implementation each time it needs to be created.

This saves a huge deployment cost each time as the deployment cost of proxy is very low.

## 2. Consider implementing a 2-step ownership transfer in [`RubiconMarket.sol`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)
If the owner of RubiconMarket accidentally transfers ownership to an incorrect address, protected functions may become permanently inaccessible.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28
```
    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
```
Suggestion: Handle ownership transfers with two steps and two transactions. First, allow the current owner to propose a new owner address. Second, allow the proposed owner (and only the proposed owner) to accept ownership, and update the contract owner internally.


## 3. No upper limit on `feeBPS` and `makerFee`
There is no upper limit on `feeBPS` and `makerFee`, which can allow the owner to set it to any high arbitrary value.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1469
```
    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS;
        return true;
    }
```
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471-L1475
```
    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
        return true;
    }
```

## 4. Lack of zero address check and missing configuration function can lead to redeployment of contract
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/PoolsUtility.sol#L26-L39

In `intialize()` there is no check for zero address. Once it's set to any address, there's no configuration functions to change it.
```
    function initialize(
        address _oracle,
        address _rubiconMarket,
        address _bathHouseV2
    ) external {
        require(!initialized, "Contract already initalized!");
        coreAddresses = CoreAddrresses({
            oracle: _oracle,
            rubiconMarket: _rubiconMarket,
            bathHouseV2: _bathHouseV2
        });


        initialized = true;
    }
```
The Position creation function uses these addresses to deploy a Position contract. So, this misconfiguration ultimately renders all new Position contracts useless
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/PoolsUtility.sol#L59
```
        Position position = new Position(oracle, rubiconMarket, bathHouseV2);
```

## 5. Wrong value gets emitted in the event
In RubiconMarket, Offer.owner will be address(0) in case of the old version struct as this has been added in new version.

So, address(0) gets emitted in a certain event in case of old version struct.

`emitFee`
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L368

`emitTake`
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L398-L407

`emitDelete`
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L440-L445

`emitCancel`
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L474-L482






