# NFT Marketplace & NFT Minting Contracts

This repository contains two Solidity smart contracts:

1. **Marketplace Contract**: A decentralized marketplace to buy and sell NFTs (ERC721 tokens).
2. **NFT Minting Contract**: A basic contract to mint new NFTs (ERC721 tokens) and store their metadata (token URI).

## Prerequisites

- Solidity version ^0.8.4
- OpenZeppelin contracts (version 4.5.0)
- Ethereum environment (e.g., Remix, Hardhat, Truffle)

## Contracts

### 1. **Marketplace Contract**
This contract allows users to list and purchase NFTs. It implements the following features:
- **Listing an NFT**: Users can list their NFTs for sale by specifying the price.
- **Buying an NFT**: Users can buy listed NFTs, paying the seller and a transaction fee to the contract owner.
- **Transaction Fee**: The contract owner can set a fee percentage that will be deducted from each transaction.
- **Reentrancy Guard**: The contract uses OpenZeppelin's `ReentrancyGuard` to prevent reentrancy attacks during transactions.

#### Functions:
- `makeItem(IERC721 _nft, uint _tokenId, uint _price)`: Lists an NFT for sale.
- `purchaseItem(uint _itemId)`: Allows a user to purchase a listed NFT.
- `getTotalPrice(uint _itemId)`: Returns the total price (including fee) for a listed item.

#### Events:
- `Offered`: Emitted when an item is listed for sale.
- `Bought`: Emitted when an item is purchased.

### 2. **NFT Minting Contract**
This contract allows users to mint new NFTs. It uses OpenZeppelin's `ERC721URIStorage` to mint tokens and store their metadata.

#### Functions:
- `mint(string memory _tokenURI)`: Mints a new NFT and assigns a URI to it, returning the token ID.

## How to Use

1. **Deploy the Contracts**:
   - Deploy the `NFT` contract first to mint NFTs.
   - Deploy the `Marketplace` contract, passing the fee percentage as a parameter during deployment.

2. **Mint an NFT**:
   - Use the `mint` function of the `NFT` contract to mint new NFTs. You will need to provide a valid URI for the token's metadata.

3. **List an NFT for Sale**:
   - Once you have an NFT, you can list it for sale using the `makeItem` function from the `Marketplace` contract. Provide the NFT contract address, token ID, and price.

4. **Buy an NFT**:
   - Use the `purchaseItem` function to buy a listed NFT by sending the required amount of Ether.

## Example Usage

### Deploy `NFT` contract:
```solidity
NFT nft = new NFT();
uint tokenId = nft.mint("ipfs://<tokenURI>");
```

### Deploy `Marketplace` contract:
```solidity
Marketplace marketplace = new Marketplace(5); // 5% transaction fee
```

### List an NFT for Sale:
```solidity
nft.approve(address(marketplace), tokenId);
marketplace.makeItem(nft, tokenId, 1 ether);
```

### Buy an NFT:
```solidity
marketplace.purchaseItem(itemId);  // Send enough Ether to cover the price + fee
```

## Notes
- Ensure that the `NFT` contract is approved to transfer the NFTs to the `Marketplace` contract before listing.
- The marketplace contract ensures that the seller receives the agreed price, while the contract owner collects a fee.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
