# BTTSTokenLibrary120

Source file [../../contracts/Shared/BTTSTokenLibrary120.sol](../../contracts/Shared/BTTSTokenLibrary120.sol).

<br />

<hr />

```solidity
pragma solidity ^0.5.4;

// ----------------------------------------------------------------------------
// BokkyPooBah's Token Teleportation Service v1.20
//
// https://github.com/bokkypoobah/BokkyPooBahsTokenTeleportationServiceSmartContract
//
// Enjoy. (c) BokkyPooBah / Bok Consulting Pty Ltd 2018. The MIT Licence.
// ----------------------------------------------------------------------------

import "./BTTSTokenInterface120.sol";

// ----------------------------------------------------------------------------
// BokkyPooBah's Token Teleportation Service Library v1.20
//
// Enjoy. (c) BokkyPooBah / Bok Consulting Pty Ltd 2018. The MIT Licence.
// ----------------------------------------------------------------------------
library BTTSLib {
   struct Data {
       bool initialised;

       // Ownership
       address owner;
       address newOwner;

       // Minting and management
       address minter;
       bool mintable;
       bool transferable;
       mapping(address => bool) accountLocked;

       // Token
       string symbol;
       string name;
       uint8 decimals;
       uint totalSupply;
       mapping(address => uint) balances;
       mapping(address => mapping(address => uint)) allowed;
       mapping(address => uint) nextNonce;
   }


   // ------------------------------------------------------------------------
   // Constants
   // ------------------------------------------------------------------------
   uint public constant bttsVersion = 110;
   bytes public constant signingPrefix = "\x19Ethereum Signed Message:\n32";
   bytes4 public constant signedTransferSig = "\x75\x32\xea\xac";
   bytes4 public constant signedApproveSig = "\xe9\xaf\xa7\xa1";
   bytes4 public constant signedTransferFromSig = "\x34\x4b\xcc\x7d";
   bytes4 public constant signedApproveAndCallSig = "\xf1\x6f\x9b\x53";

   // ------------------------------------------------------------------------
   // Event
   // ------------------------------------------------------------------------
   event OwnershipTransferred(address indexed from, address indexed to);
   event MinterUpdated(address from, address to);
   event Mint(address indexed tokenOwner, uint tokens, bool lockAccount);
   event MintingDisabled();
   event TransfersEnabled();
   event AccountUnlocked(address indexed tokenOwner);
   event Transfer(address indexed from, address indexed to, uint tokens);
   event Approval(address indexed tokenOwner, address indexed spender, uint tokens);


   // ------------------------------------------------------------------------
   // Initialisation
   // ------------------------------------------------------------------------
   function init(Data storage self, address owner, string memory symbol, string memory name, uint8 decimals, uint initialSupply, bool mintable, bool transferable) public {
       require(!self.initialised);
       self.initialised = true;
       self.owner = owner;
       self.symbol = symbol;
       self.name = name;
       self.decimals = decimals;
       if (initialSupply > 0) {
           self.balances[owner] = initialSupply;
           self.totalSupply = initialSupply;
           emit Mint(self.owner, initialSupply, false);
           emit Transfer(address(0), self.owner, initialSupply);
       }
       self.mintable = mintable;
       self.transferable = transferable;
   }

   // ------------------------------------------------------------------------
   // Safe maths, inspired by OpenZeppelin
   // ------------------------------------------------------------------------
   function safeAdd(uint a, uint b) internal pure returns (uint c) {
       c = a + b;
       require(c >= a);
   }
   function safeSub(uint a, uint b) internal pure returns (uint c) {
       require(b <= a);
       c = a - b;
   }
   function safeMul(uint a, uint b) internal pure returns (uint c) {
       c = a * b;
       require(a == 0 || c / a == b);
   }
   function safeDiv(uint a, uint b) internal pure returns (uint c) {
       require(b > 0);
       c = a / b;
   }

   // ------------------------------------------------------------------------
   // Ownership
   // ------------------------------------------------------------------------
   function transferOwnership(Data storage self, address newOwner) public {
       require(msg.sender == self.owner);
       self.newOwner = newOwner;
   }
   function acceptOwnership(Data storage self) public {
       require(msg.sender == self.newOwner);
       emit OwnershipTransferred(self.owner, self.newOwner);
       self.owner = self.newOwner;
       self.newOwner = address(0);
   }
   function transferOwnershipImmediately(Data storage self, address newOwner) public {
       require(msg.sender == self.owner);
       emit OwnershipTransferred(self.owner, newOwner);
       self.owner = newOwner;
       self.newOwner = address(0);
   }

   // ------------------------------------------------------------------------
   // Minting and management
   // ------------------------------------------------------------------------
   function setMinter(Data storage self, address minter) public {
       require(msg.sender == self.owner);
       require(self.mintable);
       emit MinterUpdated(self.minter, minter);
       self.minter = minter;
   }
   function mint(Data storage self, address tokenOwner, uint tokens, bool lockAccount) public returns (bool success) {
       require(self.mintable);
       require(msg.sender == self.minter || msg.sender == self.owner);
       if (lockAccount) {
           self.accountLocked[tokenOwner] = true;
       }
       self.balances[tokenOwner] = safeAdd(self.balances[tokenOwner], tokens);
       self.totalSupply = safeAdd(self.totalSupply, tokens);
       emit Mint(tokenOwner, tokens, lockAccount);
       emit Transfer(address(0), tokenOwner, tokens);
       return true;
   }
   function unlockAccount(Data storage self, address tokenOwner) public {
       require(msg.sender == self.owner);
       require(self.accountLocked[tokenOwner]);
       self.accountLocked[tokenOwner] = false;
       emit AccountUnlocked(tokenOwner);
   }
   function disableMinting(Data storage self) public {
       require(self.mintable);
       require(msg.sender == self.minter || msg.sender == self.owner);
       self.mintable = false;
       if (self.minter != address(0)) {
           emit MinterUpdated(self.minter, address(0));
           self.minter = address(0);
       }
       emit MintingDisabled();
   }
   function enableTransfers(Data storage self) public {
       require(msg.sender == self.owner);
       require(!self.transferable);
       self.transferable = true;
       emit TransfersEnabled();
   }

   // ------------------------------------------------------------------------
   // Other functions
   // ------------------------------------------------------------------------
   function transferAnyERC20Token(Data storage self, address tokenAddress, uint tokens) public returns (bool success) {
       require(msg.sender == self.owner);
       return ERC20Interface(tokenAddress).transfer(self.owner, tokens);
   }

   // ------------------------------------------------------------------------
   // ecrecover from a signature rather than the signature in parts [v, r, s]
   // The signature format is a compact form {bytes32 r}{bytes32 s}{uint8 v}.
   // Compact means, uint8 is not padded to 32 bytes.
   //
   // An invalid signature results in the address(0) being returned, make
   // sure that the returned result is checked to be non-zero for validity
   //
   // Parts from https://gist.github.com/axic/5b33912c6f61ae6fd96d6c4a47afde6d
   // ------------------------------------------------------------------------
   function ecrecoverFromSig(bytes32 hash, bytes memory sig) public pure returns (address recoveredAddress) {
       bytes32 r;
       bytes32 s;
       uint8 v;
       if (sig.length != 65) return address(0);
       assembly {
           r := mload(add(sig, 32))
           s := mload(add(sig, 64))
           // Here we are loading the last 32 bytes. We exploit the fact that 'mload' will pad with zeroes if we overread.
           // There is no 'mload8' to do this, but that would be nicer.
           v := byte(0, mload(add(sig, 96)))
       }
       // Albeit non-transactional signatures are not specified by the YP, one would expect it to match the YP range of [27, 28]
       // geth uses [0, 1] and some clients have followed. This might change, see https://github.com/ethereum/go-ethereum/issues/2053
       if (v < 27) {
         v += 27;
       }
       if (v != 27 && v != 28) return address(0);
       return ecrecover(hash, v, r, s);
   }

   // ------------------------------------------------------------------------
   // Get CheckResult message
   // ------------------------------------------------------------------------
   function getCheckResultMessage(Data storage /*self*/, BTTSTokenInterface.CheckResult result) public pure returns (string memory) {
       if (result == BTTSTokenInterface.CheckResult.Success) {
           return "Success";
       } else if (result == BTTSTokenInterface.CheckResult.NotTransferable) {
           return "Tokens not transferable yet";
       } else if (result == BTTSTokenInterface.CheckResult.AccountLocked) {
           return "Account locked";
       } else if (result == BTTSTokenInterface.CheckResult.SignerMismatch) {
           return "Mismatch in signing account";
       } else if (result == BTTSTokenInterface.CheckResult.InvalidNonce) {
           return "Invalid nonce";
       } else if (result == BTTSTokenInterface.CheckResult.InsufficientApprovedTokens) {
           return "Insufficient approved tokens";
       } else if (result == BTTSTokenInterface.CheckResult.InsufficientApprovedTokensForFees) {
           return "Insufficient approved tokens for fees";
       } else if (result == BTTSTokenInterface.CheckResult.InsufficientTokens) {
           return "Insufficient tokens";
       } else if (result == BTTSTokenInterface.CheckResult.InsufficientTokensForFees) {
           return "Insufficient tokens for fees";
       } else if (result == BTTSTokenInterface.CheckResult.OverflowError) {
           return "Overflow error";
       } else {
           return "Unknown error";
       }
   }

   // ------------------------------------------------------------------------
   // Token functions
   // ------------------------------------------------------------------------
   function transfer(Data storage self, address to, uint tokens) public returns (bool success) {
       // Owner and minter can move tokens before the tokens are transferable
       require(self.transferable || (self.mintable && (msg.sender == self.owner  || msg.sender == self.minter)));
       require(!self.accountLocked[msg.sender]);
       self.balances[msg.sender] = safeSub(self.balances[msg.sender], tokens);
       self.balances[to] = safeAdd(self.balances[to], tokens);
       emit Transfer(msg.sender, to, tokens);
       return true;
   }
   function approve(Data storage self, address spender, uint tokens) public returns (bool success) {
       require(!self.accountLocked[msg.sender]);
       self.allowed[msg.sender][spender] = tokens;
       emit Approval(msg.sender, spender, tokens);
       return true;
   }
   function transferFrom(Data storage self, address from, address to, uint tokens) public returns (bool success) {
       require(self.transferable);
       require(!self.accountLocked[from]);
       self.balances[from] = safeSub(self.balances[from], tokens);
       self.allowed[from][msg.sender] = safeSub(self.allowed[from][msg.sender], tokens);
       self.balances[to] = safeAdd(self.balances[to], tokens);
       emit Transfer(from, to, tokens);
       return true;
   }
   function approveAndCall(Data storage self, address spender, uint tokens, bytes memory data) public returns (bool success) {
       require(!self.accountLocked[msg.sender]);
       self.allowed[msg.sender][spender] = tokens;
       emit Approval(msg.sender, spender, tokens);
       ApproveAndCallFallBack(spender).receiveApproval(msg.sender, tokens, address(this), data);
       return true;
   }

   // ------------------------------------------------------------------------
   // Signed function
   // ------------------------------------------------------------------------
   function signedTransferHash(Data storage /*self*/, address tokenOwner, address to, uint tokens, uint fee, uint nonce) public view returns (bytes32 hash) {
       hash = keccak256(abi.encodePacked(signedTransferSig, address(this), tokenOwner, to, tokens, fee, nonce));
   }
   function signedTransferCheck(Data storage self, address tokenOwner, address to, uint tokens, uint fee, uint nonce, bytes memory sig, address feeAccount) public view returns (BTTSTokenInterface.CheckResult result) {
       if (!self.transferable) return BTTSTokenInterface.CheckResult.NotTransferable;
       bytes32 hash = signedTransferHash(self, tokenOwner, to, tokens, fee, nonce);
       if (tokenOwner == address(0) || tokenOwner != ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig)) return BTTSTokenInterface.CheckResult.SignerMismatch;
       if (self.accountLocked[tokenOwner]) return BTTSTokenInterface.CheckResult.AccountLocked;
       if (self.nextNonce[tokenOwner] != nonce) return BTTSTokenInterface.CheckResult.InvalidNonce;
       uint total = safeAdd(tokens, fee);
       if (self.balances[tokenOwner] < tokens) return BTTSTokenInterface.CheckResult.InsufficientTokens;
       if (self.balances[tokenOwner] < total) return BTTSTokenInterface.CheckResult.InsufficientTokensForFees;
       if (self.balances[to] + tokens < self.balances[to]) return BTTSTokenInterface.CheckResult.OverflowError;
       if (self.balances[feeAccount] + fee < self.balances[feeAccount]) return BTTSTokenInterface.CheckResult.OverflowError;
       return BTTSTokenInterface.CheckResult.Success;
   }
   function signedTransfer(Data storage self, address tokenOwner, address to, uint tokens, uint fee, uint nonce, bytes memory sig, address feeAccount) public returns (bool success) {
       require(self.transferable);
       bytes32 hash = signedTransferHash(self, tokenOwner, to, tokens, fee, nonce);
       require(tokenOwner != address(0) && tokenOwner == ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig));
       require(!self.accountLocked[tokenOwner]);
       require(self.nextNonce[tokenOwner] == nonce);
       self.nextNonce[tokenOwner] = nonce + 1;
       self.balances[tokenOwner] = safeSub(self.balances[tokenOwner], tokens);
       self.balances[to] = safeAdd(self.balances[to], tokens);
       emit Transfer(tokenOwner, to, tokens);
       self.balances[tokenOwner] = safeSub(self.balances[tokenOwner], fee);
       self.balances[feeAccount] = safeAdd(self.balances[feeAccount], fee);
       emit Transfer(tokenOwner, feeAccount, fee);
       return true;
   }
   function signedApproveHash(Data storage /*self*/, address tokenOwner, address spender, uint tokens, uint fee, uint nonce) public view returns (bytes32 hash) {
       hash = keccak256(abi.encodePacked(signedApproveSig, address(this), tokenOwner, spender, tokens, fee, nonce));
   }
   function signedApproveCheck(Data storage self, address tokenOwner, address spender, uint tokens, uint fee, uint nonce, bytes memory sig, address feeAccount) public view returns (BTTSTokenInterface.CheckResult result) {
       if (!self.transferable) return BTTSTokenInterface.CheckResult.NotTransferable;
       bytes32 hash = signedApproveHash(self, tokenOwner, spender, tokens, fee, nonce);
       if (tokenOwner == address(0) || tokenOwner != ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig)) return BTTSTokenInterface.CheckResult.SignerMismatch;
       if (self.accountLocked[tokenOwner]) return BTTSTokenInterface.CheckResult.AccountLocked;
       if (self.nextNonce[tokenOwner] != nonce) return BTTSTokenInterface.CheckResult.InvalidNonce;
       if (self.balances[tokenOwner] < fee) return BTTSTokenInterface.CheckResult.InsufficientTokensForFees;
       if (self.balances[feeAccount] + fee < self.balances[feeAccount]) return BTTSTokenInterface.CheckResult.OverflowError;
       return BTTSTokenInterface.CheckResult.Success;
   }
   function signedApprove(Data storage self, address tokenOwner, address spender, uint tokens, uint fee, uint nonce, bytes memory sig, address feeAccount) public returns (bool success) {
       require(self.transferable);
       bytes32 hash = signedApproveHash(self, tokenOwner, spender, tokens, fee, nonce);
       require(tokenOwner != address(0) && tokenOwner == ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig));
       require(!self.accountLocked[tokenOwner]);
       require(self.nextNonce[tokenOwner] == nonce);
       self.nextNonce[tokenOwner] = nonce + 1;
       self.allowed[tokenOwner][spender] = tokens;
       emit Approval(tokenOwner, spender, tokens);
       self.balances[tokenOwner] = safeSub(self.balances[tokenOwner], fee);
       self.balances[feeAccount] = safeAdd(self.balances[feeAccount], fee);
       emit Transfer(tokenOwner, feeAccount, fee);
       return true;
   }
   function signedTransferFromHash(Data storage /*self*/, address spender, address from, address to, uint tokens, uint fee, uint nonce) public view returns (bytes32 hash) {
       hash = keccak256(abi.encodePacked(signedTransferFromSig, address(this), spender, from, to, tokens, fee, nonce));
   }
   function signedTransferFromCheck(Data storage self, address spender, address from, address to, uint tokens, uint fee, uint nonce, bytes memory sig, address feeAccount) public view returns (BTTSTokenInterface.CheckResult result) {
       if (!self.transferable) return BTTSTokenInterface.CheckResult.NotTransferable;
       bytes32 hash = signedTransferFromHash(self, spender, from, to, tokens, fee, nonce);
       if (spender == address(0) || spender != ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig)) return BTTSTokenInterface.CheckResult.SignerMismatch;
       if (self.accountLocked[from]) return BTTSTokenInterface.CheckResult.AccountLocked;
       if (self.nextNonce[spender] != nonce) return BTTSTokenInterface.CheckResult.InvalidNonce;
       uint total = safeAdd(tokens, fee);
       if (self.allowed[from][spender] < tokens) return BTTSTokenInterface.CheckResult.InsufficientApprovedTokens;
       if (self.allowed[from][spender] < total) return BTTSTokenInterface.CheckResult.InsufficientApprovedTokensForFees;
       if (self.balances[from] < tokens) return BTTSTokenInterface.CheckResult.InsufficientTokens;
       if (self.balances[from] < total) return BTTSTokenInterface.CheckResult.InsufficientTokensForFees;
       if (self.balances[to] + tokens < self.balances[to]) return BTTSTokenInterface.CheckResult.OverflowError;
       if (self.balances[feeAccount] + fee < self.balances[feeAccount]) return BTTSTokenInterface.CheckResult.OverflowError;
       return BTTSTokenInterface.CheckResult.Success;
   }
   function signedTransferFrom(Data storage self, address spender, address from, address to, uint tokens, uint fee, uint nonce, bytes memory sig, address feeAccount) public returns (bool success) {
       require(self.transferable);
       bytes32 hash = signedTransferFromHash(self, spender, from, to, tokens, fee, nonce);
       require(spender != address(0) && spender == ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig));
       require(!self.accountLocked[from]);
       require(self.nextNonce[spender] == nonce);
       self.nextNonce[spender] = nonce + 1;
       self.balances[from] = safeSub(self.balances[from], tokens);
       self.allowed[from][spender] = safeSub(self.allowed[from][spender], tokens);
       self.balances[to] = safeAdd(self.balances[to], tokens);
       emit Transfer(from, to, tokens);
       self.balances[from] = safeSub(self.balances[from], fee);
       self.allowed[from][spender] = safeSub(self.allowed[from][spender], fee);
       self.balances[feeAccount] = safeAdd(self.balances[feeAccount], fee);
       emit Transfer(from, feeAccount, fee);
       return true;
   }
   function signedApproveAndCallHash(Data storage /*self*/, address tokenOwner, address spender, uint tokens, bytes memory data, uint fee, uint nonce) public view returns (bytes32 hash) {
       hash = keccak256(abi.encodePacked(signedApproveAndCallSig, address(this), tokenOwner, spender, tokens, data, fee, nonce));
   }
   function signedApproveAndCallCheck(Data storage self, address tokenOwner, address spender, uint tokens, bytes memory data, uint fee, uint nonce, bytes memory sig, address feeAccount) public view returns (BTTSTokenInterface.CheckResult result) {
       if (!self.transferable) return BTTSTokenInterface.CheckResult.NotTransferable;
       bytes32 hash = signedApproveAndCallHash(self, tokenOwner, spender, tokens, data, fee, nonce);
       if (tokenOwner == address(0) || tokenOwner != ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig)) return BTTSTokenInterface.CheckResult.SignerMismatch;
       if (self.accountLocked[tokenOwner]) return BTTSTokenInterface.CheckResult.AccountLocked;
       if (self.nextNonce[tokenOwner] != nonce) return BTTSTokenInterface.CheckResult.InvalidNonce;
       if (self.balances[tokenOwner] < fee) return BTTSTokenInterface.CheckResult.InsufficientTokensForFees;
       if (self.balances[feeAccount] + fee < self.balances[feeAccount]) return BTTSTokenInterface.CheckResult.OverflowError;
       return BTTSTokenInterface.CheckResult.Success;
   }
   function signedApproveAndCall(Data storage self, address tokenOwner, address spender, uint tokens, bytes memory data, uint fee, uint nonce, bytes memory sig, address feeAccount) public returns (bool success) {
       require(self.transferable);
       bytes32 hash = signedApproveAndCallHash(self, tokenOwner, spender, tokens, data, fee, nonce);
       require(tokenOwner != address(0) && tokenOwner == ecrecoverFromSig(keccak256(abi.encodePacked(signingPrefix, hash)), sig));
       require(!self.accountLocked[tokenOwner]);
       require(self.nextNonce[tokenOwner] == nonce);
       self.nextNonce[tokenOwner] = nonce + 1;
       self.allowed[tokenOwner][spender] = tokens;
       emit Approval(tokenOwner, spender, tokens);
       self.balances[tokenOwner] = safeSub(self.balances[tokenOwner], fee);
       self.balances[feeAccount] = safeAdd(self.balances[feeAccount], fee);
       emit Transfer(tokenOwner, feeAccount, fee);
       ApproveAndCallFallBack(spender).receiveApproval(tokenOwner, tokens, address(this), data);
       return true;
   }
}

```
