Lack of proper validation checks for the `feeType` and `feeValue` parameters in the `calculateFee` function.
===============================================================================================================
## Impact
The `calculateFee` function in the contract `FeeWrapper.sol` does not perform proper validation for the `feeType` and `feeValue` parameters. If a third-party application does not set appropriate validation for these parameters as well and sets them to the value `0`, it will cause the calculation `fees[i] = (tokenAmounts[i] * feeValue) / feeType` to revert. This will cause the `calculateFee` function to not work as expected and possibly result in incorrect fee calculations.

## Proof of Concept
Affected code:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L28-#L44
Foundry test:
Add this lines to the file `ProtocolDeployment.t.sol`
```solidity
import "../../contracts/utilities/FeeWrapper.sol";

  function testCalculateFee() public{
    uint256[] memory tokenAmounts = new uint256[](3);
    tokenAmounts[0] = 100;
    tokenAmounts[1] = 200;
    tokenAmounts[2] = 300;

    uint256 feeType = 0; // Invalid value
    uint256 feeValue = 5;
    //uint256 feeValue = 0; //uncomment this line to test for feeValue being zero

    FeeWrapper feeWrapper = new FeeWrapper();

    (uint256[] memory amountsWithFee, uint256[] memory fees) = feeWrapper.calculateFee(tokenAmounts, feeType, feeValue);

     //Assert that fees array contains only 0 values due to the invalid feeType
    for (uint256 i = 0; i < fees.length; i++) {
      assert(fees[i] == 0);
    }

  }

```

Foundry Output:
```Foundry

Failing tests:
Encountered 1 failing test in test/foundry-tests/ProtocolDeployment.t.sol:ProtocolDeploymentTest
[FAIL. Reason: Division or modulo by 0] testCalculateFee() (gas: 610467)
  [610467] ProtocolDeploymentTest::testCalculateFee()
    ├─ [575206] → new FeeWrapper@0x15cF58144EF33af1e14b5208015d11F9143E27b9
    │   └─ ← 2873 bytes of code
    ├─ [1407] FeeWrapper::calculateFee([100, 200, 300], 0, 5) [staticcall]
    │   └─ ← "Division or modulo by 0"
    └─ ← "Division or modulo by 0"
```

## Tools Used
* Foundry
* Manual Review

## Recommended Mitigation Steps
To ensure accurate calculation and prevent reverting, it is recommended to add proper validation checks for the `feeType` and `feeValue` parameters. This can be done by adding a check at the beginning of the function to ensure that both `feeType` and `feeValue` are greater than 0 before proceeding with the calculation. The code below shows the modified function:

```solidity
function calculateFee(
    uint256[] memory tokenAmounts,
    uint256 feeType,
    uint256 feeValue
)
    external
    view
    returns (uint256[] memory amountsWithFee, uint256[] memory fees)
{
    require(feeType > 0 && feeValue > 0, "Fee type and value must be greater than 0");

    amountsWithFee = new uint256[](tokenAmounts.length);
    fees = new uint256[](tokenAmounts.length);

    for (uint256 i = 0; i < tokenAmounts.length; ++i) {
        fees[i] = (tokenAmounts[i] * feeValue) / feeType;
        amountsWithFee[i] = tokenAmounts[i] - fees[i];
    }
}
```


Missing Re-entrancy lock in `offer()` functions in the contract `RubiconMarket.sol`
===================================================================================
## Impact
  Three `offer` functions in the contract do not include a reentrancy lock. This means that an attacker could potentially trigger a reentrancy attack on the contract by calling one of these functions multiple times before the previous call completes. Although the impact of such an attack is low due to the limited functionality of the offer functions, it could still result in unexpected behaviour, such as the duplication of orders or the loss of funds.

## Proof Of Concept
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L760-#L822

`offer` function that has implemented a re-entrancy lock:
* https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L824-#L852

## Recommended Mitigation Steps
To mitigate this vulnerability, a reentrancy lock should be added to all offer functions. This can be achieved by adding a locked boolean variable that is set to true at the beginning of the function and reset to false at the end of the function. Additionally, the locked variable should be checked at the beginning of the function to ensure that no reentrancy attack is in progress.

Example:
```solidity
bool private locked;

function offer(
    uint256 pay_amt,
    ERC20 pay_gem,
    uint256 buy_amt,
    ERC20 buy_gem
) external can_offer returns (uint256) {
    require(!locked, "Reentrancy attempt");
    locked = true;
    // function body here
    locked = false;
}

function offer(
    uint256 pay_amt,
    ERC20 pay_gem,
    uint256 buy_amt,
    ERC20 buy_gem,
    uint pos,
    bool rounding
) external can_offer returns (uint256) {
    require(!locked, "Reentrancy attempt");
    locked = true;
    // function body here
    locked = false;
}

function offer(
    uint256 pay_amt,
    ERC20 pay_gem,
    uint256 buy_amt,
    ERC20 buy_gem,
    uint256 pos,
    address owner,
    address recipient
) external can_offer returns (uint256) {
    require(!locked, "Reentrancy attempt");
    locked = true;
    // function body here
    locked = false;
}


```


