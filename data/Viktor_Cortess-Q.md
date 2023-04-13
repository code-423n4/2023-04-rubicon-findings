## [NC] NAMED IMPORTS CAN BE USED

It’s possible to name the imports to improve code readability for all files in scope.


## [NC] Outdated compiler version 

**contracts\RubiconMarket.sol pragma solidity ^0.8.9;**
**contracts\periphery\BathBuddy.sol pragma solidity ^0.8.0;**

## [NC] SOME FUNCTIONS AND VARIABLES ARE NEVER USED OR USING OF THEM DOESN’T MAKE SENSE

It’s confusing to see some functions and boolean variables that are supposed to perform important functions without use.

**contracts\RubiconMarket.sol**

Boolean stopped is never used.

    596: bool public stopped;
    630: function stop() external auth {
        stopped = true; 
    }

buyEnabled and matchingEnabled are always true and there is no possibility to change them to false.
So using them as a method to prevent smth doesn’t make sense. 

    677: bool public buyEnabled = true; //buy enabled TODO: review this decision!
        bool public matchingEnabled = true; //true: enable matching,
    622: function isClosed() public pure returns (bool closed) {
        return false; 
    }

Also changing the logic of work of these boolean variables or deleting them could save some gas.

## [L] USE SAFETRANSFEROWNERSHIP INSTEAD OF TRANSFEROWNERSHIP FUNCTION

Recommend implementing a two-step process where the owner or admin nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to succeed. This ensures the nominated EOA account is a valid and active account.

**contracts\RubiconMarket.sol**

    26: function setOwner(address owner_) external auth {
        owner = owner_; //@LOW 2 step owner
        emit LogSetOwner(owner);
    }

## [L] PREVENT DIVISION BY 0

On several locations in the code precautions are not being taken for not dividing by 0, this will revert the code.
These functions can be called with a 0 value in the input, this value is not checked for being bigger than 0, which means in some scenarios this can potentially trigger a division by zero.

**contracts\RubiconMarket.sol**

    320: uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt

contracts\utilities\FeeWrapper.sol

    42: fees[i] = (tokenAmounts[i] * feeValue) / feeType

## [L] INITIALIZERS COULD BE FRONT-RUN AND CALLED BY ANY PERSON.

**contracts\periphery\BathBuddy.sol**

In the pre-contest test, there were already mentioned the problem with initializers, but they didn’t include the spawnBuddy function, which works like an initializer and should be called from the BathHouseV2 contract, but can be called by anybody.

    71: function spawnBuddy(
        address _owner,
        address newBud,
        address _bathHouse
    ) external { 
        require(!friendshipStarted, "I already have a buddy!");
        owner = _owner;
        myBathTokenBuddy = newBud;
        bathHouse = _bathHouse;

        // Note, rewards duration must be set by admin

        // Constructor pattern
        friendshipStarted = true;
    }
