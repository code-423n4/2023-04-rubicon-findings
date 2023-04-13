1.Initializers could be front-run(Excluding Automatic findings of this contest)
->Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment.
->Here I have excluded instances which were listed in Automated Findings / Publicly Known Issues.

Instances (1):
File: contracts/periphery/BathBuddy.sol
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L71-L85


2.Function Calls in Loop Could Lead to Denial of Service because of Out Of Gas Error
->Function calls made in an unbounded loop/(very high number of iterations) are error-prone with potential resource exhaustion as it can trap the contract due to the gas limitations or failed transactions.
->Consider bounding the loop where possible to avoid unnecessary denial of service.
-> Or split loop transactions in batches.
->Here are some of the instances entailed:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L899-906
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L911-L913
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L924-L932
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1037-L1064
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1077-L1106
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1452-L1456

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/BathHouseV2.sol#L122-L127

https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L158-L192
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L541-L582


3.Add a Timelock to Critical Parameter Change
->It is a good practice to give time for users to react and adjust to critical changes with a mandatory time window between them. The first step merely broadcasts to users that a particular change is coming, and the second step commits that change after a suitable waiting period. This allows users that do not accept the change to withdraw within the grace period. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious Owner making any malicious or ulterior intention). Specifically, privileged roles could use front running to make malicious changes just ahead of incoming transactions, or purely accidental negative effects could occur due to the unfortunate timing of changes.
->This will also protect the user and protocol when the owner mistakenly updates any important state parameter with the wrong value. 
->Consider extending the timelock feature beyond contract ownership management to business critical functions/parameters.
->Here are some of the instances entailed:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L962
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1469
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471-L1474
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1476-L1480


4.Check if the new value is not the same as the old one.
->Many functions are used for updating values of certain state variables.
->These setter functions also emit events when the state variable value is updated. So if the new value is the same as the old one, then emitted events will create confusion for users.
->So In those functions first check if the new provided value is not the same as the old value and then only change it,only if the new value is different than the old one.

->Here are some of the instances entailed:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L955-L962
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1469
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471-L1474
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1476-L1480
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L232-L242


5.Emit both old and new value when critical state variable value gets updated
->Currently code is not emitting any event when critical state variables like feeBPS, makerFee, feeTo and many others get updated.
->So emit both new and old value when critical state variable gets updated

->Here are some of the instances entailed:
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1466-L1469
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1471-L1474
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1476-L1480


6.Unlocked pragma can make contracts deployed accidently with compiler version other than it was tested with
RubiconMarket and BathBuddy contracts use an unlocked pragma (e.g., pragma solidity ^0.8.0) which is not fixed to a specific Solidity version. 
->Locking the pragma helps ensure that contracts do not accidentally get deployed using a different compiler version with which they have been tested the most. 

->Recommend locking pragmas to a specific Solidity version. Consider the compiler bugs in the following lists and ensure the contracts are not affected by them. It is also recommended to use the latest version of Solidity when deploying contracts (see Solidity docs : https://docs.soliditylang.org/en/v0.8.6/).

->Solidity compiler bugs: 
->Solidity repo - known bugs https://github.com/ethereum/solidity/blob/develop/docs/bugs.json
->Solidity repo - bugs by version
https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json


7.Missing checks for arr length for batchRequote function
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L917-L923

->in batchRequote function 5 arrays are passed as parameters.
->Technically all 5 arrays passed in parameter should have the same length, otherwise the call will revert. But here the function batchRequote is not checking the same. 
-> You can add the below test inside ProtocolDeployement.ts file at line number 275.

               it("batch Requote works correctly?", async function () {
                 const { testStableCoin, testCoin, rubiconMarket, owner, otherAccount } =
                   await loadFixture(deployRubiconProtocolFixture);
                   await testCoin!
                   .connect(owner)
                   .approve(rubiconMarket.address, parseUnits("100000000"));
                 await testStableCoin!
                   .connect(owner)
                   .approve(rubiconMarket.address, parseUnits("100000000"));
            
                   await rubiconMarket.functions["offer(uint256,address,uint256,address)"](
                     parseUnits("120", 6),
                     testStableCoin.address,
                     parseUnits("130"),
                     testCoin.address,
                     { from: owner.address }
                   );
                   await rubiconMarket.functions["offer(uint256,address,uint256,address)"](
                     parseUnits("140", 6),
                     testStableCoin.address,
                     parseUnits("170"),
                     testCoin.address,
                     { from: owner.address }
                   );
                 const tx = rubiconMarket.functions["batchRequote(uint256[],uint256[],address[],uint256[],address[])"](
                   [6,7],
                   [],
                   [testStableCoin.address,testStableCoin.address],
                   [parseUnits("120"),parseUnits("160")],
                   [testCoin.address,testCoin.address],
                   { from: owner.address }
                 );
                 // console.log(rubiconMarket.functions)
                 await expect(tx)
                 .to.be.revertedWith("INVLAID ARRAY VALUES");
            
               });




->It should fail with INVALID ARRAY VALUE error but it is reverting with panic code 0x32 (Array accessed at an out-of-bounds or negative index)
->So add below check inside batchRequote function at line number 924.

                        require(
                            ids.length == payAmts.length &&
                                ids.length == payGems.length &&
                                ids.length == buyAmts.length &&
                                ids.length == buyGems.length,
                            "Array lengths do not match"
                        );

8.Validate User Inputs correctly
->calculateFee function should handle user input validations correctly.
->Currently it is experiencing division by zero error when the user passes feeType as 0.
https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/FeeWrapper.sol#L28-L44

->It should revert with a proper error message when the user adds invalid value for feeType parameter, but here it is reverting with EVM based panic codes which is confusing from user's perspective because users will not be familiar with the codebase of this protocol.
->Add below test inside fee-wrapper.ts file at line number 158

           it("calculateFee should work correctly", async function () {
             const { feeWrapper } = await loadFixture(
               deployRubiProtocolFixture
             );
             const tokenAmounts = [parseUnits("10",18),parseUnits("20",18),parseUnits("30",18)];
             var feeType = parseUnits("10000");
             var feeValue = parseUnits("10");
             const res = await feeWrapper.calculateFee(tokenAmounts,feeType,feeValue);
             for(var i=0;i<tokenAmounts.length;i++){
               expect(res.amountsWithFee[i]).to.be.equal(tokenAmounts[i].sub((tokenAmounts[i].mul(feeValue)).div(feeType)));
               expect(res.fees[i]).to.be.equal(tokenAmounts[i].mul(feeValue).div(feeType));
             }


             feeType = parseUnits("0");
             feeValue = parseUnits("10");
             const res2 = feeWrapper.calculateFee(tokenAmounts,feeType,feeValue);
             await expect( res2)
             .to.be.revertedWith("INVLAID FEETYPE VALUE");
           });


 
-> Here test should failed with 'INVALID FEETYPE VALUE' error, 
    but it actually reverted with panic code 0x12 (Division or modulo division by zero)
     
->So we are recommending adding a required check for checking if feeType value is valid or not.    
->Add below code at line number 37 in FeeWrapper.sol

                 require(feeType!=0,"INVLAID FEETYPE VALUE");

Non Critical
1.Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)
->There are 10 instances of this issue:
->https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L74

File: contracts/RubiconMarket.sol
74:       uint256 constant WAD = 10**18;
75:       uint256 constant RAY = 10**27;
1057:                      mul(pay_amt, 10 ** 9),
1059:                  ) / 10**9;
1097:                          mul(buy_amt, 10 ** 9),
1101:                      ) / 10**9
1142:                  mul(pay_amt, 10 ** 9),
1144:              ) / 10**9
1175:                  mul(buy_amt, 10 ** 9),
1177:              ) / 10**9

