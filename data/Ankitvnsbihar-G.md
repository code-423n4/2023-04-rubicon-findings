1. In RubiconMarket.sol contract 

*changing emit LogSetOwner(owner) to emit LogSetOwner(owner_) will save gas upto 153(in line 25 of  RubiconMarket.sol contract )

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol


 function setOwner(address owner_) external auth {
       owner = owner_;
        emit LogSetOwner(owner);
    }

CHANGE TO:

 function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner_);
    }




2.In line 234 of RubiconMarket.sol contract ( Arrage in ascending order to save 18 gas )

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol



 struct OfferInfo {
        uint256 pay_amt;
        ERC20 pay_gem;
        uint256 buy_amt;
        ERC20 buy_gem;
        address recipient;
        uint64 timestamp;
        address owner;
    }

change to...
struct OfferInfo {
        uint64 timestamp;
        address owner;
        address recipient;
        ERC20 buy_gem;
        ERC20 pay_gem;
        uint256 buy_amt;
        uint256 pay_amt;
    }


