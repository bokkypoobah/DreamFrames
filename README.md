# DreamFrameTokens

**Status: Work in progress**

## Table Of Contents

## Contracts

### contracts
* [ ] [Migrations.sol](contracts/Migrations.sol)

### contracts/CollectableFrames
* [ ] [DreamFramesCollectable.sol](contracts/CollectableFrames/DreamFramesCollectable.sol)
* [ ] [ERC721.sol](contracts/CollectableFrames/ERC721.sol)
* [ ] [ERC721Basic.sol](contracts/CollectableFrames/ERC721Basic.sol)
* [ ] [ERC721BasicToken.sol](contracts/CollectableFrames/ERC721BasicToken.sol)
* [ ] [ERC721Holder.sol](contracts/CollectableFrames/ERC721Holder.sol)
* [ ] [ERC721Receiver.sol](contracts/CollectableFrames/ERC721Receiver.sol)

### contracts/DreamFramesToken
* [ ] [BonusList.sol](contracts/DreamFramesToken/BonusList.sol)
* [ ] [BonusListInterface.sol](contracts/DreamFramesToken/BonusListInterface.sol)
* [ ] [DreamFramesCrowdsale.sol](contracts/DreamFramesToken/DreamFramesCrowdsale.sol)
* [ ] [DreamFramesToken.sol](contracts/DreamFramesToken/DreamFramesToken.sol)
* [ ] [MakerDAOETHUSDPriceFeedSimulator.sol](contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol)
* [ ] [MakerDAOPriceFeedAdaptor.sol](contracts/DreamFramesToken/MakerDAOPriceFeedAdaptor.sol)
* [ ] [PriceFeedInterface.sol](contracts/DreamFramesToken/PriceFeedInterface.sol)

### contracts/RoyaltyToken
* [ ] [DividendToken.sol](contracts/RoyaltyToken/DividendToken.sol)
* [ ] [DreamFrameSecurityToken.sol](contracts/RoyaltyToken/DreamFrameSecurityToken.sol)
* [ ] [RoyaltyTokenCrowdsale.sol](contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol)
* [ ] [WhiteList.sol](contracts/RoyaltyToken/WhiteList.sol)
* [ ] [WhiteListInterface.sol](contracts/RoyaltyToken/WhiteListInterface.sol)

### contracts/Shared
* [ ] [AddressUtils.sol](contracts/Shared/AddressUtils.sol)
* [ ] [BTTSTokenInterface120.sol](contracts/Shared/BTTSTokenInterface120.sol)
* [ ] [BTTSTokenLibrary120.sol](contracts/Shared/BTTSTokenLibrary120.sol)
* [ ] [CanSendCodes.sol](contracts/Shared/CanSendCodes.sol)
* [ ] [ERC165.sol](contracts/Shared/ERC165.sol)
* [ ] [ERC20Interface.sol](contracts/Shared/ERC20Interface.sol)
* [ ] [IERC1594.sol](contracts/Shared/IERC1594.sol)
* [ ] [Operated.sol](contracts/Shared/Operated.sol)
* [ ] [Owned.sol](contracts/Shared/Owned.sol)
  * [ ] Check all derived contracts call the `initOwned(...)` function
    * [ ] [contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol](contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol)
    * [ ] [contracts/DreamFramesToken/DreamFramesCrowdsale.sol](contracts/DreamFramesToken/DreamFramesCrowdsale.sol)
    * [ ] [contracts/Shared/Operated.sol](contracts/Shared/Operated.sol)
    * [ ] [contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol](contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol)
    * [ ] [contracts/Shared/Operated.sol](contracts/Shared/Operated.sol)
      * [ ] [contracts/DreamFramesToken/BonusList.sol](contracts/DreamFramesToken/BonusList.sol)
      * [ ] [contracts/RoyaltyToken/WhiteList.sol](contracts/RoyaltyToken/WhiteList.sol)
* [ ] [SafeMath.sol](contracts/Shared/SafeMath.sol)
* [ ] [SupportsInterfaceWithLookup.sol](contracts/Shared/SupportsInterfaceWithLookup.sol)
