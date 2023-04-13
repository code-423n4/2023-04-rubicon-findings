# [NC-01] Confirmed with Sponsor anyone can call del_rank function in RubiconMarket but the comment mentions it's only to be called by Keepers.
## Context
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L936-L948
```
    //  Function should be called by keepers.
    function del_rank(uint256 id) external returns (bool) {
        require(!locked);

        require(
            !isActive(id) &&
                _rank[id].delb != 0 &&
                _rank[id].delb < block.number - 10
        );
        delete _rank[id];
        emit LogDelete(msg.sender, id);
        return true;
    }
```
## Description
The comments say the function should only be called by keepers. I confirmed with the Sponsor and they said the function is not used right now and anyone may call it, but the comments show it is only to be called by Keepers.

## Resolution
Remove comments to not cause any doubt or if it's not going to be used, remove the function entirely.