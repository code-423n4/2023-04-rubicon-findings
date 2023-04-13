*** FeeWrapper ***

The functions _chargeFee and _chargeFeePayable lacks validation/security checks to prevent unauthorized access, since others contracts are supposed to interact with it there's a potential risk that both sides lack validation. Therefore, I reccomend adding security checks to prevent future attacks.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L93-L121

 