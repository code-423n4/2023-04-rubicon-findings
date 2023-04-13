QA1.
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

QA2.
### The proxyadmin contract and logic contract may not be initialized.

There is no constructor function in the contract, only an initialize function. Both the proxyadmin contract and the logic contract need to be initialized when deploying the contract.

[RubiconMarket.sol]('https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol')
[BathHouseV2.sol]('https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol')

Suggest using the [Initializable contract]('https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/62aa67491a9b46a21c2b5e3600560150a2710f98/contracts/proxy/utils/Initializable.sol') provided by OpenZeppelin and adding a constructor function:
```solidity
constructor() {
    _disableInitializers();
}
```
This can better avoid security issues caused by uninitialized contracts.