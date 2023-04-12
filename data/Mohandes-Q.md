Contract: SimpleMarket
Functions: buy , offer
Issue: Using require for transfer and transferFrom does not make any sense, as there will be no such case that transfer or transferFrom returns false. These functions return true by default, and revert on any error.
```
contract SimpleMarket is EventfulMarket, DSMath {
...
    function buy(
...
        require(
            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
            "Insufficient funds to cover fee"
        );
...
            if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
                require(
                    _offer.buy_gem.transferFrom(
                        msg.sender,
                        _offer.recipient,
                        mFee
                    ),
                    "Insufficient funds to cover fee"
                );
            } else {
                require(
                    _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),
                    "Insufficient funds to cover fee"
                );
            }
...
        require(
            _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
            "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
        );

        require(
            _offer.pay_gem.transfer(msg.sender, quantity),
            "_offer.pay_gem.transfer(msg.sender, quantity) failed"
        );
...
...
...
    function offer(
...
        require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
...
```
The best way to handle the output of the transfer and transferFrom function is using SafeERC20 library.
