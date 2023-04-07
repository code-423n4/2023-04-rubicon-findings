# GAS OPTIMIZATION REPORT 

## G1 Save the value directly in the storage mapping offers

Instead of create a memory variable and use to assign the values in memory to then save the value in the storage variable , assign the value directly in the storage.

The offer function is creating a memory variable to keep the input paramatres and then pass it to the offers variable, insted of do this:

```
contracts/RubiconMarket.sol:
    function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
        require(uint128(pay_amt) == pay_amt);
        require(uint128(buy_amt) == buy_amt);
        require(pay_amt > 0);
        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
        require(buy_amt > 0);
        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
        require(pay_gem != buy_gem);

        OfferInfo memory info; //@audit gas optmization is more cheaper 
        info.pay_amt = pay_amt;
        info.pay_gem = pay_gem;
        info.buy_amt = buy_amt;
        info.buy_gem = buy_gem;
        info.recipient = recipient;
        info.owner = owner;
        info.timestamp = uint64(block.timestamp);
        id = _next_id();
        offers[id] = info;

```
Do this:

```
    function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
        require(uint128(pay_amt) == pay_amt);
        require(uint128(buy_amt) == buy_amt);
        require(pay_amt > 0);
        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
        require(buy_amt > 0);
        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
        require(pay_gem != buy_gem);

        id = _next_id();
        offers[id]= OfferInfo(pay_amt, pay_gem, buy_amt, buy_gem, recipient, owner, uint64(block.timestamp) );
   }

```

Doing the change in remix you are saving aproximatly 137224 in gas





