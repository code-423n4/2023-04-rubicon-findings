1.Inconsistent solidity versions:

Contracts BathHouseV2.sol, V2Migrator.sol, Position.sol, FeeWrapper.sol works on Solidity 0.8.17 whereas RubiconMarket.sol works on 0.8.9 and BathBuddy.sol works on 0.8.0

It is generally best practice to have all newly deployed contracts run on a fairly new Solidity version as these contracts aren't deployed yet.

Newer Solidity versions generally hold bug and optimisation improvements from the previous version.

It is said that running the latest version of Solidity is not recommended as it can hold unknown bugs but running 0.8.17 as in some contracts is sufficient enough as 0.8.18 & 0.8.19 haven't brought anyway major changes, however letting RubiconMarket.sol and BathBuddy.sol run on at least 0.8.17 would be preferable as there are major improvements since both versions.

It would also be recommend that developers consider adding a "^" so that it reads ^0.8.17 and the contracts can be "future prove" for any possible future integrations.



2.Uncorrelated Liceneses in:

Contract RubiconMarket.sol uses the license AGPL-3.0 whereas all other contracts under scope uses MIT

Consider using the same license for all contracts under scope as this is also best practice.



3.Uncorrelated comment marks in:

In line 1 of contract RubiconMarket.sol it's the license line. It's best practice to have only two comment initialisations as "//" here and not three as it currently is.

This will also let it correlate with other contracts under scope.



4.Developer notes still remaining in comments in:

4.1

Contract: RubiconMarket.sol

      line 4, 5 (testing environment leftovers)
     
     line 522, 524 (details from refactorred code)
     
     line 837, 1381, 1394 (reference deprecated code in line 838, 1382, 1395)

4.2

Contract: BathBuddy.sol

    line 35, 36 (TODO states these 2 comments should be adjusted as what's stated is not the case anymore)
    
    line 65, 66, 67, 146, 231 (production questions)
    
    line 16, 81, 110, 138, 165, 189, 215 (general developer discussions irrelevant for post production)

4.3

Contract: Position.sol

    line 64 (general developer discussions irrelevant for post production - more detail in step -add- )



5.Remove deprecated code in:

5.1

Contract: RubiconMarket.sol

    line 16

    line 100 - 116

    line 129 - 150

    line 163 - 172

    line 185 - 195

    line 207

    line 300 - 309

    line 386 - 396

    line 409 - 417

    line 429 - 434

    line 438

    line 464 - 472

    line 542 - 551

    line 661, 663, 666

    line 838, 850, 851

    line 1346 - 1359

    line 1382, 1395

5.2

Contract: Position.sol

    line 263



6.Remove unused variables according to comments in:

6.1

Contract: RubiconMarket.sol

    line 681 - 684

In the comments of line 682 & 683 the developer comments state that the two variables in line 681 & 684 have been deprecated but they haven't been removed from production yet.

Recommendation: 

Remove unused variables still in post production code in accordance with respective comments mentioned above.



7.Code in Error messages returned to user in:

7.1

Contract: RubiconMarket.sol

    line 376, 381

Recommendation:

Consider removing code in error messages.

In line 376 there is a short description stating " failed - check that you can pay the fee".

This should be sufficient with code removed.

Something similar can be done for line 381, but adjusted for "quantity".



8.Double comment initialization in:

It is best practice to only have one "//" comment initialization and although the throughput on Github is a normal comment that is a little bit odd, in an IDE like VS Code the comment gets uncommented with a "-----" through the comment.

In correlation with step 5 regarding the removal of developer notes, comments in line 1343 - 1345 in contract RubiconMarket.sol should be removed, however the remaining should follow best practices as they are crucial for the explanation of functions / modifiers, unless you there were intentions to also deprecate these modifiers but the code is still active. If so it should also be removed.

8.1

Contract: RubiconMarket.sol

    line 250

- // /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it
+ /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it

    line 718

- // // After close, anyone can cancel an offer
+ // After close, anyone can cancel an offer




9.Consider adding more comments to correlate with other lines of code:

9.1

Contract: RubiconMarket.sol

    line 808, 809. Comments will correlate with line 831, 832, respectively



10.Add more clarity to function segments in:

There are comment sections to clarify which functions follow. This is great, however some do not correlate in style across all contracts.

To make the case I'm taking excerpt from contracts throughout.

Contract: RubiconMarket.sol

    line 1181

// ---- Internal Functions ---- //

Contract: BathHouseV2.sol

    line 41

//============================= VIEW =============================

Contract: BathBuddy.sol

    line 42

/* ========== STATE VARIABLES ========== */

Contracts BathHouseV2.sol, Position.sol, & FeeWrapper.sol correlate however the other 2 contracts under scope does not.

Recommendation:

Which style to choose is up to preference but it is best practice to have the same style across all contracts.



11.Function is in wrong category in:

Function borrowBalance is not suppose to be under the view category as also stated in the comment of line 64

11.1

Contract: Position.sol

    line 65

Recommendation:

line 65 - 69 which holds the function and it's comment line 63 should be moved to somewhere around line 91 in the MAIN category and the comment of line 63 should be deprecated in accordance of step 4.3 above.



12.Events placing is not following best practice in:

It is general best practice to have events before the constructor, functions and modifiers if events don't have their own inheritance contract as in RubiconMarket.sol

This is the case in contracts under scope except for contract BathBuddy.sol

Recommendation: 

Consider creating a new Event contract around line 34 which contract BathBuddy will inherit from or relocate events around line 62. 

Events for reallocation are in line 259 - line 266.



13.Insufficient variable naming in:

Generally it's best practice to keep variable naming consistent with the exact length of the variables especially given most variables do follow these rules in contracts under scope and some don't.

13.1

Contract: RubiconMarket.sol

The variable "uint" should be "uint256"

    line 785

- uint pos,
+ uint256 pos,

    line 888

- uint[] calldata payAmts,
+ uint256[] calldata payAmts,

    line 890

- uint[] calldata buyAmts,
+ uint256[] calldata buyAmts,

    line 899

- for (uint i = 0; i < payAmts.length; i++) {
+ for (uint256 i = 0; i < payAmts.length; i++) {

    line 910

- function batchCancel(uint[] calldata ids) external {
+ function batchCancel(uint256[] calldata ids) external {

    line 911

- for (uint i = 0; i < ids.length; i++) {
+ for (uint256 i = 0; i < ids.length; i++) {

    line 918

- uint[] calldata ids,
+ uint256[] calldata ids,

    line 919

- uint[] calldata payAmts,
+ uint256[] calldata payAmts,

    line 921

- uint[] calldata buyAmts,
+ uint256[] calldata buyAmts,

    line 924

- for (uint i = 0; i < ids.length; i++) {
+ for (uint256 i = 0; i < ids.length; i++) {

13.2

Contract: RubiconMarket.sol

The variable "bytes" should be "bytes32"

    line 640

- bytes fax
+ bytes32 fax

13.3

Contract: BathHouseV2.sol

The variable "bytes" should be "bytes32"

    line 65

- bytes memory becomeImplementationData
+ bytes32 memory becomeImplementationData

13.4

Contract: FeeWrapper.sol

The variable "bytes" should be "bytes32"

    line 14

- bytes args;
+ bytes32 args;

    line 50

- ) external payable returns (bytes memory) {
+ ) external payable returns (bytes32 memory) {

    line 62

- ) internal returns (bytes memory) {
+ ) internal returns (bytes32 memory) {

    line 66

- (bool _OK, bytes memory _data) = _params.target.call(
+ (bool _OK, bytes32 memory _data) = _params.target.call(

    line 78

- ) internal returns (bytes memory) {
+ ) internal returns (bytes32 memory) {

    line 82

- (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
+ (bool _OK, bytes32 memory _data) = _params.target.call{value: _msgValue}(



14.Inconsistent spacing in comments in:

It is also best practice to leave at least one space after the initial comment "//" mark.

Some comments have no space after comment initialisation while others have 2 or more whereas it should be 1 spcae aftwe each comment initialisation.

14.1

Contract: RubiconMarket.sol

    line 324

- ///@dev For backwards semantic compatibility.
+ /// @dev For backwards semantic compatibility.

    line 675

- //buy enabled 
+ // buy enabled 

Note: In line 675 there is also a TODO, in accordance with the automated tool findings the part from TODO on should ofcourse be removed.

    line 676

- //true: enable matching,
+ // true: enable matching,

    line 687

- //points to id of next higher offer
+ // points to id of next higher offer

    line 688

- //points to id of previous lower offer
+ // points to id of previous lower offer

    line 689

- //the blocknumber where this entry was marked for delete
+ // the blocknumber where this entry was marked for delete

    line 691

- //doubly linked lists of sorted offer ids
+ // doubly linked lists of sorted offer ids

    line 692

- //id of the highest offer for a token pair
+ // id of the highest offer for a token pair

    line 693

- //number of offers stored for token pair in sorted orderbook
+ // number of offers stored for token pair in sorted orderbook

    line 694

- //minimum sell amount for a token to avoid dust offers
+ // minimum sell amount for a token to avoid dust offers

    line 695

- //next unsorted offer id
+ // next unsorted offer id

    line 696

- //first unsorted offer id
+ // first unsorted offer id

    line 762

- //maker (ask) sell how much
+ // maker (ask) sell how much

    line 763

- //maker (ask) sell which token
+ // maker (ask) sell which token

    line 764
        
- //maker (ask) buy how much
+ // maker (ask) buy how much

    line 765
        
- //maker (ask) buy which token
+ // maker (ask) buy which token

    line 781

- //maker (ask) sell how much
+ // maker (ask) sell how much

    line 782

- //maker (ask) sell which token
+ // maker (ask) sell which token

    line 783
        
- //maker (ask) buy how much
+ // maker (ask) buy how much

    line 784
        
- //maker (ask) buy which token
+ // maker (ask) buy which token

    line 785
        
- //position to insert offer, 0 should be used if unknown
+ // position to insert offer, 0 should be used if unknown

    line 803

- //maker (ask) sell how much
+ // maker (ask) sell how much

    line 804

- //maker (ask) sell which token
+ // maker (ask) sell which token

    line 805
        
- //maker (ask) buy how much
+ // maker (ask) buy how much

    line 806
        
- //maker (ask) buy which token
+ // maker (ask) buy which token

    line 807 
        
- //position to insert offer, 0 should be used if unknown
+ // position to insert offer, 0 should be used if unknown

    line 825

- //maker (ask) sell how much
+ // maker (ask) sell how much

    line 826

- //maker (ask) sell which token
+ // maker (ask) sell which token

    line 827
        
- //maker (ask) buy how much
+ // maker (ask) buy how much

    line 828
        
- //maker (ask) buy which token
+ // maker (ask) buy which token

    line 829
        
- //position to insert offer, 0 should be used if unknown
+ // position to insert offer, 0 should be used if unknown

    line 830
        
- //match "close enough" orders?
+ // match "close enough" orders?

    line 854

- //Transfers funds from caller to offer maker, and from market to caller.
+ // Transfers funds from caller to offer maker, and from market to caller.

    line 882

- //delete the offer.
+ // delete the offer.

    line 935

- //deletes _rank [id]
+ // deletes _rank [id]

    line 950

- //set the minimum sell amount for a token
+ // set the minimum sell amount for a token

    line 956

- //token to assign minimum sell amount to
+ // token to assign minimum sell amount to

    line 957

- //maker (ask) minimum sell amount
+ // maker (ask) minimum sell amount

    line 964

- //returns the minimum sell amount for an offer
+ // returns the minimum sell amount for an offer

    line 966

- //token for which minimum sell amount is queried
+ // token for which minimum sell amount is queried

    line 971

- //return the best offer for a token pair
+ // return the best offer for a token pair

    line 981

- //return the next worse offer in the sorted list
+ // return the next worse offer in the sorted list

    line 989

- //return the next better offer in the sorted list
+ // return the next better offer in the sorted list

    line 997

- //return the amount of better offers for a token pair
+ // return the amount of better offers for a token pair

    line 1005

- //get the first unsorted offer that was inserted by a contract
+ // get the first unsorted offer that was inserted by a contract

    line 1014

- //get the next unsorted offer
+ // get the next unsorted offer

    line 1038

- //while there is amount to sell
+ // while there is amount to sell

    line 1039

- //Get the best offer for the token pair
+ // Get the best offer for the token pair

    line 1040

- //Fails if there are not more offers
+ // Fails if there are not more offers

    line 1047

- //We consider that all amount is sold
+ // We consider that all amount is sold

    line 1050

- //If amount to sell is higher or equal than current offer amount to buy
+ // If amount to sell is higher or equal than current offer amount to buy

    line 1051

- //Add amount bought to acumulator
+ // Add amount bought to acumulator

    line 1052
                
- //Decrease amount to sell
+ // Decrease amount to sell

    line 1053
                
- //We take the whole offer
+ // We take the whole offer

    line 1060

- //Add amount bought to acumulator
+ // Add amount bought to acumulator

    line 1061

- //We take the portion of the offer that we need
+ // We take the portion of the offer that we need

    line 1062
                
- //All amount is sold
+ // All amount is sold

    line 1078

- //Meanwhile there is amount to buy
+ // Meanwhile there is amount to buy

    line 1079

- //Get the best offer for the token pair
+ // Get the best offer for the token pair

    line 1087

- //We consider that all amount is sold
+ // We consider that all amount is sold

    line 1090

- //If amount to buy is higher or equal than current offer amount to sell
+ // If amount to buy is higher or equal than current offer amount to sell

    line 1091

- //Add amount sold to acumulator
+ // Add amount sold to acumulator

    line 1092
                
- //Decrease amount to buy
+ // Decrease amount to buy

    line 1093
                
- //We take the whole offer
+ // We take the whole offer

    line 1095

- //if lower
+ // if lower

    line 1102

- //Add amount sold to acumulator
+ // Add amount sold to acumulator

    line 1103

- //We take the portion of the offer that we need
+ // We take the portion of the offer that we need

    line 1104

- //All amount is bought
+ // All amount is bought

    line 1129

- //Get best offer for the token pair
+ // Get best offer for the token pair

    line 1131

- //Add amount to buy accumulator
+ // Add amount to buy accumulator

    line 1132

- //Decrease amount to pay
+ // Decrease amount to pay

    line 1134

- //If we still need more offers
+ // If we still need more offers

    line 1135

- //We look for the next best offer
+ // We look for the next best offer

    line 1136

- //Fails if there are not enough offers to complete
+ // Fails if there are not enough offers to complete

    line 1145

- //Add proportional amount of last offer to buy accumulator
+ // Add proportional amount of last offer to buy accumulator

    line 1162

- //Get best offer for the token pair
+ // Get best offer for the token pair

    line 1164

- //Add amount to pay accumulator
+ // Add amount to pay accumulator

    line 1165

- //Decrease amount to buy
+ // Decrease amount to buy

    line 1167

- //If we still need more offers
+ // If we still need more offers
        
    line 1168
  
- //We look for the next best offer
+ // We look for the next best offer

    line 1169

- //Fails if there are not enough offers to complete
+ // Fails if there are not enough offers to complete

    line 1178

- //Add proportional amount of last offer to pay accumulator
+ // Add proportional amount of last offer to pay accumulator

    line 1187

- //offers[id] must be removed from sorted list because all of it is bought
+ // offers[id] must be removed from sorted list because all of it is bought

    line 1201

- //enable current msg.sender to call cancel(id)
+ // enable current msg.sender to call cancel(id)

    line 1224

- //find the id of the next higher offer after offers[id]
+ // find the id of the next higher offer after offers[id]

    line 1234

- //if we got to the end of list without a single active offer
+ // if we got to the end of list without a single active offer

    line 1260

- //return true if offers[low] priced less than or equal to offers[high]
+ // return true if offers[low] priced less than or equal to offers[high]

    line 1262

- //lower priced offer's id
+ // lower priced offer's id

    line 1263

- //higher priced offer's id
+ // higher priced offer's id

    line 1270

- //these variables are global only because of solidity local variable limit
+ // these variables are global only because of solidity local variable limit

    line 1272

- //match offers with taker offer, and execute token transactions
+ // match offers with taker offer, and execute token transactions

    line 1274

- //taker sell how much
+ // taker sell how much

    line 1275

- //taker sell which token
+ // taker sell which token

    line 1276

- //taker buy how much
+ // taker buy how much

    line 1277

- //taker buy which token
+ // taker buy which token

    line 1278

- //position id
+ // position id

    line 1279

- //match "close enough" orders?
+ // match "close enough" orders?

    line 1283

- //highest maker id
+ // highest maker id

    line 1284

+ //taker buy how much saved
- // taker buy how much saved

    line 1285

- //maker offer wants to buy this much token
+ // maker offer wants to buy this much token

    line 1286

- //maker offer wants to sell this much token
+ // maker offer wants to sell this much token

    line 1329

- //new offer should be created
+ // new offer should be created

    line 1338

- //insert offer into the sorted list
+ // insert offer into the sorted list

    line 1361

- //put offer into the sorted list
+ // put offer into the sorted list

    line 1363

-  //maker (ask) id
+ // maker (ask) id

    line 1364

- //position to insert into
+ // position to insert into

    line 1370

- //maker (ask) id
+ // maker (ask) id

    line 1380

- //offers[id] is not the highest offer
+ // offers[id] is not the highest offer

    line 1387

- //offers[id] is the highest offer
+ // offers[id] is the highest offer

    line 1393

- //if lower offer does exist
+ // if lower offer does exist

    line 1406

- //id of maker (ask) offer to remove from sorted list
+ // id of maker (ask) offer to remove from sorted list

    line 1413

 - //assert id is in the sorted list
+ // assert id is in the sorted list

    line 1422

- //offers[id] is the highest offer
+ // offers[id] is the highest offer

    line 1427

- //offers[id] is not the lowest offer
+ // offers[id] is not the lowest offer

    line 1433

- //mark _rank[id] for deletion
+ // mark _rank[id] for deletion

    line 1437

- //Hide offer from the unsorted order book (does not cancel offer)
+ // Hide offer from the unsorted order book (does not cancel offer)

    line 1439

- //id of maker offer to remove from unsorted list
+ // id of maker offer to remove from unsorted list

    line 1441

- //id of an offer in unsorted offers list
+ // id of an offer in unsorted offers list

    line 1442

- //id of previous offer in unsorted offers list
+ // id of previous offer in unsorted offers list

    line 1444

- //make sure offer id is not in sorted offers list
+ // make sure offer id is not in sorted offers list

    line 1447

- //check if offer is first offer in unsorted offers list
+ // check if offer is first offer in unsorted offers list

    line 1448

- //set head to new first unsorted offer
+ // set head to new first unsorted offer

    line 1449

- //delete order from unsorted order list
+ // delete order from unsorted order list

    line 1453

- //find offer in unsorted order list
+ // find offer in unsorted order list

    line 1458

- //did not find offer id in unsorted offers list
+ // did not find offer id in unsorted offers list

    line 1461

- //set previous unsorted offer to point to offer after offer id
+ // set previous unsorted offer to point to offer after offer id

    line 1462

- //delete order from unsorted order list
+ // delete order from unsorted order list

14.2

Contract: BathBuddy.sol

    line 12

- ///            for a given token during the current rewards period ongoing on bathBuddy
+ /// for a given token during the current rewards period ongoing on bathBuddy


Added Note for Step 14:


Exceptions to the above can be made to:

Contract Rubicon 

    line 972, 973

    line 982 - 984

    line 990 - 992

    line 1006 - 1009

    line 1015

Which holds 6 spaces throughout but if this logic is followed, the following lines should also have 6 spaces instead of inconsistent spaces which hold 2 spaces on line 936 & 4 spaces each in line 951 - 954

    line 936

- //  Function should be called by keepers.
+ //      Function should be called by keepers.

    line 951 - 954

- //    Function is used to avoid "dust offers" that have
- //    very small amount of tokens to sell, and it would
- //    cost more gas to accept the offer, than the value
- //    of tokens received.

+ //      Function is used to avoid "dust offers" that have
+ //      very small amount of tokens to sell, and it would
+ //      cost more gas to accept the offer, than the value
+ //      of tokens received.

At first glance it can be said that contract BathBuddy.sol line 12 should also fall in this category. However I believe that the latter is not the case and it can come down to two accidentally tabs instead of a potential pattern being created as we're the defined function definitions in contract Rubicon with above lines mentioned.



15.Grammar issues in:

15.1

Contract: RubiconMarket.sol

    line 676

The comma at the end of the comment should be removed

- //true: enable matching,
+ //true: enable matching

Note: If the best practice for spacing in comments is followed in step 15.1 regarding this specific line the respective comment will change to:

// true: enable matching



16.Typos in:

16.1

Contract: RubiconMarket.sol

    line 886

"multiple" is misspelled as "multuple"

- /// @notice Batch offer functionality - multuple offers in a single transaction
+ /// @notice Batch offer functionality - multiple offers in a single transaction

    line 1413

"asset" is misspelled as "assert"

- //assert id is in the sorted list
+ //asset id is in the sorted list

Note: If the best practice for spacing in comments is followed in step 15.1 regarding this specific line the respective comment will change to:

// asset id is in the sorted list

16.2

Contract: FeeWrapper.sol

    line 7

"interactions" is misspelled as "interatcions"

- /// @notice allows 3rd party protocols to charge their own fees, from interatcions with the Rubicon Protocol
+ /// @notice allows 3rd party protocols to charge their own fees, from interactions with the Rubicon Protocol

    line 59

"default" is misspelled as "defaull"

- /// @notice defaull call to the router functions
+ /// @notice default call to the router functions
   
    line 75

"router" is misspelled as "rotuer"

- /// @notice rotuer call with ETH sent
+ /// @notice router call with ETH sent