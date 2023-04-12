Remove commented out/unused code that does not provide information.
---


This will increase code legibility and decrease total Solc

Instances include:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L128-L150

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1343-L1359


The Error Message in can_cancel modifier is long, unclear and confusing
---

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L719-L728

The error message is inaccurate and does not take into account the actual logic defined in the modifier

```
    modifier can_cancel(uint256 id) override {
        require(isActive(id), "Offer was deleted or taken, or never existed.");
        require(
            isClosed() || msg.sender == getOwner(id) || id == dustId,
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );
        _;
    }
```


It can be improved to this.


```
    modifier can_cancel(uint256 id) override {
        require(isActive(id), "Offer was deleted or taken, or never existed.");
        require(
            isClosed() || msg.sender == getOwner(id) || id == dustId,
            "Cannot cancel. Either user is not owner, market is open, or offer sells required tokens."
        );
        _;
    }

```

The function kill() in RubiconMarket.sol performs the exact same function as cancel() and is never used
--


```
    function kill(bytes32 id) external virtual {
        require(cancel(uint256(id)));
    }

```

Consider removing this function as it is never used. 

Remove Unused Modifier onlyBuddy()
--

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L94

```
  modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }
```

Consider removing this function from the codebase as it is never used.


Make consistent use of openzeplinn transfer functions
--


Some functions make use of [transfer](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L380)  while others make use of [safetransfer](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L182)

Best practice is to remain consistent where possible to increase code readability and avoid unforseen events.  