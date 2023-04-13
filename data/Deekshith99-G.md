## [G-1] Structs can be closely packed
There are 2 instances of it
1st instance is at [Position.sol#L33-L40](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L33-L40)
here `bool isActive;` (1 byte) can be packed with `address quote;` (20 bytes) to save 1 storage slot 
which saves around 2100 of gas

    struct Position {
        address asset; // supplied as collateral
        address quote; // borrowed token
        uint256 borrowedAmount; // amount of borrowed quote
        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
        uint256 blockNum; // block number on which position was opened
        bool isActive; // false by default, when active - true
    }

`to`

    struct Position {
        address asset; // supplied as collateral
        address quote; // borrowed token                        // 20 bytes
        bool isActive; // false by default, when active - true // 1 byte
        uint256 borrowedAmount; // amount of borrowed quote
        uint256 bathTokenAmount; // amount of bathTokens to which collateral was supplied
        uint256 blockNum; // block number on which position was opened
    }
2nd instance is at [FeeWrapper.sol#L12-L17](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L12-L17)
here `address target;` (20 bytes) can be packed with `bytes4 selector;` (4bytes) to save
1 storage slot & can save around 2100 of gas

    struct CallParams {
        bytes4 selector; // function selector
        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
        address target; // target contract to call
        FeeParams feeParams;
    }

`to`

    struct CallParams {
        bytes4 selector; // function selector
        address target; // target contract to call
        bytes args; // encoded arguments (abi.encode() || abi.encodePacked)
        FeeParams feeParams;
    }
## [G-2] State variables only set in the constructor should be declared immutable
To Avoids a Gsset (20000 gas) in the constructor, and replaces the first access in each transaction (Gcoldsload - 2100 gas) and each access thereafter (Gwarmacces - 100 gas) with a PUSH32 (3 gas).

While strings are not value types, and therefore cannot be immutable/constant if not hard-coded outside of the constructor, the same behaviour can be achieved by making the current contract abstract with virtual functions for the string accessors, and having a child contract override the functions with the hard-coded implementation-specific values.

There are 4 instance of it

        oracle = PriceOracle(_oracle);
        rubiconMarket = RubiconMarket(_rubiconMarket);
        bathHouseV2 = BathHouseV2(_bathHouseV2);
        comptroller = bathHouseV2.comptroller();
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L55-L58

## [G-3] state variables can be closely packed
There is 1 instance of it [RubiconMarket.sol#L224-L230](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L224-L230)
here `bool locked;` (1 byte) can be packed with `address internal feeTo;` (20 bytes)
to save 1 storage slot which saves around 2100 of gas

    bool locked;

    /// @dev This parameter is in basis points
    uint256 internal feeBPS;

    /// @dev This parameter provides the address to which fees are sent
    address internal feeTo;

`to`



    /// @dev This parameter is in basis points
    uint256 internal feeBPS;

    /// @dev This parameter provides the address to which fees are sent
    address internal feeTo; // 20 bytes
    bool locked; // 1 byte
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L224-L230