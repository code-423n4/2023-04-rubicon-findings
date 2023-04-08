## QA-1
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L96
contract EventfulMarket should be marked as abstract as it isn't planned to compile it

## QA-2
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L695
Looks like unsorted order type isn't used and `_near` mapping isn't filled anywhere with non-zero data 
 
Also `_head` is always zero  
We don't need these two things

## QA-3
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L887
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917
good practice to check arrays are not empty

## QA-4
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917
equal arrays length shall be checked
