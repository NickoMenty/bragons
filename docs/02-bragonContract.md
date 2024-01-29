# BragonNft Contract

## Overview

The `DragonNft` contract is a Solidity smart contract that implements an ERC721-compatible non-fungible token (NFT) standard with URI storage. This contract allows users to mint dragon-themed NFTs, and it includes functionalities such as minting, airdropping, and claiming yield and gas rewards using the `IBlast` interface.

## Contract Structure

### Contract Inheritance

The `DragonNft` contract inherits from both the `ERC721URIStorage` and `Ownable` contracts, providing ERC721 token functionality and ownership control over the contract.

### Enums

#### `YieldMode`

- **AUTOMATIC:** Automatic yield mode.
- **VOID:** Void yield mode.
- **CLAIMABLE:** Claimable yield mode.

#### `GasMode`

- **VOID:** Void gas mode.
- **CLAIMABLE:** Claimable gas mode.

### Structs

None

## Events

- **NftMinted:** Fired when a new NFT is minted.
- **TokensAirdropped:** Fired when tokens are airdropped to multiple recipients.

## Errors

- **DragonNft__AlreadyInitialized:** Contract already initialized error.
- **DragonNft__NeedMoreETHSent:** Insufficient ETH sent error.
- **DragonNft__RangeOutOfBounds:** Out-of-bounds range error.
- **DragonNft__TransferFailed:** Transfer failed error.
- **DragonNft__MaxSupplyReached:** Maximum supply reached error.
- **DragonNft__InvalidAirdrop:** Invalid airdrop error.
- **DragonNft__SoldOut:** Contract sold out error.

## Variables

- **i_mintFee:** Immutable variable representing the minting fee.
- **s_tokenCounter:** Counter for tracking the number of minted tokens.
- **MAX_CHANCE_VALUE:** Constant representing the maximum chance value.
- **i_maxSupply:** Immutable variable representing the maximum supply of NFTs.
- **s_genTokenUris:** Array storing the URIs of generated tokens.
- **s_initialized:** Boolean indicating whether the contract is initialized.
- **i_blast:** Instance of the `IBlast` interface for handling yield and gas operations.

## Constructor

### `DragonNft`

```solidity
constructor(uint256 mintFee, string[10] memory genTokenUris, uint256 maxSupply) ERC721("Bragon", "BRAG")
```

Constructor initializes the `DragonNft` contract with the minting fee, token URIs, and maximum supply.

## Functions

### `mintNft`

```solidity
function mintNft() public payable
```

Mints a new NFT, requires payment of the minting fee.

### `airdropToken`

```solidity
function airdropToken(uint64[] calldata quantity, address[] calldata recipients) external onlyOwner
```

Airdrops tokens to specified recipients based on the provided quantities.

### `withdraw`

```solidity
function withdraw() public onlyOwner
```

Withdraws the contract's ETH balance to the owner.

### `_initializeContract`

```solidity
function _initializeContract(string[10] memory genTokenUris) private
```

Initializes the contract with generated token URIs.

### `claimYield`, `claimAllYield`, `claimAllGas`, `claimMaxGas`

Functions for claiming yield and gas rewards using the `IBlast` interface.

### `readClaimableYield`, `readGasParams`

Functions for reading claimable yield and gas parameters using the `IBlast` interface.

## View and Pure Functions

### `getMintFee`

```solidity
function getMintFee() public view returns (uint256)
```

Returns the current minting fee.

### `getgenTokenUris`

```solidity
function getgenTokenUris(uint256 index) public view returns (string memory)
```

Returns the URI of a generated token at the specified index.

### `getInitialized`

```solidity
function getInitialized() public view returns (bool)
```

Returns whether the contract is initialized.

### `getTokenCounter`

```solidity
function getTokenCounter() public view returns (uint256)
```

Returns the current value of the token counter.

---

Feel free to modify or extend this template to suit your specific documentation needs.