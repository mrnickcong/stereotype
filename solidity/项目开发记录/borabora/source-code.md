# bora合约的主要代码


## 源码位置

https://bscscan.com/token/0xabac6324a2bc094357d715b219ceed7b4e572d8c#code


## 以下为主要的源码

```text
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";
import "@openzeppelin/contracts/utils/Strings.sol";
import "../interface/ITuna.sol";
import "../interface/IBora.sol";
import "../interface/IEvent.sol";



contract Bora is ERC721, IBora {

    using Strings for uint256;

    mapping(uint256 => uint256) private _levelOf;

    mapping(uint256 => bool) public _signatureUsed;

    mapping(address => bool) public isMinted;

    address public owner;
    address public signer;
    address public tunaAddr;
    string public baseUri;
    address public _eventAddress;

    using ECDSA for bytes32;
    using Counters for Counters.Counter;
    Counters.Counter public counter;

    

    constructor(address _singer, string memory _uri, address _tunaAddr, address eventAddress) ERC721("Bora SmartLiz Club", "BORA") {
        counter.reset();
        owner = msg.sender;
        tunaAddr = _tunaAddr;
        baseUri = _uri;
        signer = _singer;
        _eventAddress = eventAddress;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "you have not right");
        _;
    }

    function getNFTLevel(uint256 tokenId) public view returns(uint256) {
        return _levelOf[tokenId];
    }

    function mint() external {

        require(isMinted[msg.sender] == false, "Bora_mint: you have minted");
        isMinted[msg.sender] = true;

        counter.increment();
        uint256 tokenId = counter.current();
        _mint(msg.sender, tokenId);
        
        bytes memory log = abi.encode(msg.sender, tokenId);
        IEvent(_eventAddress).eventOut(1, log);
    }

    function updateLevel(
        uint256 id,
        uint256 tokenId,
        uint256 tunaAmount,
        uint256 feature,
        bytes memory sign
        ) external {

            require(!_signatureUsed[id], "Bora_updateLevel: signature reused.");
            _signatureUsed[id] = true;

            bytes32 hash = keccak256(abi.encodePacked("Bora_updateLevel", id, msg.sender, tokenId, tunaAmount, feature));
            address signerOfMsg = hash.toEthSignedMessageHash().recover(sign);
            require(signer == signerOfMsg, "Bora_updateLevel: invalid signature.");

            require(ownerOf(tokenId) == msg.sender, "Bora_updateLevel: you are not owner.");
            require(getNFTLevel(tokenId) < 20, "Bora_updateLevel: nft level limits in 20");

            _levelOf[tokenId] += 1;
            ITuna(tunaAddr).burnThroughUpdateBoraNFT(msg.sender, tunaAmount);

            bytes memory log = abi.encode(id, msg.sender, tokenId, getNFTLevel(tokenId), tunaAmount, feature);
            IEvent(_eventAddress).eventOut(2, log);

        }

    function _baseURI() internal view override returns (string memory) {
        return baseUri;
    }

    function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {
        _requireMinted(tokenId);

        string memory baseURI = _baseURI();
        uint256 level = getNFTLevel(tokenId);
        return bytes(baseURI).length > 0 ? string(abi.encodePacked(baseURI, level.toString())) : "";
    }

    function getCurrentTokenId() public view returns(uint256){
        return counter.current();
    }

    function getNextTokenId() public view returns(uint256){
        uint256 tokenId = counter.current();
        return tokenId + 1;
    }
    
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 tokenId
    ) internal override {

        bytes memory log = abi.encode(from, to, tokenId);
        IEvent(_eventAddress).eventOut(3, log);
    }

    function setSigner(address newSigner) external onlyOwner {
        signer = newSigner;
    }

    function setTunaAddr(address newTunaAddr) external onlyOwner {
        tunaAddr = newTunaAddr;
    }
    
    function setBaseUri(string memory newUri) external onlyOwner {
        baseUri = newUri;
    }
    function setEvent(address newEvent) external onlyOwner{
        _eventAddress = newEvent;
    }
    function transferOwner(address newOwner) external onlyOwner {
        owner = newOwner;
    }
}
```