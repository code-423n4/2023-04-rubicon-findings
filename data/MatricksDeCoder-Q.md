#### NC-1 Unlocked Pragma

-Description => Some Contracts in scope  make use of floating pragma e.g ^0.8.9 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L2

-Impact => This can lead to problems with different versions being used for testing, production etc as a floating pragma implies any within range may be used could lead to outdated compiler, versions with different bugs etc
-POC => SWC103 https://swcregistry.io/docs/SWC-103
-Tools Used => Manual
-Recommendation => It is recommended to lock the pragma. Make use of pragma solidity 0.8.17 remove caret

#### NC-2 Solidity Version

-Description => Some contracts in scope make use of an older solidity version e.g ^0.8.9
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L2

-Impact => Older versions may have bugs that are fixed in more recent versions which may impact compiler and or project
-POC => https://swcregistry.io/docs/SWC-102
-Tools Used => Manual
-Recommendation => It is recommended to use latest versions of solidity e.g 0.8.17 0.8.18 etc

#### NC-3 Use Scientific Notation

-Description => Some contracts in scope make use of exponentiation based on multiplication for large numbers e.g  10 ** 18; 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L75

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1057

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1059

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1099

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1101

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1142

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1144

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1175

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1177

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L317

-Impact => This can be prone to errors e.g use 10*18 when needed was 1*10**18. Arguably scientific notation is more readable compared to above
-POC => https://docs.soliditylang.org/fr/latest/types.html
-Tools Used => Manual
-Recommendation => It is recommended to use scientific notation e.g 1*10**18 => 1e18 in all relevant instances


#### NC-4 Natspec Code Commenting

-Description => Contracts in scope do not make use of rich consistent detailed Natspec commenting especially for functions with parameters e.g most functions just make use of @notice
They ignore this best practise as below e.g 
 /// @notice Calculate tree age in years, rounded up, for live trees
 /// @dev The Alexandr N. Tetearing algorithm could increase precision
 /// @param rings The number of rings from dendrochronological sample
 /// @return Age in years, rounded up for partial years

-Impact => This can reduce the ability or other devs, users, auditors to understand the functions
-POC => https://docs.soliditylang.org/en/v0.8.17/natspec-format.html
-Tools Used => Manual
-Recommendation => It is recommended to fix all relevant function instances to make use of rich Natspec code commenting which allows for solid documentation of functions, how they work, its paramaters and return values to help with code readability and maintainability


#### NC-5 Function Parameters Use of UnderScore

-Description => Some contracts in scope  make use _ underscore in front of function parameters or back e. g var_ or none . However certain instances as below are not consistent with what majority of code has 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L297
Most params don't use _ e.g uint256 id but above uses bytes32 id_ 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L700
Makes use of address _feeTo 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466
Makes use of uint256 _newFeeBPS when majority code uses e.g uint256 newFeeBPS

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471
Makes use of uint256 _newMakerFee 

Contracts BathHouseV2.sol 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L32
makes use of _ underscore params other functions do not

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L138
Makes use of _ underscore param -> address _underlying 

Contracts V2Migrator.sol function params don't make use of _ undescore

Contracts BathBuddy.sol majority function params dont make us of _ underscore but following lines do:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L72

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L74

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L233
On same function next param has no underscore which shows amount of inconsistency in all these contracts

Contracts Position.sol 
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L251 
From above line functions start making use of _ undescore in front of function params

Contracts FeeWrapper.sol some functions use _ others do not e.g following use _

Not only in params also check in functions when initiating temporary variables e.g uint256 _a = 34 vs 
uint a = 34 

-Impact => This can lead to inconsistencies in code
-POC => Inconsistency 
-Tools Used => Manual
-Recommendation => It is recommended to be consistent and make use of e.g _ undescore always or for internal functions or temp variables etc or not use undescore consistently


#### NC-6 Implicit use of uint

-Description => There are instances where uint is used without specifying which type e.g uint256 uint8 in the contracts e.g

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L785

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L888

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L890

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L918

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L919

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L921

-Impact => This can lead to readability, maintenance or even make code prone to error if incorrect casting of values occurs
-POC => Recommended Best Practice
-Tools Used => Manual
-Recommendation => It is recommended to always be specific with the uint types e.g uint256. Fix all instances mentioned and other similar that may not have been mentioned


#### NC-7 Unused named return values 

-Description => Some Contracts in scope  make use of named return values e.g returns( bool active) however in function body named return value is never used.  

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L276
(bool active) not used 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L280
(address owner) not used

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L284
(address owner) not used 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L496
e.g should be id =  bytes32(offer(pay_amt,pay_gem,buy_amt,buy_gem,msg.sender,msg.sender));

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L620
e.g closed = false

-Impact => This can lead to confusion on code readability, auditing, maintain, chance to return wrong values or forget to return right values and variables etc 
-POC => Best practise 
-Tools Used => Manual
-Recommendation => It is recommended to not use "return" keyword in functions with named return values but just assign the return values to their respective named variables. e.g active = true


#### NC-8 Only constant or immutable variables should be capitalized 

-Description => Some code instances capitalize variables like e.g named return variables 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L130 

-Impact => This can lead to problems with code readability, consistency
-POC => Best Practise 
-Tools Used => Manual
-Recommendation => It is recommended to use cameCase for all other variables 

#### NC-9 Spelling and grammar errors

-Description => There are instances of typos, misspelling, unclear grammar, mistakes etc in the code 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L295
entrypoints vs entry points 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L592
cancelled vs canceled 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L886
multuple vs multiple

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1051
acumulator vs accumulator --- also check other instances e.g line 1060,1091,1102 etc


https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L70
ALDREADY vs ALREADY 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L139
then vs the

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L7
interatcions vs interactions

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L59
defaull vs default

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L75
rotuer  vs router 


-Impact => This can lead to problems with code readability, auditing, maintaning, or even errors if dev misinterprets comments 
-POC => Best Practise
-Tools Used => Manual
-Recommendation => It is recommended to fix all typos, spelling and grammar errors 

#### Low 1 Missing events for critical updates 

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1476

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L107

-Impact => This can lead to lack of transparency, lack of monitoring, insufficient information o off chain tooling that may depend on emission of these critical events.
-POC => Best Practise
-Tools Used => Manual
-Recommendation => It is recommended to emit events for critical parameter updates. Fix for above instances or any other relevant that are useful to report changes to contract functionality


