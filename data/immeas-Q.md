# low

## L-01 FeeWrapper will fail of there is no fee taken
In FeeWrapper:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102
```solidity
File: utilities/FeeWrapper.sol

102:        IERC20(_feeToken).transfer(_feeTo, _feeAmount);
```
If `_feeToken` is `address(0)` or `_feeAmount` is `0` (for some tokens) the above row will revert.
This puts it on the implementer to do the call themselves in the case they don't want to charge any fee for certain calls.

### Recommended Mitigation Steps
Add check if feeAmount > 0 before transferring tokens.

## L-02 positions might not be able taken
When opening a Position, the last leverage might be very little:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L264
```solidity
File: utilities/poolsUtility/Position.sol

264:        _toBorrow = wmul(_maxBorrow(_bathTokenQuote), _toBorrow);
```
If this is `<_dust` this will cause the opening of the position to revert in `RubiconMarket` when the trade is later done.

### Recommended Mitigation Steps
Add a check if the trade is `<_dust` and don't do it. This will cause the leverage to be a little under the requested but just dust.

## L-03 fees can be set to any value
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1474
```solidity
File: RubiconMarket.sol

1466:    function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1467:        feeBPS = _newFeeBPS;
1468:        return true;
1469:    }
1470:
1471:    function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1472:        StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
1473:        return true;
1474:    }
```
If fee is set too high it will cause `calcAmountAfterFee` to underflow

### Recommended Mitigation Steps
Add reasonable checks that fee isn't set to something enormous, like between 0 and 25% or something. Possibly hide fee changing behind a timelock as well to increase trust in the protocol.

## L-04 Confusing that `makerFee` is not BPS
Easy to set fee of by 10% since its traditional that fees are BPS unless otherwise stated.


## L-05 Consider using a two factor ownership transfer
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28
Consider adding a two-phase transfer, where the current owner nominates the next owner, and the next owner has to call accept*() to become the new owner. This prevents passing the ownership to an account that is unable to use it.

## L-06 no check for address(0) when changing ownership
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L25-L28
Consider adding a check for `address(0)` when changing owner to not accidentally throw away ownership.

# nc

## NC-01 Lots of commented out code
Makes the code untidy and hard to overview.

`EventfulMarket` as an example:
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L99-L207

Please consider removing these to give a better scrolling experience.

## NC-02 Unnecessary assembly
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L367-L378
```solidity
        assembly {
            switch _initBathTokenAmount
            case 0 {
                _bathTokenAmount := _currentBathTokenAmount
            }
            default {
                _bathTokenAmount := sub(
                    _currentBathTokenAmount,
                    _initBathTokenAmount
                )
            }
        }
```
can be rewritten as:
```solidity
if(_initBathTokenAmount==0) {
	_bathTokenAmount = _currentBathTokenAmount;
} else {
	_bathTokenAmount = _currentBathTokenAmount - _initBathTokenAmount;
}
```
add unchecked if you need it.

## NC-03 Unnecessary return parameter names

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L58-L72
```solidity
    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        return x <= y ? x : y;
    }

    function max(uint256 x, uint256 y) internal pure returns (uint256 z) {
        return x >= y ? x : y;
    }

    function imin(int256 x, int256 y) internal pure returns (int256 z) {
        return x <= y ? x : y;
    }

    function imax(int256 x, int256 y) internal pure returns (int256 z) {
        return x >= y ? x : y;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L276-L286
```solidity
    function isActive(uint256 id) public view returns (bool active) {
        return offers[id].timestamp > 0;
    }

    function getOwner(uint256 id) public view returns (address owner) {
        return offers[id].owner;
    }

    function getRecipient(uint256 id) public view returns (address owner) {
        return offers[id].recipient;
    }
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620
```solidity
    function isClosed() public pure returns (bool closed) {
        return false;
    }
```

## NC-04 `SimpleMarket::bump` doesn't do anything

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L297-L310

```solidity
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

## NC-05 Consider using scientific notation for increased readability

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L74-L75
```solidity
    uint256 constant WAD = 10 ** 18;
    uint256 constant RAY = 10 ** 27;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L317
```solidity
        _max = (_liq.mul(10 ** 18)).div(_price);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L331
```solidity
        ).div(10 ** 18);
```

## NC-06 declare visibility for fields

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L224
```solidity
File: RubiconMarket.sol

224:    bool locked;
```

## NC-07 events in `EventfulMarket` don't follow standard naming rules
Events should start with capital letter
https://docs.soliditylang.org/en/v0.8.17/style-guide.html#event-names

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L118
```solidity
    event emitOffer(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L152
```solidity
    event emitTake(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L174
```solidity
    event emitCancel(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L198
```solidity
    event emitFee(
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L209
```solidity
    event emitDelete(
```

## NC-08 Unused parameters
`_asset` is unused
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L528


## NC-09 Function can be view
`_borrowLimit` can be `view`
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526


## NC-10 Unnecessary parentheses
`(borrowBalance(bathTokenQuote))`
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L195

## NC-11 unused imports
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L5
```solidity
import "./RubiconRouter.sol";
```
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L4

## NC-12 unused modifier
Unused code clutters the contract and impeeds on readability and quick understanding
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94-L102
```solidity
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

## NC-13 comment doesn't describe anything
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1270
```solidity
    //these variables are global only because of solidity local variable limit
```