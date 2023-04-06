*** RUBICON MARKET ***
1)Instead of using the “&&” operator in a single require statement to check multiple conditions, using multiple require statements with 1 condition per “require” statement will save 3 GAS per “&&”. Following changes could save some gas:

Old:
function del_rank (uint256 id) external returns (bool) {
require(!locked);
require(!isActive(id) && _rank[id].delb != 0 && _rank[id].delb < block.number - 10);

New: 
function del_rank (uint256 id) external returns (bool) {
require(!locked);
require( !isActive(id)); 
require(_rank[id].delb != 0);
require(_rank[id].delb < block.number - 10);

2)The following one is the same recommendation as above but for the function _unsort(...); line 1405 to 1415

Old:
require(_rank[id].delb == 0 && isOfferSorted(id));

New:
require(_rank[id].delb == 0); 
require(isOfferSorted(id));

3)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:
-------Lines: 720 ("Offer was deleted or taken, or never existed."); 
--------------726 ("Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."); 
--------------1109 ("fill_amt exceeds max_fill_amount");


*** BATHHOUSEV2 ***
1)Use immutable variables instead of storage variables - Immutable variables are stored in the bytecode instead of in storage, which can save gas. The “admin” and “proxyAdmin” variables in the BathHouseV2 contract could be declared as immutable since they are never modified after initialization. Lines 14-17

2)Use “bytes32” instead of “string” - Using “bytes32” instead of “string” for small pieces of data can save gas. The “_bathify” function in the BathHouseV2 contract could use “bytes32” instead of “string” for the “_name” and “_symbol” variables. Just make sure the data won’t pass 32 characters. Lines 137-149

3)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:
-------Lines: 33 ("BathHouseV2 already initialized!");
-------------- 70; 74; 78 also contain strings longer than 32 characters;

*** V2 MIGRATOR CODE ***
1)Same recommendation as the RubiconMarket, instead of multiples "&&" use multiples "require" to save 3 gas per "&&":

Old (lines 94-102):
modifier onlyBuddy() {
        require(msg.sender == myBathTokenBuddy && msg.sender != address(0) && friendshipStarted, "You are not my buddy!");
        _;
    }

New:
modifier onlyBuddy() {
        require(msg.sender == myBathTokenBuddy);
        require(msg.sender != address(0)); 
        require(friendshipStarted, "You are not my buddy!");
        _;
    }
2)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:
--------Line 65 ("migrate: BATH TOKENS V2 STUCK IN THE CONTRACT");

*** BATHBUDDY ***
1) Same recommendation as in BathHouseV2: Use immutable variables instead of state variables. Immutable variables are initialized at deployment time, and their values are hardcoded into the bytecode. Accessing them is generally cheaper than accessing state variables, which require a storage read operation. The following variables can be made immutable Lines 46-48:

address immutable public owner;
address immutable public myBathTokenBuddy;
address immutable public bathHouse;

2)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:
----------Lines 105 ("You are not my beloved bath house!");
----------------122 ("I have not started a bathToken friendship");
----------------143 ("I have not started a bathToken friendship");
----------------238 ("Previous rewards period must be complete before changing the duration for the new period");

*** POSITION ***

1)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:
----------Lines 597, 593 ("_leverageCheck{Short}: INVLAID LEVERAGE");
----------------290 ("_repay: balance of quote lt. debt");
----------------116, 102 ("sellAllAmountWithLeverage: FAILED TO OPEN POSITION");
----------------76 ("borrowBalanceOfPos: POS ISN'T ACTIVE");

*** FeeWRAPPER ***

1)Strings that are more than 32 characters will require more than 1 storage slot, costing more gas. Consider reducing the message length to less than 32 characters or use error codes:
----------Lines 70 ("low-level call to the Rubicon failed");
----------------86 ("low-level call to the router failed");



