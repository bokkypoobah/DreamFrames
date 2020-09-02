你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
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

* [ ] [DreamFramesCollectable.md](boksNotes/CollectableFrames/DreamFramesCollectable.md)
  * [ ] contract DreamFramesCollectable is SupportsInterfaceWithLookup, ERC721BasicToken, ERC721
* [ ] [ERC721.md](boksNotes/CollectableFrames/ERC721.md)
  * [ ] contract ERC721Enumerable is ERC721Basic
  * [ ] contract ERC721Metadata is ERC721Basic
  * [ ] contract ERC721 is ERC721Basic, ERC721Enumerable, ERC721Metadata
* [ ] [ERC721Basic.md](boksNotes/CollectableFrames/ERC721Basic.md)
  * [ ] contract ERC721Basic is ERC165
* [ ] [ERC721BasicToken.md](boksNotes/CollectableFrames/ERC721BasicToken.md)
  * [ ] contract ERC721BasicToken is SupportsInterfaceWithLookup, ERC721Basic
  * [ ]   using SafeMath for uint256;
  * [ ]   using AddressUtils for address;
* [ ] [ERC721Holder.md](boksNotes/CollectableFrames/ERC721Holder.md)
  * [ ] contract ERC721Holder is ERC721Receiver
* [ ] [ERC721Receiver.md](boksNotes/CollectableFrames/ERC721Receiver.md)
  * [ ] contract ERC721Receiver

### contracts/DreamFramesToken
* [ ] [BonusList.md](boksNotes/DreamFramesToken/BonusList.md)
  * [ ] contract BonusList is BonusListInterface, Operated
* [ ] [BonusListInterface.md](boksNotes/DreamFramesToken/BonusListInterface.md)
  * [ ] contract BonusListInterface
* [ ] [DreamFramesCrowdsale.md](boksNotes/DreamFramesToken/DreamFramesCrowdsale.md)
  * [ ] contract DreamFramesCrowdsale is Owned
  * [ ]   using SafeMath for uint256;
* [ ] [DreamFramesToken.md](boksNotes/DreamFramesToken/DreamFramesToken.md)
  * [ ] contract DreamFramesToken is BTTSTokenInterface
  * [ ]     using BTTSLib for BTTSLib.Data;
* [ ] [MakerDAOETHUSDPriceFeedSimulator.md](boksNotes/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.md)
  * [ ] contract MakerDAOETHUSDPriceFeedSimulator is Owned
* [ ] [MakerDAOPriceFeedAdaptor.md](boksNotes/DreamFramesToken/MakerDAOPriceFeedAdaptor.md)
  * [ ] contract MakerDAOPriceFeedInterface
  * [ ] contract MakerDAOPriceFeedAdaptor is PriceFeedInterface
* [ ] [PriceFeedInterface.md](boksNotes/DreamFramesToken/PriceFeedInterface.md)
  * [ ] contract PriceFeedInterface

### contracts/RoyaltyToken
* [ ] [DividendToken.md](boksNotes/RoyaltyToken/DividendToken.md)
  * [ ] contract DividendToken is BTTSTokenInterface
  * [ ]     using BTTSLib for BTTSLib.Data;
  * [ ]     using BTTSLib for uint256;
* [ ] [DreamFrameSecurityToken.md](boksNotes/RoyaltyToken/DreamFrameSecurityToken.md)
  * [ ] contract DreamFrameSecurityToken is DividendToken, CanSendCodes, IERC1594
* [ ] [RoyaltyTokenCrowdsale.md](boksNotes/RoyaltyToken/RoyaltyTokenCrowdsale.md)
  * [ ] contract DreamFramesCSInterface
  * [ ] contract DreamFramesRoyaltyCrowdsale is Owned
  * [ ]     using SafeMath for uint256;
* [ ] [WhiteList.md](boksNotes/RoyaltyToken/WhiteList.md)
  * [ ] contract WhiteList is WhiteListInterface, Operated
* [ ] [WhiteListInterface.md](boksNotes/RoyaltyToken/WhiteListInterface.md)
  * [ ] contract WhiteListInterface

### contracts/Shared
* [ ] [AddressUtils.md](boksNotes/Shared/AddressUtils.md)
  * [ ] library AddressUtils
* [ ] [BTTSTokenInterface120.md](boksNotes/Shared/BTTSTokenInterface120.md)
  * [ ] contract ApproveAndCallFallBack
  * [ ] contract BTTSTokenInterface is ERC20Interface
* [ ] [BTTSTokenLibrary120.md](boksNotes/Shared/BTTSTokenLibrary120.md)
  * [ ] library BTTSLib
* [ ] [CanSendCodes.md](boksNotes/Shared/CanSendCodes.md)
  * [ ] contract CanSendCodes
* [ ] [ERC165.md](boksNotes/Shared/ERC165.md)
  * [ ] interface ERC165
* [ ] [ERC20Interface.md](boksNotes/Shared/ERC20Interface.md)
  * [ ] contract ERC20Interface
* [ ] [IERC1594.md](boksNotes/Shared/IERC1594.md)
  * [ ] interface IERC1594 is ERC20Interface
* [ ] [Operated.md](boksNotes/Shared/Operated.md)
  * [ ] contract Operated is Owned
* [ ] [Owned.md](boksNotes/Shared/Owned.md)
  * [ ] contract Owned
  * [ ] Check all derived contracts call the `initOwned(...)` function
    * [ ] [contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol](contracts/DreamFramesToken/MakerDAOETHUSDPriceFeedSimulator.sol)
    * [ ] [contracts/DreamFramesToken/DreamFramesCrowdsale.sol](contracts/DreamFramesToken/DreamFramesCrowdsale.sol)
    * [ ] [contracts/Shared/Operated.sol](contracts/Shared/Operated.sol)
    * [ ] [contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol](contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol)
    * [ ] [contracts/Shared/Operated.sol](contracts/Shared/Operated.sol)
      * [ ] [contracts/DreamFramesToken/BonusList.sol](contracts/DreamFramesToken/BonusList.sol)
      * [ ] [contracts/RoyaltyToken/WhiteList.sol](contracts/RoyaltyToken/WhiteList.sol)
* [ ] [SafeMath.md](boksNotes/Shared/SafeMath.md)
  * [ ] library SafeMath
* [ ] [SupportsInterfaceWithLookup.md](boksNotes/Shared/SupportsInterfaceWithLookup.md)
  * [ ] contract SupportsInterfaceWithLookup is ERC165

<br />

### Excluded

* [ ] [Migrations.sol](contracts/Migrations.sol)
