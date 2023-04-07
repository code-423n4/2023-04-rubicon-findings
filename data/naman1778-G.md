## Use assembly to write address storage values

When writing value for variables whose type is address, make use of assembly code instead of solidity code.
There are two instances for this

    File: contracts/RubiconMarket.sol
    25: function setOwner(address owner_) external auth {
    - 26:   owner = owner_;
    +       assembly {                      
    +           sstore(owner.slot, owner_)
    +       }    
    27:     emit LogSetOwner(owner);
    28: }

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L26

    702:  function initialize(address _feeTo) public {
    703:    require(!initialized, "contract is already initialized");
    704:    require(_feeTo != address(0));

    706:    /// @notice The market fee recipient
    - 707:  feeTo = _feeTo;
    +       assembly {                      
    +           sstore(owner.feeTo, _feeTo)
    +       } 
    
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L705

