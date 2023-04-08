# [L-01] Outdated Compiler 0.8.9 Used by the Contracts

### Description:

During the analysis it was observed that the contracts code written in Solidity uses an outdated compiler version 0.8.9. The use of an outdated compiler version 0.8.9 poses several security risks that can compromise the security and stability of the smart contract. The following are the key findings of our audit:

- *Code Generation:** Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

Apart from these, there are several minor bug fixes and improvements.

- Assembler: Avoid duplicating subassembly bytecode where possible.
- Code Generator: Avoid including references to the deployed label of referenced functions if they are called right away.
- ContractLevelChecker: Properly distinguish the case of missing base constructor arguments from having an unimplemented base function.
- SMTChecker: Fix internal error caused by unhandled `z3` expressions that come from the solver when bitwise operators are used.
- SMTChecker: Fix internal error when using the custom NatSpec annotation to abstract free functions.
- TypeChecker: Also allow external library functions in `using for`.

### Steps to reproduce:

Navigate to the following contracts and directory:

[`https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2)

```solidity
/// SPDX-License-Identifier: AGPL-3.0
pragma solidity ^0.8.9;
```

### Mitigation

The minimum required version must be [0.8.19]([https://github.com/ethereum/solidity/releases/tag/v0.8.19](https://github.com/ethereum/solidity/releases/tag/v0.8.19)). I strongly recommend that  update the compiler version to the latest version available. This will help to ensure the security and functionality of the contract.

The latest version of the Solidity compiler contains significant improvements and bug fixes, and is also more secure than the previous versions. Updating the compiler version will help to ensure that the contract remains compatible with future updates to the Solidity language.

---

# [L-02] Use _safeMint() instead of _mint()

### Description

During the analysis it was observed that the contract is using function `_mint()` instead of `_safeMint()`. _mint() has the potential to allow an attacker to mint an arbitrary number of tokens, which could result in a supply inflation vulnerability. Additionally, if the contract does not have proper checks and balances in place, _mint() could be used to generate tokens and transfer them to arbitrary addresses, including attacker-controlled addresses.

### Steps to reproduce

Navigate to following URL: [`https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/TokenWithFaucet.sol#L49`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/TokenWithFaucet.sol#L49)

```solidity
// --- Token ---
    function faucet() external returns (bool) {
        if (block.timestamp < faucetCheck[msg.sender] + timeDelay) {
            return false;
        }
        _mint(msg.sender, 10_000 * (10**decimals()));
        faucetCheck[msg.sender] = block.timestamp;
        return true;
    }

    function adminMint() external {
        require(admin == msg.sender);
        _mint(msg.sender, 1000 * (10**decimals()));
    }
```

### Recommendations:

To address these potential vulnerabilities, we recommend using the alternative function, `_safeMint()`. `_safeMint()` offers additional security guarantees by ensuring that the number of tokens minted is always within a specified range, and that tokens are only transferred to addresses that have been properly vetted and approved by the contract.

Based on our analysis of the smart contract, we recommend the following changes to improve its security:

1- Replace `_mint()` with `_safeMint()` to ensure that tokens are only minted and transferred to authorized addresses.

2- Implement proper checks and balances to ensure that only authorized users are able to call `_safeMint()`.

3- Consider adding additional security measures, such as multi-factor authentication or permission-based access controls, to further restrict access to the _safeMint() function.

---

# [L-03] **Deprecated safeApprove() function**

### Description

The **`safeApprove()`** function is commonly used to grant permission for a smart contract to spend a specific amount of a user's tokens. One of the primary issues with **`safeApprove()`** is that it allows an unlimited amount of tokens to be spent once the allowance has been granted. This means that if a smart contract is compromised or has a bug, it can drain the entire balance of tokens that the user has approved.

### Proof of Concept

1. Navigate to following URL: [`https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/MarketAid.sol#L366`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/MarketAid.sol#L366)
    
    ```
    IERC20(toApprove).safeApprove(RubiconMarketAddress, 2 ** 256 - 1);
    ```
    
2. Navigate to following URL: [`https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L632`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L632) and [`https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L689`](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L689) 
    
    ```
    IERC20(target).safeApprove(bathToken, amount);
    
    IERC20(toApprove).safeApprove(RubiconMarketAddress, type(uint256).max);
    ```
    

### Recommendation

Using this deprecated function can lead to unintended reverts and potentially the locking of funds. A deeper discussion on the deprecation of this function is in OZ issue #2219 (OpenZeppelin/openzeppelin-contracts#2219). The OpenZeppelin ERC20 `safeApprove()` function has been deprecated, as seen in the comments of the OpenZeppelin code. As recommended by the OpenZeppelin comment, consider replacing `safeApprove()` with `safeIncreaseAllowance()` or `safeDecreaseAllowance()` instead:

[https://github.com/OpenZeppelin/openzeppelin-contracts/issues/2219](https://github.com/OpenZeppelin/openzeppelin-contracts/issues/2219)

---

# [L-04] Unused/empty receive()/fallback() function used by the contract

### Description

The **`receive()`** and **`fallback()`** functions are special functions that are automatically invoked when a contract receives a transaction with no function signature or invalid function signature, respectively.

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. `require(msg.sender == address(weth))`). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds.

### Proof of Concept

*There is 1 instance of this issue:*

[https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L63](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/RubiconRouter.sol#L63)

```solidity
		receive() external payable {}

    fallback() external payable {}
```

### Mitigation

It is important to note that an empty **`receive()`** or **`fallback()`** function can pose a security risk if it is not properly handled. An attacker could potentially send a transaction to the contract and invoke the empty **`receive()`** or **`fallback()`** function, which could lead to unexpected behavior. Therefore, it is generally a good practice to implement proper error handling in these functions to ensure that unexpected transactions are handled correctly.

Overall, it is recommended to remove any unused or unnecessary code, including an empty **`receive()`** or **`fallback()`** function, as a good programming practice.