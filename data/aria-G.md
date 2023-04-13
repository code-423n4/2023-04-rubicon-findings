Gas optimisation:
	
There are a few instances of unneeded modifiers in the code specifically in the 
RubiconMarket contract, for example:

line 780:

    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint pos, //position to insert offer, 0 should be used if unknown
        bool rounding
    ) external can_offer returns (uint256) {
        return
            offer(
                pay_amt,
                pay_gem,
                buy_amt,
                buy_gem,
                pos,
                rounding,
                msg.sender,
                msg.sender
            );
    }

When using the offer functions some of them link directly to another 
offer function but both have the can_offer modifier.

This is the same in other offer function, and should probably be fixed by deleting the first Instance of the can_offer modifier in each of them and keeping the modifier in the main 
offer function.

Medium error:
In the ExpiringMarket contract which is inherited from our RubiconMarket contract, there
is a weird mistake. The main use of the contract is to give owner the ability to stop
the contract at some point of time. In our case the function isClosed that should return
the stopped bool, is declared pure and returns false;


    function isClosed() public pure returns (bool closed) {
        return false;
    }


I assume the developer had a different idea in mind, but currently this contract has no 
effect on the market except in some cases where this function is called and just wastes 
gas. For example in the can_cancel and can offer modifier :


line 719:

    // // After close, anyone can cancel an offer
    modifier can_cancel(uint256 id) override {
        require(isActive(id), "Offer was deleted or taken, or never existed.");
        require(
            isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );
        _;
    }
---------------------------------

line 596: 
    /// @dev After close_time has been reached, no new offers are allowed.
    modifier can_offer() override {
        require(!isClosed());
        _;
    }

    /// @dev After close, no new buys are allowed.
    modifier can_buy(uint256 id) override {
        require(isActive(id));
        require(!isClosed());
        _;
    }



If the developer wanted this functionality in the contract I would suggest to fix the 
isClosed function, otherwise the entire ExpiringMarket contract should be removed.