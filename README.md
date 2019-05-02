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


<br>

<hr />

## Notes

### contracts/CollectableFrames

* [ ] [DreamFramesCollectable.md](notes/CollectableFrames/DreamFramesCollectable.md)
  * [ ] contract DreamFramesCollectable is SupportsInterfaceWithLookup, ERC721BasicToken, ERC721
* [ ] [ERC721.md](notes/CollectableFrames/ERC721.md)
  * [ ] contract ERC721Enumerable is ERC721Basic
  * [ ] contract ERC721Metadata is ERC721Basic
  * [ ] contract ERC721 is ERC721Basic, ERC721Enumerable, ERC721Metadata
* [ ] [ERC721Basic.md](notes/CollectableFrames/ERC721Basic.md)
  * [ ] contract ERC721Basic is ERC165
* [ ] [ERC721BasicToken.md](notes/CollectableFrames/ERC721BasicToken.md)
  * [ ] contract ERC721BasicToken is SupportsInterfaceWithLookup, ERC721Basic
  * [ ]   using SafeMath for uint256;
  * [ ]   using AddressUtils for address;
* [ ] [ERC721Holder.md](notes/CollectableFrames/ERC721Holder.md)
  * [ ] contract ERC721Holder is ERC721Receiver
* [ ] [ERC721Receiver.md](notes/CollectableFrames/ERC721Receiver.md)
  * [ ] contract ERC721Receiver

### contracts/DreamFramesToken
* [ ] [BonusList.md](notes/DreamFramesToken/BonusList.md)
  * [ ] contract BonusList is BonusListInterface, Operated
* [ ] [BonusListInterface.md](notes/DreamFramesToken/BonusListInterface.md)
  * [ ] contract BonusListInterface
* [ ] [DreamFramesCrowdsale.md](notes/DreamFramesToken/DreamFramesCrowdsale.md)
  * [ ] contract DreamFramesCrowdsale is Owned
  * [ ]   using SafeMath for uint256;
* [ ] [DreamFramesToken.md](notes/DreamFramesToken/DreamFramesToken.md)
  * [ ] contract DreamFramesToken is BTTSTokenInterface
  * [ ]     using BTTSLib for BTTSLib.Data;
* [ ] [MakerDAOETHUSDPriceFeedSimulator.md](notes/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.md)
  * [ ] contract MakerDAOETHUSDPriceFeedSimulator is Owned
* [ ] [MakerDAOPriceFeedAdaptor.md](notes/DreamFramesToken/MakerDAOPriceFeedAdaptor.md)
  * [ ] contract MakerDAOPriceFeedInterface
  * [ ] contract MakerDAOPriceFeedAdaptor is PriceFeedInterface
* [ ] [PriceFeedInterface.md](notes/DreamFramesToken/PriceFeedInterface.md)
  * [ ] contract PriceFeedInterface

### contracts/RoyaltyToken
* [ ] [DividendToken.md](notes/RoyaltyToken/DividendToken.md)
  * [ ] contract DividendToken is BTTSTokenInterface
  * [ ]     using BTTSLib for BTTSLib.Data;
  * [ ]     using BTTSLib for uint256;
* [ ] [DreamFrameSecurityToken.md](notes/RoyaltyToken/DreamFrameSecurityToken.md)
  * [ ] contract DreamFrameSecurityToken is DividendToken, CanSendCodes, IERC1594
* [ ] [RoyaltyTokenCrowdsale.md](notes/RoyaltyToken/RoyaltyTokenCrowdsale.md)
  * [ ] contract DreamFramesCSInterface
  * [ ] contract DreamFramesRoyaltyCrowdsale is Owned
  * [ ]     using SafeMath for uint256;
* [ ] [WhiteList.md](notes/RoyaltyToken/WhiteList.md)
  * [ ] contract WhiteList is WhiteListInterface, Operated
* [ ] [WhiteListInterface.md](notes/RoyaltyToken/WhiteListInterface.md)
  * [ ] contract WhiteListInterface

### contracts/Shared
* [ ] [AddressUtils.md](notes/Shared/AddressUtils.md)
  * [ ] library AddressUtils
* [ ] [BTTSTokenInterface120.md](notes/Shared/BTTSTokenInterface120.md)
  * [ ] contract ApproveAndCallFallBack
  * [ ] contract BTTSTokenInterface is ERC20Interface
* [ ] [BTTSTokenLibrary120.md](notes/Shared/BTTSTokenLibrary120.md)
  * [ ] library BTTSLib
* [ ] [CanSendCodes.md](notes/Shared/CanSendCodes.md)
  * [ ] contract CanSendCodes
* [ ] [ERC165.md](notes/Shared/ERC165.md)
  * [ ] interface ERC165
* [ ] [ERC20Interface.md](notes/Shared/ERC20Interface.md)
  * [ ] contract ERC20Interface
* [ ] [IERC1594.md](notes/Shared/IERC1594.md)
  * [ ] interface IERC1594 is ERC20Interface
* [ ] [Operated.md](notes/Shared/Operated.md)
  * [ ] contract Operated is Owned
* [ ] [Owned.md](notes/Shared/Owned.md)
  * [ ] contract Owned
  * [ ] Check all derived contracts call the `initOwned(...)` function
    * [ ] [contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol](contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol)
    * [ ] [contracts/DreamFramesToken/DreamFramesCrowdsale.sol](contracts/DreamFramesToken/DreamFramesCrowdsale.sol)
    * [ ] [contracts/Shared/Operated.sol](contracts/Shared/Operated.sol)
    * [ ] [contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol](contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol)
    * [ ] [contracts/Shared/Operated.sol](contracts/Shared/Operated.sol)
      * [ ] [contracts/DreamFramesToken/BonusList.sol](contracts/DreamFramesToken/BonusList.sol)
      * [ ] [contracts/RoyaltyToken/WhiteList.sol](contracts/RoyaltyToken/WhiteList.sol)
* [ ] [SafeMath.md](notes/Shared/SafeMath.md)
  * [ ] library SafeMath
* [ ] [SupportsInterfaceWithLookup.md](notes/Shared/SupportsInterfaceWithLookup.md)
  * [ ] contract SupportsInterfaceWithLookup is ERC165

<br />

### Excluded

* [ ] [Migrations.sol](contracts/Migrations.sol)
