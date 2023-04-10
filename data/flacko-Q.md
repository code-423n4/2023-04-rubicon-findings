# Useless if-else clause in RubiconMarket.sol `DSAuth#isAuthorized`

[https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L35-L41)

The method can be simplified, dropping the if-else clause and returning the result of the comparison instead.

Before:

```
    function isAuthorized(address src) internal view returns (bool) {
        if (src == owner) {
            return true;
        } else {
            return false;
        }
    }
```

After:

```
    function isAuthorized(address src) internal view returns (bool) {
        return src == owner;
    }

```

# Useless modifier in RubiconMarket.sol -`DSAuth#isAuthorized`

I checked if this method (`isAuthorized`) is used elsewhere in the codebase, but couldn’t find any occurrences besides the `DSAuth#auth` modifier. Thus I believe the modifier can be simplified to and the `isAuthorized` function can be removed.

```jsx
modifier auth() {
    require(msg.sender == owner, "ds-auth-unauthorized");
    _;
}
```