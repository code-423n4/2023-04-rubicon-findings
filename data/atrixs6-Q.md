### The return parameter may cause confusion.

[RubiconMarket.sol#L284]("https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L284")
```
    function getRecipient(uint256 id) public view returns (address owner) {
        return offers[id].recipient;
    }
```

Suggest changing ```address owner``` to ```address recipient```:
```
    function getRecipient(uint256 id) public view returns (address recipient) {
        return offers[id].recipient;
    }
```