# RUBICON MARKET 
1)Instead of using the “&&” operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per “require” statement will save 3 GAS per “&&”. Following changes could save some gas:

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L937-L948

New: 
function del_rank (uint256 id) external returns (bool) {
require(!locked);
require( !isActive(id)); 
require(_rank[id].delb != 0);
require(_rank[id].delb < block.number - 10);

2)The following one is the same recommendation as above but for the function _unsort

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1405-L1415

New:
require(_rank[id].delb == 0); 
require(isOfferSorted(id));

3)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L720 
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L726
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1109


# BATHHOUSEV2 
1)Use immutable variables instead of storage variables - Immutable variables are stored in the bytecode instead of in storage, which can save gas. The “admin” and “proxyAdmin” variables in the BathHouseV2 contract could be declared as immutable since they are never modified after initialization. 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L14-L17

2)Use “bytes32” instead of “string” - Using “bytes32” instead of “string” for small pieces of data can save gas. The “_bathify” function in the BathHouseV2 contract could use “bytes32” instead of “string” for the “_name” and “_symbol” variables. Just make sure the data won’t pass 32 characters.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L137-L149

3)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L33

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L70-L78

# V2MIGRATOR  

1)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L65

# BATHBUDDY 
1)Same recommendation as the RubiconMarket, instead of multiples "&&" use multiples "require" to save 3 gas per "&&":

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L94-L102

New:
modifier onlyBuddy() {
        require(msg.sender == myBathTokenBuddy);
        require(msg.sender != address(0)); 
        require(friendshipStarted, "You are not my buddy!");
        _;
    }

2) Same recommendation as in BathHouseV2: Use immutable variables instead of state variables. Immutable variables are initialized at deployment time, and their values are hardcoded into the bytecode. Accessing them is generally cheaper than accessing state variables, which require a storage read operation. The following variables can be made immutable Lines 46-48:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L46-L48

address immutable public owner;
address immutable public myBathTokenBuddy;
address immutable public bathHouse;

3)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L105
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L122
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L143
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L238

# POSITION 

1)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L593
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L597
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L290
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L102
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L116
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L76

# FeeWRAPPER 

1)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L70
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L86



