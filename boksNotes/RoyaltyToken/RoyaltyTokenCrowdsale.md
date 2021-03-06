# RoyaltyTokenCrowdsale

Source file [../../contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol](../../contracts/RoyaltyToken/RoyaltyTokenCrowdsale.sol).

<br />

<hr />

```solidity
pragma solidity ^0.5.4;

// ----------------------------------------------------------------------------
// DreamFrames Crowdsale Contract - Purchase royalty tokens and frames with ETH
//
// Deployed to : {TBA}
//
// Enjoy.
//
// (c) BokkyPooBah / Bok Consulting Pty Ltd for GazeCoin 2018. The MIT Licence.
// (c) Adrian Guerrera / Deepyr Pty Ltd for Dreamframes 2019. The MIT Licence.
// ----------------------------------------------------------------------------

import "../Shared/Owned.sol";
import "../Shared/SafeMath.sol";


// ----------------------------------------------------------------------------
// DreamFramesToken Contract
// ----------------------------------------------------------------------------
contract DreamFramesCSInterface {
    function claimRoyaltyFrames(address tokenOwner, uint256 frames, uint256 ethToTransfer) external;
    function frameEth() external view returns (uint _rate, bool _live);
    function calculateFrames(uint256 ethAmount) external view returns (uint256 frames, uint256 ethToTransfer);
}

contract DreamFramesRoyaltyCrowdsale is Owned {

    using SafeMath for uint256;

    uint256 public contributedEth;
    DreamFramesCSInterface public crowdsaleContract;
    address payable public wallet;
    mapping(address => uint256) public royaltyEthAmount;
    uint256 public royaltiesSold;

    event Purchased(address indexed addr, uint256 frames, uint256 ethToTransfer, uint256 royaltiesSold, uint256 contributedEth);
    event SetFrameCrowdsale(address oldCrowdsaleContract, address newCrowdsaleContract);
    event WalletUpdated(address indexed oldWallet, address indexed newWallet);

    constructor(address payable _wallet, address _crowdsaleContract) public {
      require(_wallet != address(0));
      require(_crowdsaleContract != address(0));
      wallet = _wallet;
      crowdsaleContract = DreamFramesCSInterface(_crowdsaleContract);
      initOwned(msg.sender);
    }

    // Setter functions
    function setFrameCrowdsaleContract(address _crowdsaleContract) public onlyOwner {
      require(_crowdsaleContract != address(0));
       emit SetFrameCrowdsale(address(crowdsaleContract), _crowdsaleContract);
       crowdsaleContract = DreamFramesCSInterface(_crowdsaleContract);
    }
    function setWallet(address payable _wallet) public onlyOwner {
        require(_wallet != address(0));
        emit WalletUpdated(wallet, _wallet);
        wallet = _wallet;
    }

    function () external payable {
        // require(now >= startDate && now <= endDate);
        // Get number of frames, will revert if sold out
        uint256 ethToTransfer;
        uint256 frames;
        (frames, ethToTransfer) = crowdsaleContract.calculateFrames( msg.value);

        // Update crowdsale contract state
        royaltiesSold = royaltiesSold.add(frames);
        contributedEth = contributedEth.add(ethToTransfer);
        royaltyEthAmount[msg.sender] = royaltyEthAmount[msg.sender].add(ethToTransfer);

        // Accept Payments
        uint256 ethToRefund = msg.value.sub(ethToTransfer);
        if (ethToTransfer > 0) {
            wallet.transfer(ethToTransfer);
        }
        if (ethToRefund > 0) {
            msg.sender.transfer(ethToRefund);
        }
        // Issue royalty tokens
        crowdsaleContract.claimRoyaltyFrames(msg.sender, frames, ethToTransfer);
        emit Purchased(msg.sender, frames, ethToTransfer, royaltiesSold, contributedEth);
    }

    /*
    //  AG: To Remove
    function withdrawFunds() public onlyOwner {
        // AG: Crowdsale over
        msg.sender.transfer(address(this).balance);
    }
    */
}

```
