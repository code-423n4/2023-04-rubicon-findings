Files analyzed:
- 2023-04-rubicon/contracts/BathHouseV2.sol
- 2023-04-rubicon/contracts/RubiconMarket.sol
- 2023-04-rubicon/contracts/V2Migrator.sol
- 2023-04-rubicon/contracts/compound-v2-fork/BaseJumpRateModelV2.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegate.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CErc20Immutable.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CEther.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CToken.sol
- 2023-04-rubicon/contracts/compound-v2-fork/CTokenInterfaces.sol
- 2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol
- 2023-04-rubicon/contracts/compound-v2-fork/ComptrollerInterface.sol
- 2023-04-rubicon/contracts/compound-v2-fork/ComptrollerStorage.sol
- 2023-04-rubicon/contracts/compound-v2-fork/DAIInterestRateModelV3.sol
- 2023-04-rubicon/contracts/compound-v2-fork/EIP20Interface.sol
- 2023-04-rubicon/contracts/compound-v2-fork/EIP20NonStandardInterface.sol
- 2023-04-rubicon/contracts/compound-v2-fork/ErrorReporter.sol
- 2023-04-rubicon/contracts/compound-v2-fork/ExponentialNoError.sol
- 2023-04-rubicon/contracts/compound-v2-fork/InterestRateModel.sol
- 2023-04-rubicon/contracts/compound-v2-fork/JumpRateModel.sol
- 2023-04-rubicon/contracts/compound-v2-fork/JumpRateModelV2.sol
- 2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol
- 2023-04-rubicon/contracts/compound-v2-fork/Maximillion.sol
- 2023-04-rubicon/contracts/compound-v2-fork/PriceOracle.sol
- 2023-04-rubicon/contracts/compound-v2-fork/Reservoir.sol
- 2023-04-rubicon/contracts/compound-v2-fork/SafeMath.sol
- 2023-04-rubicon/contracts/compound-v2-fork/SimplePriceOracle.sol
- 2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol
- 2023-04-rubicon/contracts/compound-v2-fork/Unitroller.sol
- 2023-04-rubicon/contracts/compound-v2-fork/WhitePaperInterestRateModel.sol
- 2023-04-rubicon/contracts/interfaces/IBathBuddy.sol
- 2023-04-rubicon/contracts/interfaces/IBathToken.sol
- 2023-04-rubicon/contracts/interfaces/IRubiconMarket.sol
- 2023-04-rubicon/contracts/interfaces/ISwapRouter.sol
- 2023-04-rubicon/contracts/interfaces/IWETH.sol
- 2023-04-rubicon/contracts/periphery/BathBuddy.sol
- 2023-04-rubicon/contracts/periphery/BathTokenV1.sol
- 2023-04-rubicon/contracts/periphery/DummyPriceOracle.sol
- 2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol
- 2023-04-rubicon/contracts/periphery/Test3rdPartyProtocol.sol
- 2023-04-rubicon/contracts/periphery/TokenWithFaucet.sol
- 2023-04-rubicon/contracts/periphery/WETH9.sol
- 2023-04-rubicon/contracts/proxy/RubiconProxy.sol
- 2023-04-rubicon/contracts/utilities/FeeWrapper.sol
- 2023-04-rubicon/contracts/utilities/MarketAid.sol
- 2023-04-rubicon/contracts/utilities/MarketAidFactory.sol
- 2023-04-rubicon/contracts/utilities/RubiconRouter.sol
- 2023-04-rubicon/contracts/utilities/poolsUtility/PoolsUtility.sol
- 2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol

Issues found:
 G001:
  2023-04-rubicon/contracts/BathHouseV2.sol::122 => for (uint256 i = 0; i < buddies.length; ++i) {
  2023-04-rubicon/contracts/RubiconMarket.sol::899 => for (uint i = 0; i < payAmts.length; i++) {
  2023-04-rubicon/contracts/RubiconMarket.sol::911 => for (uint i = 0; i < ids.length; i++) {
  2023-04-rubicon/contracts/RubiconMarket.sol::924 => for (uint i = 0; i < ids.length; i++) {
  2023-04-rubicon/contracts/V2Migrator.sol::31 => for (uint256 i = 0; i < bathTokensV1.length; ++i) {
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::81 => uint startingAllowance = 0;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::176 => for (uint i = 0; i < len; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::275 => for (uint i = 0; i < len; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::907 => for (uint i = 0; i < assets.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1232 => for (uint i = 0; i < allMarkets.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1289 => for (uint i = 0; i < numMarkets; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1424 => for (uint i = 0; i < affectedUsers.length; ++i) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1723 => for (uint i = 0; i < cTokens.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1729 => for (uint j = 0; j < holders.length; j++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1739 => for (uint j = 0; j < holders.length; j++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1744 => for (uint j = 0; j < holders.length; j++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1807 => for (uint i = 0; i < numTokens; ++i) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::73 => uint compSupplySpeed = 0;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::85 => uint compBorrowSpeed = 0;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::131 => uint borrowCap = 0;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::167 => for (uint i = 0; i < cTokenCount; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::212 => for (uint i = 0; i < cTokenCount; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::236 => for (uint i = 0; i < cTokenCount; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::269 => //     for (uint i = 0; i < proposalCount; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::291 => //     for (uint i = 0; i < proposalCount; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::344 => //     for (uint i = 0; i < proposalIds.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::405 => for (uint i = 0; i < proposalIds.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::478 => //     for (uint i = 0; i < blockNumbers.length; i++) {
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::586 => for (uint256 index = 0; index < bonusTokens.length; index++) {
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::40 => for (uint256 i = 0; i < tokenAmounts.length; ++i) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::245 => for (uint i = 0; i < data.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::345 => bool assigned = false;
  2023-04-rubicon/contracts/utilities/MarketAid.sol::346 => for (uint256 index = 0; index < array.length; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::571 => for (uint256 index = 0; index < quantity; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::748 => for (uint256 index = 0; index < ids.length; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::781 => for (uint i = 0; i < erc20s.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::796 => for (uint i = 0; i < erc20s.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::815 => for (uint i = 0; i < erc20s.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::921 => uint256 amountOut = 0;
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1025 => uint256 quoteOnChainBalance = 0;
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1026 => uint256 assetOnChainBalance = 0;
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1028 => for (uint256 index = 0; index < stratBook.length; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1082 => for (uint256 index = 0; index < stratBook.length; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::92 => for (uint256 i = 0; i < tokens.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::107 => for (uint256 i = 0; i < offerIDs.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::143 => for (uint256 index = 0; index < size; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::167 => for (uint256 i = 0; i < size; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::226 => for (uint256 i = 0; i < route.length - 1; i++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::253 => for (uint256 i = 0; i < routes.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::255 => for (uint256 n = 0; n < routes[i].length - 1; ++n) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::283 => bool assigned = false;
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::284 => for (uint256 index = 0; index < array.length; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::300 => for (uint256 index = 0; index < targetBathTokens.length; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::316 => for (uint256 i = 0; i < routes.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::706 => for (uint256 i = 0; i < route.length - 1; ++i) {
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::158 => for (uint256 i = 0; i < vars.limit; ++i) {

 G002:
  2023-04-rubicon/contracts/BathHouseV2.sol::122 => for (uint256 i = 0; i < buddies.length; ++i) {
  2023-04-rubicon/contracts/RubiconMarket.sol::894 => payAmts.length == payGems.length &&
  2023-04-rubicon/contracts/RubiconMarket.sol::895 => payAmts.length == buyAmts.length &&
  2023-04-rubicon/contracts/RubiconMarket.sol::896 => payAmts.length == buyGems.length,
  2023-04-rubicon/contracts/RubiconMarket.sol::897 => "Array lengths do not match"
  2023-04-rubicon/contracts/RubiconMarket.sol::899 => for (uint i = 0; i < payAmts.length; i++) {
  2023-04-rubicon/contracts/RubiconMarket.sol::911 => for (uint i = 0; i < ids.length; i++) {
  2023-04-rubicon/contracts/RubiconMarket.sol::924 => for (uint i = 0; i < ids.length; i++) {
  2023-04-rubicon/contracts/V2Migrator.sol::31 => for (uint256 i = 0; i < bathTokensV1.length; ++i) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::173 => uint len = cTokens.length;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::273 => uint len = userAssetList.length;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::285 => // copy last item in list to location of item to be removed, reduce length by 1
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::287 => storedList[assetIndex] = storedList[storedList.length - 1];
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::907 => for (uint i = 0; i < assets.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1232 => for (uint i = 0; i < allMarkets.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1281 => uint numMarkets = cTokens.length;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1282 => uint numBorrowCaps = newBorrowCaps.length;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1415 => require(affectedUsers.length == amounts.length, "Invalid input");
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1424 => for (uint i = 0; i < affectedUsers.length; ++i) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1723 => for (uint i = 0; i < cTokens.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1729 => for (uint j = 0; j < holders.length; j++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1739 => for (uint j = 0; j < holders.length; j++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1744 => for (uint j = 0; j < holders.length; j++) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1800 => uint numTokens = cTokens.length;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1802 => numTokens == supplySpeeds.length &&
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1803 => numTokens == borrowSpeeds.length,
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::165 => uint cTokenCount = cTokens.length;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::210 => uint cTokenCount = cTokens.length;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::234 => uint cTokenCount = cTokens.length;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::267 => //     uint proposalCount = proposalIds.length;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::289 => //     uint proposalCount = proposalIds.length;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::343 => //     GovProposal[] memory res = new GovProposal[](proposalIds.length);
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::344 => //     for (uint i = 0; i < proposalIds.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::404 => GovBravoProposal[] memory res = new GovBravoProposal[](proposalIds.length);
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::405 => for (uint i = 0; i < proposalIds.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::477 => //     CompVotes[] memory res = new CompVotes[](blockNumbers.length);
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::478 => //     for (uint i = 0; i < blockNumbers.length; i++) {
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::93 => if (bytes(signature).length == 0) {
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::585 => if (bonusTokens.length > 0) {
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::586 => for (uint256 index = 0; index < bonusTokens.length; index++) {
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::37 => amountsWithFee = new uint256[](tokenAmounts.length);
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::38 => fees = new uint256[](tokenAmounts.length);
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::40 => for (uint256 i = 0; i < tokenAmounts.length; ++i) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::245 => for (uint i = 0; i < data.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::321 => current[target] = current[current.length - 1];
  2023-04-rubicon/contracts/utilities/MarketAid.sol::346 => for (uint256 index = 0; index < array.length; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::562 => askNumerators.length == askDenominators.length &&
  2023-04-rubicon/contracts/utilities/MarketAid.sol::563 => askDenominators.length == bidNumerators.length &&
  2023-04-rubicon/contracts/utilities/MarketAid.sol::564 => bidNumerators.length == bidDenominators.length,
  2023-04-rubicon/contracts/utilities/MarketAid.sol::565 => "not all order lengths match"
  2023-04-rubicon/contracts/utilities/MarketAid.sol::567 => uint256 quantity = askNumerators.length;
  2023-04-rubicon/contracts/utilities/MarketAid.sol::663 => askNumerators.length == askDenominators.length &&
  2023-04-rubicon/contracts/utilities/MarketAid.sol::664 => askDenominators.length == bidNumerators.length &&
  2023-04-rubicon/contracts/utilities/MarketAid.sol::665 => bidNumerators.length == bidDenominators.length &&
  2023-04-rubicon/contracts/utilities/MarketAid.sol::666 => ids.length == askNumerators.length,
  2023-04-rubicon/contracts/utilities/MarketAid.sol::667 => "not all input lengths match"
  2023-04-rubicon/contracts/utilities/MarketAid.sol::748 => for (uint256 index = 0; index < ids.length; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::781 => for (uint i = 0; i < erc20s.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::795 => require(_admin != address(0) && erc20s.length == amounts.length);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::796 => for (uint i = 0; i < erc20s.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::814 => require(_admin != address(0) && erc20s.length == amounts.length);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::815 => for (uint i = 0; i < erc20s.length; i++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1027 => if (stratBook.length > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1028 => for (uint256 index = 0; index < stratBook.length; index++) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1079 => if (stratBook.length > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1080 => ordersOnBook = new MarketOffer[](stratBook.length);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1082 => for (uint256 index = 0; index < stratBook.length; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::92 => for (uint256 i = 0; i < tokens.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::107 => for (uint256 i = 0; i < offerIDs.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::226 => for (uint256 i = 0; i < route.length - 1; i++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::253 => for (uint256 i = 0; i < routes.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::255 => for (uint256 n = 0; n < routes[i].length - 1; ++n) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::284 => for (uint256 index = 0; index < array.length; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::300 => for (uint256 index = 0; index < targetBathTokens.length; index++) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::316 => for (uint256 i = 0; i < routes.length; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::486 => route[route.length - 1] == wethAddress,
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::701 => require(route.length > 1, "Not enough hop destinations!");
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::706 => for (uint256 i = 0; i < route.length - 1; ++i) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::729 => IERC20(route[route.length - 1]).safeTransfer(to, currentAmount);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::736 => ///     route[route.length - 1],
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::737 => ///     keccak256(abi.encodePacked(route[0], route[route.length - 1])),
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::745 => route[route.length - 1],
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::746 => keccak256(abi.encodePacked(route[0], route[route.length - 1])),

 G003:
  2023-04-rubicon/contracts/RubiconMarket.sol::277 => return offers[id].timestamp > 0;
  2023-04-rubicon/contracts/RubiconMarket.sol::345 => if (makerFee() > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::521 => require(pay_amt > 0);
  2023-04-rubicon/contracts/RubiconMarket.sol::523 => require(buy_amt > 0);
  2023-04-rubicon/contracts/RubiconMarket.sol::581 => require(amount > 0);
  2023-04-rubicon/contracts/RubiconMarket.sol::585 => if (makerFee() > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::1037 => while (pay_amt > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::1077 => while (buy_amt > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::1133 => if (pay_amt > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::1166 => if (buy_amt > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::1209 => require(id > 0);
  2023-04-rubicon/contracts/RubiconMarket.sol::1226 => require(id > 0);
  2023-04-rubicon/contracts/RubiconMarket.sol::1289 => while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
  2023-04-rubicon/contracts/RubiconMarket.sol::1325 => t_buy_amt > 0 &&
  2023-04-rubicon/contracts/RubiconMarket.sol::1326 => t_pay_amt > 0 &&
  2023-04-rubicon/contracts/RubiconMarket.sol::1410 => require(_span[pay_gem][buy_gem] > 0);
  2023-04-rubicon/contracts/RubiconMarket.sol::1452 => while (uid > 0 && uid != id) {
  2023-04-rubicon/contracts/V2Migrator.sol::41 => require(bathBalance > 0, "migrate: ZERO AMOUNT");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::37 => require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::488 => /* If redeemTokensIn > 0: */
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::489 => if (redeemTokensIn > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::400 => if (shortfall > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::425 => if (redeemTokens == 0 && redeemAmount > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::484 => if (shortfall > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1447 => if (amountToSubtract > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1522 => if (deltaBlocks > 0 && supplySpeed > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1525 => Double memory ratio = supplyTokens > 0
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1533 => } else if (deltaBlocks > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1554 => if (deltaBlocks > 0 && borrowSpeed > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1560 => Double memory ratio = borrowAmount > 0
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1568 => } else if (deltaBlocks > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1679 => if (deltaBlocks > 0 && compSpeed > 0) {
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1765 => if (amount > 0 && amount <= compRemaining) {
  2023-04-rubicon/contracts/compound-v2-fork/SafeMath.sol::154 => require(b > 0, errorMessage);
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::180 => if (reward > 0) {
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::585 => if (bonusTokens.length > 0) {
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::289 => return offers[id].timestamp > 0;
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::442 => require(pay_amt > 0);
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::444 => require(buy_amt > 0);
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::874 => while (pay_amt > 0) {
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::913 => while (buy_amt > 0) {
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::955 => if (pay_amt > 0) {
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::979 => if (buy_amt > 0) {
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1022 => require(id > 0);
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1039 => require(id > 0);
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1100 => while (_best[address(t_buy_gem)][address(t_pay_gem)] > 0) {
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1136 => t_buy_amt > 0 &&
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1137 => t_pay_amt > 0 &&
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1212 => require(_span[pay_gem][buy_gem] > 0);
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::1254 => while (uid > 0 && uid != id) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::285 => // if delta > 0 - delta is fill => handle any amount of fill here
  2023-04-rubicon/contracts/utilities/MarketAid.sol::286 => if (askDelta > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::300 => // if delta > 0 - delta is fill => handle any amount of fill here
  2023-04-rubicon/contracts/utilities/MarketAid.sol::301 => if (bidDelta > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::406 => (askNumerator > 0 && askDenominator > 0) ||
  2023-04-rubicon/contracts/utilities/MarketAid.sol::407 => (bidNumerator > 0 && bidDenominator > 0),
  2023-04-rubicon/contracts/utilities/MarketAid.sol::462 => (askNumerator > 0 && askDenominator > 0) &&
  2023-04-rubicon/contracts/utilities/MarketAid.sol::463 => (bidNumerator > 0 && bidDenominator > 0)
  2023-04-rubicon/contracts/utilities/MarketAid.sol::482 => } else if (askNumerator > 0 && askDenominator > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::964 => if (arbProfitIfAny > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1027 => if (stratBook.length > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1049 => if (quoteOnChainBalance > 0 || assetOnChainBalance > 0) {
  2023-04-rubicon/contracts/utilities/MarketAid.sol::1079 => if (stratBook.length > 0) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::412 => if (delta > 0) {
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::454 => if (_delta > 0) {
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::313 => require(_liq > 0, "_maxBorrow: LIQUIDITY == 0");
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::318 => require(_max > 0, "_maxBorrow: can't borrow 0");

 G006:
  2023-04-rubicon/contracts/RubiconMarket.sol::232 => bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
  2023-04-rubicon/contracts/RubiconMarket.sol::302 => ///     keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::369 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::388 => ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::400 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::411 => ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::423 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::442 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::466 => ///     keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::476 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::544 => ///     keccak256(abi.encodePacked(pay_gem, buy_gem)),
  2023-04-rubicon/contracts/RubiconMarket.sol::555 => keccak256(abi.encodePacked(pay_gem, buy_gem)),
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::488 => return (keccak256(abi.encodePacked((a))) == keccak256(abi.encodePacked((b))));
  2023-04-rubicon/contracts/compound-v2-fork/SimplePriceOracle.sol::42 => return (keccak256(abi.encodePacked((a))) == keccak256(abi.encodePacked((b))));
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::65 => bytes32 txHash = keccak256(abi.encode(target, value, signature, data, eta));
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::75 => bytes32 txHash = keccak256(abi.encode(target, value, signature, data, eta));
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::84 => bytes32 txHash = keccak256(abi.encode(target, value, signature, data, eta));
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::96 => callData = abi.encodePacked(bytes4(keccak256(bytes(signature))), data);
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::127 => /// @dev keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::271 => DOMAIN_SEPARATOR = keccak256(
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::273 => keccak256(
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::276 => keccak256(bytes(name)),
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::277 => keccak256(bytes("1")),
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::672 => bytes32 digest = keccak256(
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::676 => keccak256(
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::309 => keccak256(abi.encodePacked(offers[id].pay_gem, offers[id].buy_gem)),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::362 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::373 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::408 => keccak256(abi.encodePacked(_offer.pay_gem, _offer.buy_gem)),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::463 => keccak256(abi.encodePacked(pay_gem, buy_gem)),
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::737 => ///     keccak256(abi.encodePacked(route[0], route[route.length - 1])),
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::746 => keccak256(abi.encodePacked(route[0], route[route.length - 1])),

 G007:
  2023-04-rubicon/contracts/BathHouseV2.sol::4 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  2023-04-rubicon/contracts/BathHouseV2.sol::5 => import "./compound-v2-fork/InterestRateModel.sol";
  2023-04-rubicon/contracts/BathHouseV2.sol::6 => import "./compound-v2-fork/CErc20Delegator.sol";
  2023-04-rubicon/contracts/BathHouseV2.sol::7 => import "./compound-v2-fork/Comptroller.sol";
  2023-04-rubicon/contracts/BathHouseV2.sol::8 => import "./compound-v2-fork/Unitroller.sol";
  2023-04-rubicon/contracts/BathHouseV2.sol::70 => "createBathToken: BATHTOKEN WITH THIS ERC20 EXIST ALDREADY"
  2023-04-rubicon/contracts/BathHouseV2.sol::74 => "createBathToken: UNDERLYING == ADDRESS 0"
  2023-04-rubicon/contracts/BathHouseV2.sol::78 => "createBathToken: IMPLEMENTATION == ADDRESS 0"
  2023-04-rubicon/contracts/RubiconMarket.sol::11 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  2023-04-rubicon/contracts/RubiconMarket.sol::12 => import "@openzeppelin/contracts/utils/StorageSlot.sol";
  2023-04-rubicon/contracts/RubiconMarket.sol::376 => "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
  2023-04-rubicon/contracts/RubiconMarket.sol::381 => "_offer.pay_gem.transfer(msg.sender, quantity) failed"
  2023-04-rubicon/contracts/RubiconMarket.sol::720 => require(isActive(id), "Offer was deleted or taken, or never existed.");
  2023-04-rubicon/contracts/RubiconMarket.sol::726 => "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
  2023-04-rubicon/contracts/V2Migrator.sol::4 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/V2Migrator.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/V2Migrator.sol::6 => import "./compound-v2-fork/CTokenInterfaces.sol";
  2023-04-rubicon/contracts/V2Migrator.sol::65 => "migrate: BATH TOKENS V2 STUCK IN THE CONTRACT"
  2023-04-rubicon/contracts/compound-v2-fork/BaseJumpRateModelV2.sol::68 => require(msg.sender == owner, "only the owner may call this function.");
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::32 => require(msg.sender == admin, "only the admin may initialize the implementation");
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::49 => require(address(dai) == underlying, "DAI must be the same as underlying");
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::74 => require(msg.sender == admin, "only the admin may abandon the implementation");
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::134 => require(token.transferFrom(from, address(this), amount), "unexpected EIP-20 transfer in return");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::126 => require(msg.sender == admin, "CErc20::sweepToken: only admin can sweep tokens");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::127 => require(address(token) != underlying, "CErc20::sweepToken: can not sweep underlying token");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::226 => require(msg.sender == admin, "only the admin may set the comp-like delegate");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegate.sol::30 => require(msg.sender == admin, "only the admin may call _becomeImplementation");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegate.sol::42 => require(msg.sender == admin, "only the admin may call _resignImplementation");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::39 => delegateTo(implementation_, abi.encodeWithSignature("initialize(address,address,address,uint256,string,string,uint8)",
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::62 => require(msg.sender == admin, "CErc20Delegator::_setImplementation: Caller must be admin");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::136 => bytes memory data = delegateToImplementation(abi.encodeWithSignature("repayBorrowBehalf(address,uint256)", borrower, repayAmount));
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::149 => bytes memory data = delegateToImplementation(abi.encodeWithSignature("liquidateBorrow(address,uint256,address)", borrower, repayAmount, cTokenCollateral));
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::172 => bytes memory data = delegateToImplementation(abi.encodeWithSignature("transferFrom(address,address,uint256)", src, dst, amount));
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::463 => require(msg.value == 0,"CErc20Delegator:fallback: cannot send value to fallback");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::32 => require(msg.sender == admin, "only admin may initialize the market");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::33 => require(accrualBlockNumber == 0 && borrowIndex == 0, "market may only be initialized once");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::37 => require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::49 => require(err == NO_ERROR, "setting interest rate model failed");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::481 => require(redeemTokensIn == 0 || redeemAmountIn == 0, "one of redeemTokensIn or redeemAmountIn must be zero");
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::762 => require(amountSeizeError == NO_ERROR, "LIQUIDATE_COMPTROLLER_CALCULATE_AMOUNT_SEIZE_FAILED");
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::10 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::234 => require(oErr == 0, "exitMarket: getAccountSnapshot failed"); // semi-opaque error code
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::609 => "Can not repay more than the total borrow"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1278 => "only admin or borrow cap guardian can set borrow caps"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1300 => require(msg.sender == admin, "only admin can set borrow cap guardian");
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1341 => "cannot pause a market that is not listed"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1345 => "only pause guardian and admin can pause"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1357 => "cannot pause a market that is not listed"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1361 => "only pause guardian and admin can pause"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1373 => "only pause guardian and admin can pause"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1385 => "only pause guardian and admin can pause"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1397 => "only unitroller admin can change brains"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1410 => require(msg.sender == admin, "Only admin can call this function"); // Only the timelock can call this function
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1413 => "Already executed this one-off function"
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1804 => "Comptroller::_setCompSpeeds invalid input"
  2023-04-rubicon/contracts/compound-v2-fork/DAIInterestRateModelV3.sol::55 => require(msg.sender == owner, "only the owner may call this function.");
  2023-04-rubicon/contracts/compound-v2-fork/SafeMath.sol::98 => require(c / a == b, "SafeMath: multiplication overflow");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::28 => require(delay_ >= MINIMUM_DELAY, "Timelock::constructor: Delay must exceed minimum delay.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::29 => require(delay_ <= MAXIMUM_DELAY, "Timelock::setDelay: Delay must not exceed maximum delay.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::38 => require(msg.sender == address(this), "Timelock::setDelay: Call must come from Timelock.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::39 => require(delay_ >= MINIMUM_DELAY, "Timelock::setDelay: Delay must exceed minimum delay.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::40 => require(delay_ <= MAXIMUM_DELAY, "Timelock::setDelay: Delay must not exceed maximum delay.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::47 => require(msg.sender == pendingAdmin, "Timelock::acceptAdmin: Call must come from pendingAdmin.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::55 => require(msg.sender == address(this), "Timelock::setPendingAdmin: Call must come from Timelock.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::62 => require(msg.sender == admin, "Timelock::queueTransaction: Call must come from admin.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::63 => require(eta >= getBlockTimestamp().add(delay), "Timelock::queueTransaction: Estimated execution block must satisfy delay.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::73 => require(msg.sender == admin, "Timelock::cancelTransaction: Call must come from admin.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::82 => require(msg.sender == admin, "Timelock::executeTransaction: Call must come from admin.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::85 => require(queuedTransactions[txHash], "Timelock::executeTransaction: Transaction hasn't been queued.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::86 => require(getBlockTimestamp() >= eta, "Timelock::executeTransaction: Transaction hasn't surpassed time lock.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::87 => require(getBlockTimestamp() <= eta.add(GRACE_PERIOD), "Timelock::executeTransaction: Transaction is stale.");
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::101 => require(success, "Timelock::executeTransaction: Transaction execution reverted.");
  2023-04-rubicon/contracts/interfaces/IBathBuddy.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/interfaces/IBathToken.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/interfaces/IRubiconMarket.sol::5 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/interfaces/ISwapRouter.sol::5 => // import "@uniswap/v3-core/contracts/interfaces/callback/IUniswapV3SwapCallback.sol";
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::4 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::5 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::6 => import "@openzeppelin/contracts/utils/math/SafeMath.sol";
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::7 => import "@openzeppelin/contracts/security/Pausable.sol";
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::105 => require(msg.sender == bathHouse, "You are not my beloved bath house!");
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::122 => require(friendshipStarted, "I have not started a bathToken friendship");
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::143 => require(friendshipStarted, "I have not started a bathToken friendship");
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::238 => "Previous rewards period must be complete before changing the duration for the new period"
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::10 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::11 => import "@openzeppelin/contracts/utils/math/SafeMath.sol";
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::12 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::127 => /// @dev keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::274 => "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::308 => "caller is not bathHouse - BathToken.sol"
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::413 => require(_shares == shares, "did not mint expected share count");
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::450 => "This implementation does not support non-sender owners from withdrawing user shares"
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::456 => "You cannot withdraw the amount of assets you expected"
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::485 => "This implementation does not support non-sender owners from withdrawing user shares"
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::582 => // require(msg.sender == receiver, "You cannot claim someone else`s bonus tokens");
  2023-04-rubicon/contracts/periphery/DummyPriceOracle.sol::3 => import "../compound-v2-fork/PriceOracle.sol";
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::9 => //import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::47 => require(from != address(0), "ERC20: transfer from the zero address");
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::48 => require(to != address(0), "ERC20: transfer to the zero address");
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::53 => "ERC20: transfer amount exceeds balance"
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::79 => require(owner != address(0), "ERC20: approve from the zero address");
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::80 => require(spender != address(0), "ERC20: approve to the zero address");
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::352 => "_offer.buy_gem.transferFrom(msg.sender, _offer.owner, spend) failed - check that you can pay the fee"
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::356 => "_offer.pay_gem.transfer(msg.sender, quantity) failed"
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::615 => require(isActive(id), "Offer was deleted or taken, or never existed.");
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::618 => "Offer can not be cancelled because user is not owner, and market is open, and offer sells required amount of tokens."
  2023-04-rubicon/contracts/periphery/Test3rdPartyProtocol.sol::4 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/periphery/TokenWithFaucet.sol::7 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  2023-04-rubicon/contracts/periphery/WETH9.sol::390 => A "User Product" is either (1) a "consumer product", which means any
  2023-04-rubicon/contracts/proxy/RubiconProxy.sol::5 => import "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::4 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::70 => require(_OK, "low-level call to the Rubicon failed");
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::86 => require(_OK, "low-level call to the router failed");
  2023-04-rubicon/contracts/utilities/MarketAid.sol::10 => import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
  2023-04-rubicon/contracts/utilities/MarketAid.sol::11 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/utilities/MarketAid.sol::12 => import "@openzeppelin/contracts/utils/math/SafeMath.sol";
  2023-04-rubicon/contracts/utilities/MarketAid.sol::13 => import "@openzeppelin/contracts/utils/Multicall.sol";
  2023-04-rubicon/contracts/utilities/MarketAid.sol::175 => "you are not an approved strategist"
  2023-04-rubicon/contracts/utilities/MarketAid.sol::223 => "you are not the kso or not assigned"
  2023-04-rubicon/contracts/utilities/MarketAid.sol::353 => require(assigned, "Didnt Find that element in live list, cannot scrub");
  2023-04-rubicon/contracts/utilities/MarketAid.sol::383 => "The trades must not match with self"
  2023-04-rubicon/contracts/utilities/MarketAid.sol::440 => "Not enough ERC20s to market make this call"
  2023-04-rubicon/contracts/utilities/MarketAid.sol::738 => "you are not the strategist that made this order"
  2023-04-rubicon/contracts/utilities/MarketAidFactory.sol::6 => import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::8 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::9 => import "@openzeppelin/contracts/utils/math/SafeMath.sol";
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::10 => import "../compound-v2-fork/CTokenInterfaces.sol";
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::291 => require(assigned, "Didnt Find that element in live list, cannot scrub");
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::388 => "must send as much ETH as max_fill_amount"
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::474 => "must send enough native ETH to pay as weth and account for fee"
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::513 => "didnt send enough native ETH for WETH offer"
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::601 => "You did not provide an Id for an offer you own"
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::609 => "trying to cancel a non WETH order"
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::4 => import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::5 => import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::6 => import "@openzeppelin/contracts/utils/math/SafeMath.sol";
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::7 => import "@openzeppelin/contracts/access/Ownable.sol";
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::8 => import "../../compound-v2-fork/Comptroller.sol";
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::9 => import "../../compound-v2-fork/PriceOracle.sol";
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::76 => require(pos.isActive, "borrowBalanceOfPos: POS ISN'T ACTIVE");
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::102 => "buyAllAmountWithLeverage: FAILED TO OPEN POSITION"
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::116 => "sellAllAmountWithLeverage: FAILED TO OPEN POSITION"
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::290 => "_repay: balance of quote lt. debt"
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::593 => "_leverageCheck{Long}: INVLAID LEVERAGE"
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::597 => "_leverageCheck{Short}: INVLAID LEVERAGE"

 G008:
  2023-04-rubicon/contracts/RubiconMarket.sol::75 => uint256 constant RAY = 10 ** 27;
  2023-04-rubicon/contracts/RubiconMarket.sol::78 => z = add(mul(x, y), WAD / 2) / WAD;
  2023-04-rubicon/contracts/RubiconMarket.sol::82 => z = add(mul(x, y), RAY / 2) / RAY;
  2023-04-rubicon/contracts/RubiconMarket.sol::86 => z = add(mul(x, WAD), y / 2) / y;
  2023-04-rubicon/contracts/RubiconMarket.sol::90 => z = add(mul(x, RAY), y / 2) / y;
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::174 => uint256 constant RAY = 10 ** 27;
  2023-04-rubicon/contracts/compound-v2-fork/CTokenInterfaces.sol::111 => uint public constant protocolSeizeShareMantissa = 2.8e16; //2.8%
  2023-04-rubicon/contracts/compound-v2-fork/ExponentialNoError.sol::14 => uint constant halfExpScale = expScale/2;
  2023-04-rubicon/contracts/compound-v2-fork/ExponentialNoError.sol::79 => require(n < 2**224, errorMessage);
  2023-04-rubicon/contracts/interfaces/IBathToken.sol::28 => function asset() external view returns (address assetTokenAddress); //4626
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::288 => IERC20(address(token)).approve(RubiconMarketAddress, 2 ** 256 - 1);
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::364 => maxAssets = 2 ** 256 - 1; // No limit on deposits in current implementation  = Max UINT
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::394 => maxShares = 2 ** 256 - 1; // No limit on shares that could be created via deposit in current implementation - Max UINT
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::149 => uint256 constant RAY = 10 ** 27;
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::152 => z = add(mul(x, y), WAD / 2) / WAD;
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::156 => z = add(mul(x, y), RAY / 2) / RAY;
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::160 => z = add(mul(x, WAD), y / 2) / y;
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::164 => z = add(mul(x, RAY), y / 2) / y;
  2023-04-rubicon/contracts/periphery/TokenWithFaucet.sol::3 => // hevm: flattened sources of /nix/store/8xb41r4qd0cjb63wcrxf1qmfg88p0961-dss-6fd7de0/src/dai.sol
  2023-04-rubicon/contracts/utilities/MarketAid.sol::366 => IERC20(toApprove).safeApprove(RubiconMarketAddress, 2 ** 256 - 1);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::617 => // 2. Place another

 L001:
  2023-04-rubicon/contracts/RubiconMarket.sol::340 => _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
  2023-04-rubicon/contracts/RubiconMarket.sol::351 => _offer.buy_gem.transferFrom(
  2023-04-rubicon/contracts/RubiconMarket.sol::360 => _offer.buy_gem.transferFrom(msg.sender, _offer.owner, mFee),
  2023-04-rubicon/contracts/RubiconMarket.sol::375 => _offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend),
  2023-04-rubicon/contracts/RubiconMarket.sol::376 => "_offer.buy_gem.transferFrom(msg.sender, _offer.recipient, spend) failed - check that you can pay the fee"
  2023-04-rubicon/contracts/RubiconMarket.sol::380 => _offer.pay_gem.transfer(msg.sender, quantity),
  2023-04-rubicon/contracts/RubiconMarket.sol::381 => "_offer.pay_gem.transfer(msg.sender, quantity) failed"
  2023-04-rubicon/contracts/RubiconMarket.sol::460 => ? require(_offer.pay_gem.transfer(_offer.recipient, _offer.pay_amt))
  2023-04-rubicon/contracts/RubiconMarket.sol::461 => : require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
  2023-04-rubicon/contracts/RubiconMarket.sol::538 => require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
  2023-04-rubicon/contracts/V2Migrator.sol::44 => bathTokenV1.transferFrom(msg.sender, address(this), bathBalance);
  2023-04-rubicon/contracts/V2Migrator.sol::53 => underlying.approve(bathTokenV2, amountWithdrawn);
  2023-04-rubicon/contracts/V2Migrator.sol::59 => IERC20(bathTokenV2).transfer(
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::57 => dai.approve(daiJoinAddress, type(uint).max);
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::134 => require(token.transferFrom(from, address(this), amount), "unexpected EIP-20 transfer in return");
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::129 => token.transfer(admin, balance);
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::167 => token.transferFrom(from, address(this), amount);
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::201 => token.transfer(to, amount);
  2023-04-rubicon/contracts/compound-v2-fork/CEther.sol::150 => to.transfer(amount);
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::1766 => comp.transfer(user, amount);
  2023-04-rubicon/contracts/compound-v2-fork/Maximillion.sol::43 => payable(msg.sender).transfer(received - borrows);
  2023-04-rubicon/contracts/compound-v2-fork/Reservoir.sol::64 => token_.transfer(target_, toDrip_);
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::288 => IERC20(address(token)).approve(RubiconMarketAddress, 2 ** 256 - 1);
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::344 => _offer.buy_gem.transferFrom(msg.sender, feeTo, fee),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::351 => _offer.buy_gem.transferFrom(msg.sender, _offer.owner, spend),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::352 => "_offer.buy_gem.transferFrom(msg.sender, _offer.owner, spend) failed - check that you can pay the fee"
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::355 => _offer.pay_gem.transfer(msg.sender, quantity),
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::356 => "_offer.pay_gem.transfer(msg.sender, quantity) failed"
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::403 => require(_offer.pay_gem.transfer(_offer.owner, _offer.pay_amt));
  2023-04-rubicon/contracts/periphery/RubiconMarketV1.sol::458 => require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
  2023-04-rubicon/contracts/periphery/Test3rdPartyProtocol.sol::27 => IERC20(route[0]).transferFrom(msg.sender, address(this), pay_amt);
  2023-04-rubicon/contracts/periphery/Test3rdPartyProtocol.sol::46 => IERC20(route[0]).approve(feeWrapper, pay_amt);
  2023-04-rubicon/contracts/periphery/Test3rdPartyProtocol.sol::85 => IERC20(route[0]).approve(feeWrapper, pay_amt);
  2023-04-rubicon/contracts/periphery/WETH9.sol::51 => payable(msg.sender).transfer(wad);
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::100 => IERC20(_feeToken).transferFrom(msg.sender, address(this), _totalAmount);
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::102 => IERC20(_feeToken).transfer(_feeTo, _feeAmount);
  2023-04-rubicon/contracts/utilities/FeeWrapper.sol::105 => IERC20(_feeToken).approve(_target, (_totalAmount - _feeAmount));
  2023-04-rubicon/contracts/utilities/MarketAid.sol::775 => IERC20(token).approve(target, type(uint256).max);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::783 => IERC20(erc20s[i]).transfer(_admin, amount);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::798 => IERC20(erc20s[i]).transferFrom(msg.sender, address(this), amount);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::817 => IERC20(erc20s[i]).transfer(_admin, amount);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::842 => IERC20(assetToSell).approve(address(swapRouter), amountToSell);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::885 => // IERC20(assetToSell).approve(_RubiconMarketAddress, amountToSell);
  2023-04-rubicon/contracts/utilities/MarketAid.sol::924 => // IERC20(assetToTarget).approve(_RubiconMarketAddress, fill_amt); // Add approve function for the asset being sold
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::355 => ERC20(buy_gem).transfer(msg.sender, fill);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::374 => ERC20(buy_gem).transfer(msg.sender, fill);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::414 => // msg.sender.transfer(delta);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::458 => // msg.sender.transfer(buy_amt); // Return native ETH
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::495 => // msg.sender.transfer(fill);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::582 => // msg.sender.transfer(delta);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::614 => // msg.sender.transfer(pay_amt);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::636 => IERC20(wethAddress).approve(bathToken, amount);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::660 => IERC20(bathToken).transferFrom(msg.sender, address(this), shares);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::673 => // msg.sender.transfer(withdrawnWETH);
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::297 => IERC20(_quote).approve(_bathTokenQuote, _amountToRepay);
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::465 => IERC20(_quote).approve(address(rubiconMarket), _maxFill);
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::493 => IERC20(_asset).transferFrom(
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::500 => IERC20(_asset).approve(

 L003:
  2023-04-rubicon/contracts/RubiconMarket.sol::2 => pragma solidity ^0.8.9;
  2023-04-rubicon/contracts/compound-v2-fork/BaseJumpRateModelV2.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CDaiDelegate.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CErc20.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegate.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Delegator.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CErc20Immutable.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CEther.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CToken.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/CTokenInterfaces.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/Comptroller.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/ComptrollerInterface.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/ComptrollerStorage.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/DAIInterestRateModelV3.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/EIP20Interface.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/EIP20NonStandardInterface.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/ErrorReporter.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/ExponentialNoError.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/InterestRateModel.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/JumpRateModel.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/JumpRateModelV2.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/Lens/CompoundLens.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/Maximillion.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/PriceOracle.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/Reservoir.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/SafeMath.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/SimplePriceOracle.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/Timelock.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/Unitroller.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/compound-v2-fork/WhitePaperInterestRateModel.sol::2 => pragma solidity ^0.8.10;
  2023-04-rubicon/contracts/interfaces/IBathBuddy.sol::3 => pragma solidity >=0.7.6;
  2023-04-rubicon/contracts/interfaces/IBathToken.sol::3 => pragma solidity >=0.7.6;
  2023-04-rubicon/contracts/interfaces/IRubiconMarket.sol::3 => pragma solidity >=0.8.16;
  2023-04-rubicon/contracts/interfaces/ISwapRouter.sol::2 => pragma solidity >=0.8.16;
  2023-04-rubicon/contracts/interfaces/IWETH.sol::3 => pragma solidity >=0.8.17;
  2023-04-rubicon/contracts/periphery/BathBuddy.sol::2 => pragma solidity ^0.8.0;
  2023-04-rubicon/contracts/periphery/BathTokenV1.sol::8 => pragma solidity ^0.8.0;
  2023-04-rubicon/contracts/periphery/TokenWithFaucet.sol::4 => pragma solidity ^0.8.6;
  2023-04-rubicon/contracts/proxy/RubiconProxy.sol::3 => pragma solidity >=0.8.17;
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::5 => pragma solidity ^0.8.17;

 L005:
  2023-04-rubicon/contracts/utilities/MarketAid.sol::366 => IERC20(toApprove).safeApprove(RubiconMarketAddress, 2 ** 256 - 1);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::632 => IERC20(target).safeApprove(bathToken, amount);
  2023-04-rubicon/contracts/utilities/RubiconRouter.sol::689 => IERC20(toApprove).safeApprove(RubiconMarketAddress, type(uint256).max);
  2023-04-rubicon/contracts/utilities/poolsUtility/Position.sol::358 => IERC20(_token).safeApprove(_bathToken, _amount);

