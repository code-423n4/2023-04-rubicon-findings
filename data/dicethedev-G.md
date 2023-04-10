
1. Avoid using `string.concat` in the `_bathify` function, which can be expensive in terms of gas cost. Instead, you can simply concatenate the two strings using the `+` operator, like this:

``
  _name = string(abi.encodePacked("bath", ERC20(_underlying).symbol()));
  _symbol = string(abi.encodePacked(_name, "v2"));

``

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L147-L148

2. Combine multiple external function calls into a single transaction to reduce gas costs. For example, in the `claimRewards()` function, instead of calling `comptroller.claimComp()` and then calling the `getReward()` function for each BathBuddy, you can call the `getRewards()` function for each BathBuddy in a single transaction and then call `comptroller.claimComp()` at the end.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L115-L128

3. Use `safeTransfer` instead of `transfer`
In the line `IERC20(bathTokenV2).transfer(msg.sender, IERC20(bathTokenV2).balanceOf(address(this)));`, it is safer to use `IERC20(bathTokenV2).safeTransfer(msg.sender, IERC20(bathTokenV2).balanceOf(address(this)));` instead. This will ensure that if the transfer fails, the transaction will revert and the user's funds will not be lost.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L59


4. Use memory instead of storage for function parameters
In the migrate function, the `IBathToken bathTokenV1` parameter can be declared as `IBathToken memory bathTokenV1` instead. This will tell the compiler that the parameter does not need to be stored in contract storage, which can save gas.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L38
 
5. Reorder `repayBorrowBehalf` call: In the `repayBehalfExplicit` function, you can call `cEther_.repayBorrowBehalf{value: received}(borrower);` first and then check for excess funds and transfer them. This saves gas by avoiding the additional gas cost of the `if` statement. 

  The Reorder of `repayBorrowBehalf` call should be like this for example:

``
    function repayBehalfExplicit(address borrower, address cEther_) public payable {
        uint256 received = msg.value;
        uint256 borrows = CEther(cEther_).borrowBalanceCurrent(borrower);

        CEther(cEther_).repayBorrowBehalf{value: received}(borrower);

        if (received > borrows) {
            address(uint160(msg.sender)).transfer(received - borrows);
        }
    }
``

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/compound-v2-fork/Maximillion.sol#L38-L48


6. Use `transfer` instead of `payable` for gas savings: In the `repayBehalfExplicit` function, instead of using `payable(msg.sender).transfer(received - borrows);`, you can use `address(uint160(msg.sender)).transfer(received - borrows);`. This saves gas by using a lower amount of gas for transferring the funds.

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/compound-v2-fork/Maximillion.sol#L43

