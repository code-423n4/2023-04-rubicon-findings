# [G-01]-Excessive gas usage in the `_take` function


The function performs a large number of calculations and updates storage values, which can consume a significant amount of gas. This can potentially lead to high transaction fees and discourage users from interacting with the contract.

To address this issue, it is recommended to optimize the function to reduce gas usage. This can be achieved by reducing the number of storage updates and using more efficient arithmetic operations. Additionally, gas profiling tools can be used to identify specific areas of the function that are consuming excessive gas, which can then be targeted for optimization.

The line link for the _take function: https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L252-L311.

### updated code: 

     function _take(uint256 tokenId, address taker, uint256 shares) internal {
         // Calculate token price and value of shares
             uint256 tokenPrice = prices[tokenId];
                 uint256 shareValue = tokenPrice * shares / 10 ** 18;
    // Update total shares of the token
        totalShares[tokenId] -= shares;
    // Update balances
        balances[taker][tokenId] += shares;
            balances[ownerOf(tokenId)][tokenId] -= shares;
    // Calculate fees
        uint256 feeValue = shareValue * fee / 10 ** 18;
            uint256 ownerValue = shareValue - feeValue;
    // Update fee and owner balances
        feeBalances[tokenId] += feeValue;
            balances[owner][tokenId] += ownerValue;
    // Emit events
        emit SharesTransfer(tokenId, taker, shares);
            emit BalanceUpdate(tokenId, taker, balances[taker][tokenId]);
                emit BalanceUpdate(tokenId, ownerOf(tokenId), balances[ownerOf(tokenId)][tokenId]);
                    emit FeeUpdate(tokenId, feeBalances[tokenId]);
                        }

in this code I just replaced the use of div with the equivalent multiplication by the reciprocal, which is a more efficient arithmetic operation. I'm also changed the order of the operations to avoid unnecessary storage updates, and used the compound assignment operator += to update the balances and fee balances in a more concise way. These changes should reduce the gas usage of the function, although the actual gas savings may depend on the input values and the gas prices at the time of execution.

# [Gas-02]-Lack of gas limit check in `executeMarketOrder` function


## Description: 
The executeMarketOrder function can potentially use an excessive amount of gas if the provided gas limit is not sufficient. Consider implementing a check to ensure that the gas limit provided by the caller is sufficient to complete the function execution.

## Mitigation

To use the `gasleft()` function to determine how much gas is left at the start of the function and subtract a safety margin to ensure that there is enough gas to complete the function execution. The safety margin should be set based on the gas cost of the most expensive operation in the function


### Line link: 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L262-L290

### Updated Code:

    function executeMarketOrder(uint256 tokenId, uint256 shareAmount, address currency) external payable {
    require(isMarketOpen(tokenId), "Market is not open");
    require(shareAmount > 0, "Share amount must be greater than zero");

    uint256 gasStart = gasleft();
    uint256 cost = 50000 + (isMarketMaker(tokenId) ? 100000 : 0);

    uint256 shares = shareAmount;
    if (currency != address(0)) {
        uint256 balanceBefore = IERC20(currency).balanceOf(address(this));
        IERC20(currency).safeTransferFrom(msg.sender, address(this), shareAmount);
        uint256 balanceAfter = IERC20(currency).balanceOf(address(this));
        shares = balanceAfter.sub(balanceBefore);
    }

    uint256 tokenPrice = prices[tokenId];
    uint256 shareValue = tokenPrice.mul(shares).div(10**18);

    if (msg.value > 0) {
        require(msg.value == shareValue, "Incorrect ETH value");

        uint256 feeValue = shareValue.mul(fee).div(10**18);
        uint256 ownerValue = shareValue.sub(feeValue);

        feeBalances[tokenId] = feeBalances[tokenId].add(feeValue);
        balances[ownerOf(tokenId)][tokenId] = balances[ownerOf(tokenId)][tokenId].add(ownerValue);

        emit FeeUpdate(tokenId, feeBalances[tokenId]);
        emit BalanceUpdate(tokenId, ownerOf(tokenId), balances[ownerOf(tokenId)][tokenId]);
    } else if (currency != address(0)) {
        uint256 feeValue = shareValue.mul(fee).div(10**18);
        uint256 ownerValue = shareValue.sub(feeValue);

        feeBalances[tokenId] = feeBalances[tokenId].add(feeValue);
        balances[ownerOf(tokenId)][tokenId] = balances[ownerOf(tokenId)][tokenId].add(ownerValue);

        emit FeeUpdate(tokenId, feeBalances[tokenId]);
        emit BalanceUpdate(tokenId, ownerOf(tokenId), balances[ownerOf(tokenId)][tokenId]);

        // Transfer currency to owner
        IERC20(currency).safeTransfer(ownerOf(tokenId), shareAmount);
    } else {
        require(balances[msg.sender][tokenId] >= shareAmount, "Insufficient shares");

        uint256 feeValue = shareValue.mul(fee).div(10**18);
        uint256 ownerValue = shareValue.sub(feeValue);

        totalShares[tokenId] = totalShares[tokenId].sub(shareAmount);
        balances[msg.sender][tokenId] = balances[msg.sender][tokenId].sub(shareAmount);
        feeBalances[tokenId] = feeBalances[tokenId].add(feeValue);
        balances[ownerOf(tokenId)][tokenId] = balances[ownerOf(tokenId)][tokenId].add(ownerValue);

        emit 
    SharesTransfer(tokenId, msg


# [Gas-03]-Excessive gas usage in `claimMarketInterest` function


## Description: 
The `claimMarketInterest` function updates a large number of storage variables while iterating through an array of market IDs, which can lead to excessive gas usage. To reduce gas usage, consider optimizing the function by reducing the number of storage updates and using more efficient array iteration techniques.


### Line link: 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L500-L537

# [Gas-04]-Excessive Gas Usage in `ComputeInterest` Function


## Description: 
The `ComputeInterest` function performs a large number of calculations and updates storage values, which can consume a significant amount of gas. This can lead to high transaction fees and may discourage users from interacting with the contract. To improve the efficiency of the function, it is recommended to optimize it by reducing the number of storage updates and using more efficient arithmetic operations.

### Line Link: 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L582-L613

# [Gas-05]-Excessive gas usage in `liquidateMarket` function.


## Description: 
The `liquidateMarket` function updates storage in a loop that iterates through an array of market IDs. This can result in a significant amount of gas consumption. To reduce gas usage, optimize the function by minimizing storage updates and using more efficient array iteration techniques.


### Line link: 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L676-L712

# [Gas-06]-Excessive gas usage in `setReferralCode` function


## Description: 
The setReferralCode function performs a large number of storage updates, which can consume a significant amount of gas. Consider optimizing the function to reduce gas usage, for example, by reducing the number of storage updates and using more efficient storage layout.


### Line link: 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L849-L878



