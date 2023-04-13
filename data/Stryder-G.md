# [G-01] Functions can be made external that are not called from within the contract

Link: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L288

Functions that are not called from **inside the contract can be made external** instead of public to save some gas 

there are 19 more instances of this : 

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L574
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L624
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L998
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1010
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1016
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L45
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L733-L750

# [G-02] State variables not getting inherited can be made private to save gas

Link: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L675

Consider making the bool state variables internal as the contract RubiconMarket is not getting inherited further so there is no need for the variables to be public 

other instance: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L676
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L679
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L682

# [G-03] Using calldata instead of memory for read-only arguments in external functions saves gas

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#:~:text=function%20calculateFee(,)


If you choose to make suggested above public functions as external, to continue gas optimizaton we can use calldata function arguments instead of memory.Consider changing the parameter type from memory to calldata 

 there are 10 other instances: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L29
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L49
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L61
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L77
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L109
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L30
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L65
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L82
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L116
    https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L117




# [G-04] Saving the array length before a loop saves gas in the long run 
File : FeeWrapper.sol 

In the for loop , its better to save the length of the array in a variable and then compare it with the array 


# [G-05] Struct can be more tightly packed 

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L206:~:text=OfferInfo)%20public%20offers%3B-,bool%20locked%3B,-///%20%40dev%20This%20parameter

File: RubiconMarket.sol 

The amount of gas taken to deploy the contract depends on the slots of memory assigned to the values in the contract 
in the current ordering , 6 slots will be consumed with no extra space left for any other variable as the slot space containing bool
will only store the bool and the assign feeBPS the next slot, wasting the remaining space at the slot with bool 
'''
   uint256 public last_offer_id;

    /// @dev The mapping that makes up the core orderbook of the exchange
    mapping(uint256 => OfferInfo) public offers;

    bool locked;

    /// @dev This parameter is in basis points
    uint256 internal feeBPS;

    /// @dev This parameter provides the address to which fees are sent
    address internal feeTo;

    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

 Consider changing it to :

      uint256 public last_offer_id;

    /// @dev The mapping that makes up the core orderbook of the exchange
    mapping(uint256 => OfferInfo) public offers;

  

    /// @dev This parameter is in basis points
    uint256 internal feeBPS;

    /// @dev This parameter provides the address to which fees are sent
    address internal feeTo;

    bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

    bool locked;


 This will have the 6th slot with some remaining space in which some other variables can be stored. 

# [G-06] Unncessary if else can be removed to save gas 

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=function%20isAuthorized(address%20src)%20internal%20view%20returns%20(bool)%20%7B
File : RubiconMarket.sol 
Estimated gas saving : 53 gas 
In the isAuthorized , the use of if and else can be omitted and the bool value can be directly returned from the function 

so instead of the function 

     function isAuthorized(address src)  internal  view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }

you can use this 

    function isAuthorized2(address src) internal  view returns (bool) {
        return src == owner;
     }

When i tested on remix , this saved about 53 units of gas . 

# [G-07]

Link: https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L40

File: RubiconMarket.sol 

Estimated gas saved: 80 gas 

the isAuthorized function is an internal function and is only called once and that too in a modifier. 
Consider adding the check directly in the modifier itself to prevent unnecessarily gas cost 

consider doing something like this and removing isAuthorized altogether  : 

    modifier auth() {
        require(msg.sender == owner , "ds-auth-unauthorized");
        _;
     }

# [G-08] Using Preincrement(++x) is more efficient than using Postincerement(x++)

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=_span%5Bpay_gem%5D%5Bbuy_gem%5D%2D%2D%3B

Estimated gas saved : 30 gas per instance 

using post decrement/increment costs more gas , try using pre decrement/increment instead to save some more gas 


Other instances : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=(uint256)%20%7B-,last_offer_id%2B%2B%3B,-return%20last_offer_id%3B
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=for%20(uint%20i%20%3D%200%3B%20i%20%3C%20payAmts.length%3B%20i%2B%2B)
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=ids)%20external%20%7B-,for%20(uint%20i%20%3D%200%3B%20i%20%3C%20ids.length%3B%20i%2B%2B,-)%20%7B
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=_span%5Baddress(pay_gem)%5D%5Baddress(buy_gem)%5D%2B%2B
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=_span%5Bpay_gem%5D%5Bbuy_gem%5D%2D%2D

# [G-09] Using XOR (^) and OR (|) bitwise equivalents

Link : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=function%20_sort(,)%20internal%20%7B

File : RubiconMarket.sol

Esitmated Gas Saving : 50 gas 

   function compare1(uint a , uint b
     , uint c , uint d , uint e , uint f) internal pure returns(bool){
         return (a != b || c != d || e != f);
    }

    function compare2(uint a , uint b
     , uint c , uint d , uint e , uint f) internal pure returns(bool){
         return ((a ^ b) | (c ^ d) | (e ^ f)) != 0;
    }

    in the given functions ,  the following are logical equavalent but still dont consume the same amount of gas 
    1)   (a != b || c != d || e != f);
    2)   ((a ^ b) | (c ^ d) | (e ^ f)) != 0;

    you can consider overriding the following for saving some more gas : 

    pos = pos == 0 ||
            offers[pos].pay_gem != pay_gem ||
            offers[pos].buy_gem != buy_gem ||
            !isOfferSorted(pos)
            ? _find(id)
            : _findpos(id, pos);

    to 

    pos = (((pos^ 0) |
           (offers[pos].pay_gem ^ pay_gem) |
            (offers[pos].buy_gem ^ buy_gem) ) ||
            !isOfferSorted(pos)  != 0)
            ? _find(id)
            : _findpos(id, pos);


### Logic POC : 

Given 4 variables a, b, c and d represented as such:

0 0 0 0 0 1 1 0 <- a
0 1 1 0 0 1 1 0 <- b
0 0 0 0 0 0 0 0 <- c
1 1 1 1 1 1 1 1 <- d

To have a == b means that every 0 and 1 match on both variables. Meaning that a XOR (operator ^) would evaluate to 0 ((a ^ b) == 0), as it excludes by definition any equalities.
Now, if a != b, this means that there’s at least somewhere a 1 and a 0 not matching between a and b, making (a ^ b) != 0.

Both formulas are logically equivalent and using the XOR bitwise operator costs actually the same amount of gas:

      function xOrEquivalence(uint a, uint b) external returns (bool) {
        //return a != b; //370
        //return a ^ b != 0; //370
However, it is much cheaper to use the bitwise OR operator (|) than comparing the truthy or falsy values:

    function xOrOrEquivalence(uint a, uint b, uint c, uint d) external returns (bool) {
        //return (a != b || c != d); // 495
        //return (a ^ b | c ^ d) != 0; // 442
    }
These are logically equivalent too, as the OR bitwise operator (|) would result in a 1 somewhere if any 
value is not 0 between the XOR (^) statements, meaning if any XOR (^) statement verifies that its arguments are different.


Other instances : https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#:~:text=backwards%20semantic%20compatibility.-,if%20(,%7D,-offers%5Bid%5D.pay_amt

