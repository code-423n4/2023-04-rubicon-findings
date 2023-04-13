# Gas Optimization

## Gas-01 Change the sequence of conditions in `require`

Change the sequence of conditions in the `require` function. Put the most used condition on the left side to optimize the gas cost of the function. 

If the condition matches then the opcode will skip other conditions. And it can optimize the gas cost. 

```diff
// // After close, anyone can cancel an offer
    modifier can_cancel(uint256 id) override {
        require(isActive(id), "Offer was deleted or taken, or never existed.");
+        require((msg.sender == getRecipient(id) && getOwner(id) == address(0)) || msg.sender == getOwner(id) || id == dustid || isClosed());
-				require(
-            isClosed() ||
-                msg.sender == getOwner(id) ||
-                id == dustId ||
-                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
-            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
-        ); // @audit gas put the sequence as this to improve the gas consumption: (msg.sender == getRecipient(id) && getOwner(id) == address(0)) || msg.sender == getOwner(id) || id == dustid || isClosed()
        _;
    }
```

## Gas-02 Reduce the `public` functions to improve gas cost

In the code below, there are many variables which are with the `public` key word. This increases the `public` function in the contract and as a result it increases the cost of calling the `public` functions in the contract from external accounts. The reason can be seen [here](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92). 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L12-L19](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L12-L19)

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L19-L48](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L19-L48)

### Recommendation

Reducing the `public` function can improve the gas cost of calling functions. But at the same time, the convenience of checking the contract should be also considered, so you can set the variable as `internal` or `private` and add a getter function to achieve this. 

## Gas-03 Use the existing variable as flag can improve gas cost

We can use other state variable instead of creating a new `bool friendshipStarted` to let the contract only initiate once. 

For instance, the code below if from `BathBuddy.sol` the state variable `bool friendshipStarted` is created for the purpose of initiating the contract only once. We noticed that the state variables ilke `owner` and `bathHouse` are set as non-zero when initiated so we can just use them as a flag to check if the contract is initiated or not and we do not need to create a new state variable. Hence, in this way it saves gas. 

```solidity
File: BathBuddy.sol
70:     // Proxy-safe constructor
71:     function spawnBuddy( //@audit non-critical - the implementation can get initialized by anyone. And this is risking getting initialized by a malicious attacker. In the worst case, the attacker can make the implementation destruct itself. This will make the proxy unusable. The attacker can put his malicious token as the argument `newBut` and then call the 
72:         address _owner,
73:         address newBud,
74:         address _bathHouse
75:     ) external {
76:         require(!friendshipStarted, "I already have a buddy!"); // @audit gas - can use bathHouse as a flag to check if the contract is initialized. We can reduce storage by 1 slot in this case. `require(myBathTokenBuddy == address(0), "Already spawned!");`
77:         owner = _owner;
78:         myBathTokenBuddy = newBud;
79:         bathHouse = _bathHouse;
80: 
81:         // Note, rewards duration must be set by admin
82: 
83:         // Constructor pattern
84:         friendshipStarted = true;
85:     }
86:
```

### Recommendation

```diff
// Proxy-safe constructor
    function spawnBuddy( 
        address _owner,
        address newBud,
        address _bathHouse
    ) external {
-       require(!friendshipStarted, "I already have a buddy!"); // @audit gas - can use bathHouse as a flag to check if the contract is initialized. We can
+       require(bathHouse == address(0), "Buddy already spawn!"); 
        owner = _owner;
        myBathTokenBuddy = newBud;
        bathHouse = _bathHouse;

        // Note, rewards duration must be set by admin

        // Constructor pattern
        friendshipStarted = true;
    }
```

## Gas-04 Use library to improve gas cost

I suggest use OpenZeppelin’s library `ReentrancyGuard` to [improve the gas cost as explained in the library](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/86f6eb2c9c0eb741de18ef75a290ff340acf7148/contracts/security/ReentrancyGuard.sol#L23-L33). In the protocol the code related to Reentrancy is as below: 

```diff
File: RubiconMarket.sol
837:         require(!locked, "Reentrancy attempt"); // @audit use openzeppelin's library to improve the gas efficiency. Because OpenZeppelin uses uint256 instead of bool to handle the reentrancy guard.
```

```diff
File: RubiconRouter.sol
57:         require(!locked);
```

Instead of using `bool`, the library uses `uint256` type of variables, i.e. 1, 2, to represent the state of calling. And it is very gas efficient. Adopting the library can have a huge gas impact on the functions which are supposed to be called very often by the user especially in the contract `RubiconMarket`.

### Recommendation

Adopt Openzeppelin’s `ReentrancyGuard` library. 

[https://github.com/OpenZeppelin/openzeppelin-contracts/blob/86f6eb2c9c0eb741de18ef75a290ff340acf7148/contracts/security/ReentrancyGuard.sol#L23-L33](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/86f6eb2c9c0eb741de18ef75a290ff340acf7148/contracts/security/ReentrancyGuard.sol#L23-L33)

## Gas-05 Change the variables into `immutable` to save gas

The related code is this: 

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L44-L50](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L44-L50)

If you followed my QA report’s NC-18 to change the BathBuddy’s `spawnBuddy` to `constructor`. Then it is good to conduct the change below to save more gas. Because `immutable` variables can only be set in the `constructor`.

```diff
-	 address public owner;
-        address public myBathTokenBuddy;
-        address public bathHouse;
+	 address public immutable owner;
+       address public immutable myBathTokenBuddy;
+       address public immutable bathHouse;
```