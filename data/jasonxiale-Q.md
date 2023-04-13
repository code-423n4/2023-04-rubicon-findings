# emitFee wrong parameter
File: contracts/RubiconMarket.sol, line 368 should be **feeTo**

    365        emit emitFee(
    366            bytes32(id),
    367            msg.sender,
    368            _offer.owner,
    369            keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
    370            _offer.buy_gem,
    371            mFee
    372        );

# need to make sure RubiconMarket.batchRequote's parameters share the same length
in function [RubiconMarket.batchRequote](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L933)

    function batchRequote(
        uint[] calldata ids,
        uint[] calldata payAmts,
        address[] calldata payGems,
        uint[] calldata buyAmts,
        address[] calldata buyGems
    ) external {
    +    require(ids.length == payAmts.length);
    +    require(ids.length == payGems.length);
    +    require(ids.length == buyAmts.length);
    +    require(ids.length == buyGems.length);

        for (uint i = 0; i < ids.length; i++) {
            cancel(ids[i]);
            this.offer(
                payAmts[i],
                ERC20(payGems[i]),
                buyAmts[i],
                ERC20(buyGems[i])
            );
        }
    }