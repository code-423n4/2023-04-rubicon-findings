## [LC-1] USE LATEST SOLIDITY PRAGMA STABLE VERSION
Latest stable version of solidity adds the benefits of latest bug fixes and security improvements.
see: https://swcregistry.io/docs/SWC-102
Consider using atleast `pragma solidity 0.8.18;`

Files: 
- [contracts/RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)
- [contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)
- [contracts/BathHouseV2.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L2)
- [contracts/V2Migrator.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L2)
- [contracts/utilities/FeeWrapper.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L2)
- [contracts/utilities/poolsUtility/Position.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L2)

#### Recommended Mitigation Steps
Consider using atleast version 0.8.18 `pragma solidity 0.8.18;`.

## [LC-2] AVOID FLOATING PRAGMA
Lock pragma version to ensure contracts are deployed with the same compiler version with which it was tested.
see: https://swcregistry.io/docs/SWC-103
Files: 
- [contracts/periphery/BathBuddy.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2)
- [contracts/RubiconMarket.sol#L2](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)

#### Recommended Mitigation Steps
Change `pragma solidity ^0.8.0;` in `BathBudd.sol` to `pragma solidity 0.8.18;`

## [LC-3] INPUT ARGUMENT `owner` SHADOWS THE STATE VARIABLE `owner`
variable shadowing can lead to critical bugs. The `owner` input argument of 2 `offer` functions of the `RubiconMarket.sol` contract shadows the state variable `owner`.
File: 
- [RubiconMarket.sol#L808](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L808)
- [RubiconMarket.sol#L831](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L831)
- [RubiconMarket.sol#L1280](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1280)

State variable
```
contract DSAuth is DSAuthEvents {
    address public owner;

```

Variable shadowing
```
function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        address owner,
        address recipient
    ) external can_offer returns (uint256) {
        return offer(pay_amt, pay_gem, buy_amt, buy_gem, pos, true, owner, recipient);
    }

    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        bool matching, //match "close enough" orders?
        address owner, // owner of the offer
        address recipient // recipient of the offer's fill
    ) public can_offer returns (uint256) {
        require(!locked, "Reentrancy attempt");
        require(_dust[address(pay_gem)] <= pay_amt);

        /// @dev currently matching is perma-enabled
        // if (matchingEnabled) {
        return _matcho(pay_amt, pay_gem, buy_amt, buy_gem, pos, matching, owner, recipient); //@audit owner shadowed.
            // }
            // return super.offer(pay_amt, pay_gem, buy_amt, buy_gem);
    }
```
```
function _matcho(
        uint256 t_pay_amt, //taker sell how much
        ERC20 t_pay_gem, //taker sell which token
        uint256 t_buy_amt, //taker buy how much
        ERC20 t_buy_gem, //taker buy which token
        uint256 pos, //position id
        bool rounding, //match "close enough" orders?
        address owner,
        address recipient
    ) internal returns (uint256 id) {
```

## [L-4] LACK OF 2 STEP OWNERSHIP CHANGE
Lack of of two steps ownership change for the `setOwner` function. 
The setOwner function sets a new user address in one step. When an owner errornously, sets a wrong address as the new owner the actions is not reversible. But the two step ownership change just like the one implemented in the openzeppelin's 2-step Ownable contracts allows a new owner to accept ownership by sending a confirmation transaction.
File: [RubiconMarket.sol#L25](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25) 
```
function setOwner(address owner_) external auth {
        //@audit 2-step ownership change
        owner = owner_;
        emit LogSetOwner(owner);
    }
```
#### Recommended Mitigation steps
Consider using the 2 step ownership change for critical address changes.
```
/**
     * @dev Starts the ownership transfer of the contract to a new account. Replaces the pending transfer if there is one.
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual override onlyOwner {
        _pendingOwner = newOwner;
        emit OwnershipTransferStarted(owner(), newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`) and deletes any pending owner.
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual override {
        delete _pendingOwner;
        super._transferOwnership(newOwner);
    }

    /**
     * @dev The new owner accepts the ownership transfer.
     */
    function acceptOwnership() public virtual {
        address sender = _msgSender();
        require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
        _transferOwnership(sender);
    }
```

## [NC-1] USE SPECIFIC IDENTIFIERS IN IMPORT DECLARATIONS
Using import declarations of the form `import {<identifier_name>} from "MY/Contract.sol";` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation.
Consider using named imports to import specific objects into files than importing everything from imported files. 

Consider importing this way `import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";`
 instead
`import "@openzeppelin/contracts/security/ReentrancyGuard.sol";`

Files: All files

## [NC-2] REMOVE COMMENTED CODE

Files: 
- [contracts/RubiconMarket.sol#L5](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L5)
- [contracts/RubiconMarket.sol#L100-L116](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L100-L116)
- [contracts/RubiconMarket.sol#L128-L150](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L128-L150)
- [contracts/RubiconMarket.sol#L163-L172](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L163-L172)
- [contracts/RubiconMarket.sol#L184-L195](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L184-L195)
- [contracts/RubiconMarket.sol#L299-309](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L299-309)
- [contracts/RubiconMarket.sol#L386-L396](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L386-L396)
- [contracts/RubiconMarket.sol#L409-L417](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L409-L417)
- [contracts/RubiconMarket.sol#L428-L434](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L428-L434)
- [contracts/RubiconMarket.sol#L464-L472](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L464-L472)
- [contracts/RubiconMarket.sol#L542-L551](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L542-L551)
- [contracts/RubiconMarket.sol#L1346-L1359](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1346-L1359)
- [contracts/utilities/poolsUtility/Position.sol#L263](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L263)


## [NC-3] REMOVE INCOMPLETE FUNCTIONS 
The `bump` function in `RubiconMarket.sol` seems not to do anything. Consider removing it becomes it seems incomplete and has an unused variable `id`.

Files: 
- [contracts/RubiconMarket.sol#L297](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297)

```
function bump(bytes32 id_) external can_buy(uint256(id_)) {
        uint256 id = uint256(id_);
        /// TODO: double check it is sound logic to kill this event
        /// emit LogBump(
        ///     id_,
        ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
        ///     offers[id].owner,
        ///     offers[id].pay_gem,
        ///     offers[id].buy_gem,
        ///     uint128(offers[id].pay_amt),
        ///     uint128(offers[id].buy_amt),
        ///     offers[id].timestamp
        /// );
    }
```
## [NC-4] LINES LONGER THAN RECOMMENDED IN SOLIDITY STYLE GUIDE
Solidity docs style guide recommends maximum length of 120 characters. This codebase contains lines longer than the recommended maximum length.
Consider using prettier formatter to ensure lines are not longer than the recommended max length of 120 characters.

- [contracts/RubiconMarket.sol#L312](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L312)
- [contracts/RubiconMarket.sol#L726](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L726)

## [NC-5] REFACTOR THE AUTH MODIFIER
The `auth` modifier in the `RubiconMarket.sol` file can be made simple by writting the code in the auth modifier instead of writing another `isAuthourized` function that returns true or false.

the auth modifier can be rewritten as below:

```diff
modifier auth() {
-        require(isAuthorized(msg.sender), "ds-auth-unauthorized");
+        if(msg.sender != owner) revert UnAuthorized()
        _;
    }
- function isAuthorized(address src) internal view returns (bool) {
-        if (src == owner) {
-            return true;
-        } else {
-            return false;
-        }
-    }

```

## [NC-6] NO NEED FOR OVERFLOW CHECK IN DSMATH LIBRARY SINCE SOLIDITY VERSION 0.8.* IS USED.
The overflow check in the `DSMath` library is unnecessary since solidity versions from 0.8.0 and above will handle the overflow check by default unless code is unchecked.

Files: 
- [contracts/RubiconMarket.sol#L47](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L47)

```
function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x + y) >= x, "ds-math-add-overflow");
    }

    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x - y) <= x, "ds-math-sub-underflow");
    }

    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require(y == 0 || (z = x * y) / y == x, "ds-math-mul-overflow");
    }
```

## [NC-7] EMIT EVENTS FOR USEFUL STATE CHANGES
Emitting events for useful state changes can help external systems and frontend applications monitor when those events happen.

Files: 
- [RubiconMarket.sol#L628](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L628)

```
function stop() external auth {
        stopped = true; //@audit-info emit event
    }
```
- [RubiconMarket.sol#L1476](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1476)
Emit an event with the `oldFeeTo` address and the `newFeeTo` address.
```
function setFeeTo(address newFeeTo) external auth returns (bool) {
        require(newFeeTo != address(0));
        feeTo = newFeeTo; //@audit-info emit events with old and new values
        return true;
    }

```
```
function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee; //@audit auth can frontrun
        return true;
    }
```
```
function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS; // @audit auth can frontrun, use timelock
        return true;
    }
```
## [NC-8] USE MODIFIERS FOR CHECKS AND VALIDATIONS ONLY. Make `updateReward` modifier a function.
The `updateReward` modifier in `BathBuddy.sol` is used to update state. It is best practice to only use modifiers for checks and validations and NOT state update like: 
file: [contracts/periphery/BathBuddy.sol#L247](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L247)
```
modifier updateReward(address account, address token) {
        rewardsPerTokensStored[token] = rewardPerToken(token);
        lastUpdateTime[token] = lastTimeRewardApplicable(token);
        if (account != address(0)) {
            tokenRewards[token][account] = earned(account, token);
            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
                token
            ];
        }
        _;
    }
```

## [NC-9] INCOMPLETE AND LACK OF NATSPEC COMMENT
The NatSpec comments are either incomplete( without comments about input and return values) or absent.
Consider adding Natspec comments for all functions, constructors and modifiers as recommended by the solidity doc style guide in order to provide rich documentation for the code. see: https://docs.soliditylang.org/en/v0.8.17/natspec-format.html#natspec-format

Files: all files

## [NC-10] USE LATEST STABLE VERSION OF OPENZEPPELIN CONTRACTS
The openzeppelin/contract library in this project used is version 4.8.0 and the latest stable version at this time is version 4.8.2. It is best practice to use latest stable versions of softwares to benefit from the security updates and bug fixes.

## [NC-11] ADD TIMELOCK FOR CRITICAL PARAMETER CHANGE
Consider adding timelock to critical parameter changes like fees to allow users react to the new changes before it happens. This allows users that not okay with the new fees to withdraw within the grace period.
This provides guarantee that users transactions will not be frontran with new fees as they will be notified during the grace period.

Files: [RubiconMarket.sol#L1466-L1480](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1480)
```
function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee; //@audit auth can frontrun
        return true;
    }
```
```
function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
        feeBPS = _newFeeBPS; // @audit auth can frontrun, use timelock
        return true;
    }
```
## [NC-12] ORDER OF FUNCTIONS DO NOT FOLLOW SOLIDITY DOC STYLE GUIDE
According to the Solidity style guide, the order of functions should be grouped according to their visibilities and ordered as below.

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private

Files: 
- [RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)
- [BathHouseV2.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol)