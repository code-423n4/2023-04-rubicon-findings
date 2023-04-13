## Title
DSAuth in RubiconMarket.sol should use two-step-owner transfer instead of directly transferring ownership.

## CodeLink
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L25-L28

## Impact
Current implementation is directly transferring of ownership. If old owner mistakenly put wrong address everything is down. Consider the owner in Rubicon Protocol has the superpower it should take additional care.

## Proof of Concept

    function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }

Same as Impact.

## Tools Used
Manual Review

## Recommended Mitigation Steps
Use two step owner transfer.



## Title
Current RubiconMarket has no function to add the offer to unsorted offer list (_near mapping).

## Impact
Based on comment and documentation this is something must do. However no function can be used add offer to _near mapping. The only function possible to do this is _offeru() and it's currently commented out.

## Proof of Concept
Same as Impact.

## Tools Used
Manual Review

## Recommended Mitigation Steps
Add entry point function to make user able to add offer to unsorted list.



## Title
V2Migrator constructor should check if the underlying asset of provided v1 and v2 token are same.

## CodeLink
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/V2Migrator.sol#L30-L35

## Impact
Constructor doesn't check if the underlying asset of provided v1 token is same the v2 token. If not same, the following migration process will fail.

## Proof of Concept

        constructor(address[] memory bathTokensV1, address[] memory bathTokensV2) {
        for (uint256 i = 0; i < bathTokensV1.length; ++i) {
            // set v1 to v2 bathTokens
            v1ToV2Pools[bathTokensV1[i]] = bathTokensV2[i];
        }
    }

The loop simply mapping v1 to v2, not checking if underlying assets are same. This is error-prone.

## Tools Used
Manual Review

## Recommended Mitigation Steps
Check bathTokensV1 underlying asset is same as bathTokenV2 underlying asset before set the map.