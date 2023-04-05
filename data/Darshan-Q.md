## Constant values such as a call to keccak256() , should used to immutable rather than constant


immutable values are evaluated at deployment time and are stored in the contract's deployment bytecode. This means that they can be computed at runtime, but only once during contract deployment. This can make them more secure than constant values because they are evaluated in a more predictable and controlled environment.

In the specific case of calling keccak256() at runtime to compute a value, using immutable instead of constant can help ensure that the computed value is unique and not vulnerable to preimage attacks. If the value was declared as constant, an attacker could potentially compute the same value outside of the contract and use it to manipulate the contract's behavior.

    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE"); 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L232