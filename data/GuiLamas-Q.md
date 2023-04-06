*** FeeWrapper ***

The functions _chargeFee (line 93) and _chargeFeePayable (line 108) lacks validation/security checks to prevent unauthorized access, since others contracts are supposed to interact with it there's a potential risk that both sides lack validation. Therefore, I reccomend adding security checks to prevent future attacks.

 