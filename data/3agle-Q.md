### Issues Template
| Letter | Name | Description |
|:--:|:-------:|:-------:|
| L  | Low risk | Potential risk |
| NC |  Non-critical | Non risky findings |
| R  | Refactor | Changing the code |
| O | Ordinary | Often found issues |

| Total Found Issues | 5 |
|:--:|:--:|

### Low Risk Template
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [L-01] | Array length Mismatch | 2 |
| [L-02] | Upperbound limit on fees | 2 |

| Total Low Risk Issues | 2 |
|:--:|:--:|

### Refactor Template
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [R-01] | Implement Checks-Effects-Interactions | 1 |

| Total Refactor Issues | 1 |
|:--:|:--:|

### Ordinary Issues Template
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [O-01] | Floating pragma | 2 |
| [O-02] | Use a more recent pragma version | 6 |

| Total Ordinary Issues | 2 |
|:--:|:--:|

### [L-01] Array length mismatch
1. The function `batchRequote()` at [L917-L933](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933) does not implement a check to ensure that the array length match. So as to avoid any reverts and waste gas it is recommended to apply following fix:

```solidity
contracts/RubiconMarket.sol

    function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
	    require(
	            payAmts.length == payGems.length &&
                payAmts.length == buyAmts.length &&
                payAmts.length == buyGems.length,
	            "Array lengths do not match"
        );
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

2. The function `claimRewards()` at [L115-L128](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L115-L128) does not implement a check to ensure that the array length match. So as to avoid any reverts and waste gas it is recommended to apply following fix. 

```solidity
contracts/BathHouseV2.sol

      function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {
        require(buddies.length == rewardsTokens.length, "Array length mismatch");
        
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);

        // get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }
```



### [L-02] Upperbound limit on fees
It is recommended to add an upperbound limit on the fee setters at following instances:

[L1466-L1469](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1466-L1469) & [L1471-L1474](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1471-L1474)
```solidity
contracts/RuiconMarket.sol

...

function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
	feeBPS = _newFeeBPS;
	return true;
}

...

function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
	StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
	return true;
}


```



### [R-01] Implement Checks-Effects-Interactions
The function `_chargeFeePayable()` does not implement checks-effects-interactions pattern.
Recommend to implement C-E-I pattern as follows to prevent any reentrancy issues

```solidity
contracts/utilities/FeeWrapper.sol

    function _chargeFeePayable(
        FeeParams memory _feeParams
    ) internal returns (uint256 _msgValue) {
	    //~~Checks~~
	    
		// _feeToken is ETH
        uint256 _totalAmount = _feeParams.totalAmount;
        uint256 _feeAmount = _feeParams.feeAmount;
        address _feeTo = _feeParams.feeTo;
        require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
		
		//~~Effects~~
		
		_msgValue = msg.value - _feeAmount;

		//~~Interactions~~
		
        // transfer fee to the 3rd party protocol
        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
        require(OK, "ETH transfer failed");
        

    }
```


### [O-01] Floating pragma
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Instances:
```solidity
contracts/RubiconMarket.sol
contracts/periphery/BathBuddy.sol
```

### [O-02] Use a more recent pragma version
Old version of solidity is used, consider using the new one `0.8.19`.
You can see what new versions offer regarding bug fixed [here](https://github.com/ethereum/solidity/blob/develop/Changelog.md)

Instances - All of the contracts.
