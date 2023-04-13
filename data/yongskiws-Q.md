### [L-01] `initialize()` function can be called by anybody
The `initialize()` function is a common function used in smart contracts to initialize the state variables of the contract. It is typically called only once during the deployment of the contract and is meant to set the initial values for the contract's state variables.

However, if the `initialize()` function is not properly secured, anyone can call it even after the contract has already been initialized. This can cause unexpected behavior or even create security vulnerabilities in the contract.

Furthermore, if the `initialize()` function modifies the state variable owner, then anyone who calls the function will become the new owner of the contract, which can give them full control over the contract. This is because the owner state variable typically has special permissions or privileges within the contract.

Therefore, it is important to ensure that the `initialize()` function is properly secured and only callable by authorized parties, such as the contract owner or a specific set of authorized addresses. This can help prevent unauthorized modifications to the contract's state variables and protect against potential security vulnerabilities.

``` solidity
BathHouseV2.sol
31: 
32:     function initialize(address _comptroller, address _pAdmin) external {
33:         require(!initialized, "BathHouseV2 already initialized!");
34:         comptroller = Comptroller(_comptroller);
35:         admin = msg.sender;
36:         proxyAdmin = _pAdmin;
37: 
38:         initialized = true;
39:     }
40: 
```

``` solidity
RubiconMarket.sol
700:     function initialize(address _feeTo) public {
```


## Recommended Mitigation Steps
Add a control that makes initialize() only call the Deployer Contract or EOA
EG.
``` solidity
if (msg.sender != DEPLOYER_ADDRESS) {
            revert NotDeployer();
}
```

### [L-02] Critical Set Changes Should Use Two-step Procedure

A two-step procedure for critical set changes can be beneficial in reducing errors and improving the overall reliability of critical operations. The first step of the procedure should involve a review and approval process, where the proposed change is evaluated by a team of experts to ensure that it is necessary, appropriate, and safe.

``` solidity
BathBuddy.sol
232:     function setRewardsDuration(
233:         uint256 _rewardsDuration,
234:         address token
235:     ) external onlyOwner {
RubiconMarket.sol
25:     function setOwner(address owner_) external auth {
26:         owner = owner_;
27:         emit LogSetOwner(owner);
28:     }
RubiconMarket.sol
954: 
955:     function setMinSell(
956:         ERC20 pay_gem, //token to assign minimum sell amount to
957:         uint256 dust //maker (ask) minimum sell amount
958:     ) external auth note returns (bool) {
959:         _dust[address(pay_gem)] = dust;
960:         emit LogMinSell(address(pay_gem), dust);
961:         return true;
962:     }
RubiconMarket.sol
1466:     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
1467:         feeBPS = _newFeeBPS;
1468:         return true;
1469:     }
RubiconMarket.sol
1471:     function setMakerFee(uint256 _newMakerFee) external auth returns (bool) {
1472:         StorageSlot.getUint256Slot(MAKER_FEE_SLOT).value = _newMakerFee;
1473:         return true;
1474:     }
1475: 
1476:     function setFeeTo(address newFeeTo) external auth returns (bool) {
1477:         require(newFeeTo != address(0));
1478:         feeTo = newFeeTo;
1479:         return true;
1480:     }
```

## Recommended Mitigation Steps
Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions

### [L-03] Lack of ACK during owner change
It's possible to lose the ownership under specific circumstances.

Because an human error it's possible to set a new invalid owner. When you want to change the owner's address it's better to propose a new owner, and then accept this ownership with the new wallet.

``` solidity
Position.sol
15: contract Position is Ownable, DSMath {
```

### [L-04] Use a more recent version of Solidity

Context: All contracts

Description: For security, it is best practice to use the latest Solidity version. For the security fix list in the versions; https://github.com/ethereum/solidity/blob/develop/Changelog.md

Recommendation: Old version of Solidity change to version can be used (0.8.17)

### [L-05] Open TODOs in Contract

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

``` solidity
BathBuddy.sol
37: // TODO: not really the case anymore ^

BathBuddy.sol
91: 
92:     // TODO: do we need this?

Position.sol
156:     // TODO: definitely need to work on naming things

RubiconMarket.sol
99:  /// TODO: double check it is sound logic to kill this event
128:     /// TODO: double check it is sound logic to kill this event
184:     /// TODO: double check it is sound logic to kill this event
197: /// TODO: we will need to make sure this emit is included in any taker pay maker     
299:         /// TODO: double check it is sound logic to kill this event
428:         /// TODO: double check it is sound logic to kill this event
661: /// TODO:
663:  /// TODO: 
666:  TODO: 
675:  TODO:
```


### [L-06] Rug vectors by the owner frontrun

``` solidity
BathBuddy.sol
194:     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
BathBuddy.sol
232:     function setRewardsDuration(
233:         uint256 _rewardsDuration,
234:         address token
235:     ) external onlyOwner {
Position.sol
93:     function buyAllAmountWithLeverage(
94:         address quote,
95:         address asset,
96:         uint256 quotePayAmount,
97:         uint256 leverage
98:     ) external onlyOwner {
107:     function sellAllAmountWithLeverage(
108:         address asset,
109:         address quote,
110:         uint256 assetPayAmount,
111:         uint256 leverage
112:     ) external onlyOwner {
210:     function closePosition(uint256 posId) external onlyOwner {
226:     function increaseMargin(uint256 posId, uint256 amount) external onlyOwner {
242:     function withdraw(address token, uint256 amount) external onlyOwner {
```
## Recommended Mitigation Steps
As a mitigation add a timelock and make sure the owner is a multisig and not an EOA.


### [L-07] Lack of Input Validation
Lack of input validation is the process of checking input data to ensure that it is valid and safe to process. Therefore, it is important for contracts to consider the lack of input validation to ensure that what is produced is secure and protected from potential security vulnerabilities.

## Recommended Mitigation Steps

```diff
FeeWrapper.sol
65: 
66:         (bool _OK, bytes memory _data) = _params.target.call(
67:             bytes.concat(_params.selector, _params.args)
68:         );
69: 
-70:         require(_OK, "low-level call to the Rubicon failed");
+70:         if (!_OK)

FeeWrapper.sol
82:     (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
83:             bytes.concat(_params.selector, _params.args)
84:         );
85: 
-86:         require(_OK, "low-level call to the router failed");
+70:         if (!_OK)

FeeWrapper.sol
117:         // transfer fee to the 3rd party protocol
118:         (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
-119: 		require(OK, "ETH transfer failed");
+70:         if (!_OK)
```


### [N-01] NatSpec comments should be increased in contracts
Context: All Contracts

Description: It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability. https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

Recommendation: NatSpec comments should be increased in contracts

### [N-02] For modern and more readable code; update import usages
Description: Solidity code is also cleaner in another way that might not be noticeable: the struct Point. We were importing it previously with global import but not using it. The Point struct polluted the source code with an unnecessary object we were not using because we did not need it. This was breaking the rule of modularity and modular programming: only import what you need Specific imports with curly braces allow us to apply this rule better.

## Recommendation
import {contract1 , contract2} from "filename.sol";

### [N-03] Function writing that does not comply with the Solidity Style Guide

Context: All Contracts

Description: Order of Functions; ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this.

https://docs.soliditylang.org/en/v0.8.17/style-guide.html

Functions should be grouped according to their visibility and ordered:

constructor receive function (if exists) fallback function (if exists) external public internal private within a grouping, place the view and pure functions last


### [N-04] Include return parameters in NatSpec comments

Context: All Contracts

Description: It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

## Recommendation
Include return parameters in NatSpec comments

``` diff
+/// @return add comment for return parameters "data"
eaperVaultERC4626.sol
FeeWrapper.sol
60:   function _rubicall(
61:         CallParams memory _params
62:     ) internal returns (bytes memory) {
63:         // charge fee from feeParams
64:         _chargeFee(_params.feeParams, _params.target);
65: 
66:         (bool _OK, bytes memory _data) = _params.target.call(
67:             bytes.concat(_params.selector, _params.args)
68:         );
69: 
70:         require(_OK, "low-level call to the Rubicon failed");
71: 
72:         return _data;
73:     }
```
