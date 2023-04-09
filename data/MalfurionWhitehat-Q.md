# [QA-01] Unnecessary usage of DSMath with Solidity 0.8.x

DSMath is being used unnecessarily with Solidity 0.8.x. The code is relying on DSMath functions that are already included in the latest version of Solidity, resulting in redundant code that can lead to increased gas costs and potential errors. We recommend that the use of DSMath be removed from the smart contract code and replaced with the corresponding built-in Solidity math functions. This will not only improve the efficiency of the code but also ensure compatibility with future versions of Solidity.

# [QA-02] Remove unused functions

Some functions are not used at all.

```solidity
// RubiconMarket.sol

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

# [QA-03] Incorrect parameter emitted on `RubiconMarket.emitFee` event

The parameter `feeTo` is not correct on the `RubiconMarket.emitFee` event emitted on the `SimpleMarket.buy` function. The `feeTo` parameter is always set to `_offer.owner`, while this is only the case for V1 offers. In case of v1, the `feeTo` parameter should be in reality `_offer.recipient`

```solidity
// RubiconMarket.sol

            emit emitFee(
                bytes32(id),
                msg.sender,
                _offer.owner,
                keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
                _offer.buy_gem,
                mFee
            );
```

# [QA-04] Prefix unused variables with an underscore

Some variables on `RubiconMarket.sol` are not used, and could be prefixed with an underscore for better clarity and maintainability:

```solidity
    /// @dev unused deprecated variable for applying a token distribution on top of a trade
    bool public AqueductDistributionLive;
    /// @dev unused deprecated variable for applying a token distribution of this token on top of a trade
    address public AqueductAddress;
```

Replace with

```solidity
    /// @dev unused deprecated variable for applying a token distribution on top of a trade
    bool public _aqueductDistributionLive;
    /// @dev unused deprecated variable for applying a token distribution of this token on top of a trade
    address public _aqueductAddress;
```

# [QA-05] Redundant comparisons on `BathBuddy.sol` smart contract

The `BathBuddy.onlyBuddy` modifier contains a redundant `msg.sender != address(0)` check, that can be left out:

```solidity
 modifier onlyBuddy() {
        require(
            msg.sender == myBathTokenBuddy &&
            // @audit-issue [QA] Unnecessary comparison to address(0)
                msg.sender != address(0) &&
                friendshipStarted,
            "You are not my buddy!"
        );
        _;
    }
```

# [QA-06] `BathBuddy.sol` events do not distinguish for which vested token the event was emitted

Since `BathBuddy` is a generalization of the Synthetix rewards contract, some storage variables were updated to mappings. Nevertheless, the events were not generalized, which make it impossible to distinguish between which token they refer to. This can cause issues with off-chain integrations and monitoring systems:

```solidity
// BathBuddy.sol

    function setRewardsDuration(
        uint256 _rewardsDuration,
        address token
    ) external onlyOwner {
        require(
            block.timestamp > periodFinish[token],
            "Previous rewards period must be complete before changing the duration for the new period"
        );
        rewardsDuration[token] = _rewardsDuration;
        // @audit-issue [QA-60] Must also emit which token got its reward duration updated
        emit RewardsDurationUpdated(rewardsDuration[token]);
    }
```

Change to

```solidity
  emit RewardsDurationUpdated(token, rewardsDuration[token]);
```

# [QA-07] Wrong documentation on `FeeWrapper` regarding `CallParams.args`

The `FeeWrapper.CallParams` struct contains a documentation regarding calldata encoded arguments. It states that arguments can be encoded with either `abi.encode` or `abi.encodePacked`. This is not true. Function arguments MUST be encoded with `abi.encode`

```solidity
// FeeWrapper.sol

    struct CallParams {
        bytes4 selector; // function selector
        // @audit-issue [QA] CallParams MUST be encoded with abi.encode, not abi.encodePacked
        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
        address target; // target contract to call
        FeeParams feeParams;
    }
```

From [Solidity docs](https://docs.soliditylang.org/en/v0.8.17/abi-spec.html):

> Since packed encoding is not used when calling functions, there is no special support for prepending a function selector. Since the encoding is ambiguous, there is no decoding function.


