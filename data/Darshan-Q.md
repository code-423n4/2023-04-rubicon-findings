## Constant values such as a call to keccak256() , should used to immutable rather than constant


immutable values are evaluated at deployment time and are stored in the contract's deployment bytecode. This means that they can be computed at runtime, but only once during contract deployment. This can make them more secure than constant values because they are evaluated in a more predictable and controlled environment.

In the specific case of calling keccak256() at runtime to compute a value, using immutable instead of constant can help ensure that the computed value is unique and not vulnerable to preimage attacks. If the value was declared as constant, an attacker could potentially compute the same value outside of the contract and use it to manipulate the contract's behavior.

    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232





## Unsafe use of transfer()/transferFrom() with IERC20 


The issue with the provided code is that it uses the transfer() function from the IERC20 interface to transfer tokens, which may not be safe for all tokens that implement the ERC20 standard. Some tokens, like Tether (USDT), do not follow the standard by not returning a boolean value as required by the specification. This can lead to unexpected behavior and potential vulnerabilities in the smart contract. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L59 

    IERC20(bathTokenV2).transfer(

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L102 

    IERC20(_feeToken).transfer(_feeTo, _feeAmount); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L380 

     require(
            _offer.pay_gem.transfer(msg.sender, quantity),
            "_offer.pay_gem.transfer(msg.sender, quantity) failed"
        ); 



    /// @dev V1 orders after V2 upgrade will point to address(0) in owner
        _offer.owner == address(0) && msg.sender == _offer.recipient
            ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
            : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));

## Typos 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L70 

    "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY" // ALREADY  


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L7
  
    /// @notice allows 3rd party protocols to charge their own fees, from interatcions with the Rubicon Protocol
    //interactions 


https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L75

    /// @notice rotuer call with ETH sent //router 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L886

    /// @notice Batch offer functionality - multuple offers in a single transaction // Multiple 

