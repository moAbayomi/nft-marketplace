// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

contract NFTMarketplace {
    struct NFT {
        uint256 price;
        address seller;
    }

    mapping(address => mapping(uint256 => NFT)) private NFTs;

    mapping(address => uint256) private addressToProceeds;

    event ItemListed(
        address indexed seller,
        address indexed nftAddress,
        uint256 indexed tokenId,
        uint256 price
    );
    event ItemBought(
        address indexed buyer,
        address indexed seller,
        uint256 indexed tokenId,
        uint256 price
    );

    modifier notListed(
        address nftAddress,
        uint256 tokenId,
        address owner
    ) {
        NFT memory nft = NFTs[nftAddress][tokenId];
        if (nft.price > 0) {
            revert("already listed");
        }
        _;
    }

    modifier isListed(address nftAddress, uint256 tokenId) {
        NFT memory nft = NFTs[nftAddress][tokenId];
        require(nft.price > 0, "not listed");
        _;
    }

    modifier onlyOwner(
        address nftAddress,
        uint256 tokenId,
        address spender
    ) {
        IERC721 nft = IERC721(nftAddress);
        address owner = nft.ownerOf(tokenId);
        require(spender == owner, "youre not the owner !");
        _;
    }

    function listNft(
        address nftAddress,
        uint256 tokenId,
        uint256 price
    )
        external
        notListed(nftAddress, tokenId, msg.sender)
        onlyOwner(nftAddress, tokenId, msg.sender)
    {
        IERC721 nft = IERC721(nftAddress);
        require(
            nft.getApproved(tokenId) == address(this),
            "youre not approved !"
        );

        NFTs[nftAddress][tokenId] = NFT({price: price, seller: msg.sender});

        emit ItemListed(msg.sender, nftAddress, tokenId, price);
    }

    function buyNft(
        address nftAddress,
        uint256 tokenId
    ) external payable isListed(nftAddress, tokenId) {
        NFT memory nft = NFTs[nftAddress][tokenId];
        require(msg.value == nft.price, "price doesnt match, please check");

        addressToProceeds[nft.seller] += msg.value;

        delete NFTs[nftAddress][tokenId];

        IERC721(nftAddress).safeTransferFrom(nft.seller, msg.sender, tokenId);
        emit ItemBought(msg.sender, nft.seller, tokenId, nft.price);
    }

    function withdraw() external {
        uint256 funds = addressToProceeds[msg.sender];
        require(funds > 0, "no funds to withdraw");

        addressToProceeds[msg.sender] = 0;

        payable(msg.sender).transfer(funds);
    }

    function getNft(
        address nftAddress,
        uint256 tokenId
    ) external view returns (NFT memory) {
        return NFTs[nftAddress][tokenId];
    }

    function balance(address seller) external view returns (uint256) {
        return addressToProceeds[seller];
    }
}
