# NFT Marketplace Smart Contract

## Overview

This Solidity smart contract implements a simple NFT (Non-Fungible Token) marketplace, allowing users to list their NFTs for sale and enabling other users to purchase them. The contract also includes a mechanism for the seller to withdraw their proceeds.

## Features

- **List NFT:** Owners can list their NFTs for sale by specifying the NFT contract address, token ID, and the desired price.
- **Buy NFT:** Buyers can purchase listed NFTs by providing the appropriate payment.
- **Withdraw:** Sellers can withdraw their proceeds from successful NFT sales.
- **View NFT Information:** Users can retrieve information about a listed NFT, including the price and seller details.
- **Check Balance:** Users can check their available proceeds from NFT sales.

## Smart Contract Details

### NFT Structure

The contract uses a structure called `NFT` to store information about each listed NFT, including the price and the seller's address.

### Mapping

The contract uses two mappings:
- `NFTs`: Maps NFT contract addresses and token IDs to their corresponding `NFT` structure.
- `addressToProceeds`: Maps user addresses to their available proceeds from NFT sales.

### Events

The contract emits two events:
- `ItemListed`: Triggered when a user lists an NFT for sale.
- `ItemBought`: Triggered when a user successfully purchases a listed NFT.

### Modifiers

The contract includes three modifiers:
- `notListed`: Ensures that an NFT is not already listed before attempting to list it.
- `isListed`: Ensures that an NFT is listed before attempting to buy it.
- `onlyOwner`: Ensures that the caller is the owner of the NFT being operated upon.

### Functions

1. **`listNft`**: Allows NFT owners to list their NFTs for sale.

2. **`buyNft`**: Allows users to buy listed NFTs, transferring the NFT to the buyer and the sale proceeds to the seller.

3. **`withdraw`**: Allows sellers to withdraw their proceeds from successful NFT sales.

4. **`getNft`**: Retrieves information about a listed NFT.

5. **`balance`**: Retrieves the available proceeds for a specific user.



## Disclaimer

This smart contract is my solution to the Training 11 assignment of the BIH x Arbitrum bootcamp.
