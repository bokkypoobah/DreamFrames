# BonusList

Source file [../../contracts/DreamFramesToken/BonusList.sol](../../contracts/DreamFramesToken/BonusList.sol).

<br />

<hr />

```solidity
pragma solidity ^0.5.4;

// ----------------------------------------------------------------------------
// Dream Frames Bonus List
//
// Deployed to:
//
// Enjoy.
//
// (c) Adrian Guerrera / Deepyr Pty Ltd for Dream Frames 2019. The MIT Licence.
// (c) BokkyPooBah / Bok Consulting Pty Ltd for GazeCoin 2018. The MIT Licence.
// ----------------------------------------------------------------------------

import "../Shared/Operated.sol";
import "./BonusListInterface.sol";


// ----------------------------------------------------------------------------
// Bonus List - on list or not
// ----------------------------------------------------------------------------
contract BonusList is BonusListInterface, Operated {
    mapping(address => bool) public bonusList;

    event AccountListed(address indexed account, bool status);

    constructor() public {
        initOperated(msg.sender);
    }

    function isInBonusList(address account) public view returns (bool) {
        return bonusList[account];
    }

    function add(address[] accounts) public onlyOperator {
        require(accounts.length != 0);
        for (uint i = 0; i < accounts.length; i++) {
            require(accounts[i] != address(0));
            if (!bonusList[accounts[i]]) {
                bonusList[accounts[i]] = true;
                emit AccountListed(accounts[i], true);
            }
        }
    }
    function remove(address[] accounts) public onlyOperator {
        require(accounts.length != 0);
        for (uint i = 0; i < accounts.length; i++) {
            require(accounts[i] != address(0));
            if (bonusList[accounts[i]]) {
                delete bonusList[accounts[i]];
                emit AccountListed(accounts[i], false);
            }
        }
    }
}

```
