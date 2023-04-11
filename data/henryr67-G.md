### Title: Limit maximum offer duration

## Type of issue: Improvement

## Severity: Low

## Description:

The current implementation of the offer function allows users to create an offer that lasts for an extremely long time, without any checks or limits in place. Users can unintentionally create offers that last for an excessive amount of time, such as 531 years (2^64).

## Proposed Solution:

To prevent users from making this mistake, add a check to enforce a maximum duration for offers. This can be done by including a condition within the offer function to ensure the specified duration does not exceed the allowed maximum:

```
uint256 maxDuration = 365 * 24 * 60 * 60; // For example, a maximum of 1 year

require(
    block.timestamp + duration <= block.timestamp + maxDuration,
    "Offer duration exceeds the allowed maximum"
);
```
By implementing this check, the contract can ensure that offers do not last longer than the allowed maximum duration. The choice of maximum duration should depend on the specific requirements and use case of the contract.

this is a low/non critical issue, but I made the mistake of not reading the docs. So here I send it as a gas optimization. Sorry. Next time I won't do it.