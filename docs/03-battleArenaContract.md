# Bragon Nft Arena Contract

## Overview

The `NFTBattleArena` contract is a Solidity smart contract that facilitates battles between NFTs (Non-Fungible Tokens) listed for battle. The contract supports various functionalities such as listing NFTs, starting battles, determining winners based on specified attributes, and claiming rewards. Additionally, it incorporates the `IBlast` interface for handling yield and gas-related operations.

## Contract Structure

### Contract Inheritance

The `NFTBattleArena` contract inherits from the `Ownable` contract, providing ownership control over the contract.

### Enums

#### `Terraria`

- **DESERT:** Represents the terraria type "Desert."
- **MOUNTAIN:** Represents the terraria type "Mountain."
- **VALLEY:** Represents the terraria type "Valley."

#### `YieldMode`

- **AUTOMATIC:** Automatic yield mode.
- **VOID:** Void yield mode.
- **CLAIMABLE:** Claimable yield mode.

#### `GasMode`

- **VOID:** Void gas mode.
- **CLAIMABLE:** Claimable gas mode.

### Structs

#### `NFTAttributes`

- **speed:** Speed attribute of an NFT.
- **damage:** Damage attribute of an NFT.
- **intelligence:** Intelligence attribute of an NFT.
- **hp:** Health points attribute of an NFT.

#### `Battle`

- **nft1Id:** Token ID of the defending NFT.
- **nft2Id:** Token ID of the attacking NFT.
- **startTime:** Start time of the battle.
- **isFinished:** Flag indicating whether the battle is finished.

## Events

- **ItemListed:** Fired when an NFT is listed for battle.
- **ItemCanceled:** Fired when an NFT listing is canceled.
- **TerrariaChosen:** Fired when a terraria type is chosen for a battle.
- **WinnerPicked:** Fired when a winner is determined in a battle.
- **ItemBurned:** Fired when an NFT is burned.

## Modifiers

- **isOwner:** Checks if the caller is the owner of a specified NFT.
- **isListed:** Checks if an NFT is listed for battle.

## Functions

### `setNFTAttributes`

``` solidity
function setNFTAttributes(uint256 tokenId, NFTAttributes calldata _attributes) public onlyOwner
```

Sets the attributes of an NFT.

### `listNFTForBattle`

``` solidity
function listNFTForBattle(uint256 tokenId) public isOwner(tokenId, msg.sender)
```

Lists an NFT for battle.

### `unlistNFTFromBattle`

``` solidity
function unlistNFTFromBattle(uint256 tokenId) public isOwner(tokenId, msg.sender)
```

Unlists an NFT from battle.

### `burnItem`

``` solidity
function burnItem(uint256 tokenId) public
```

Burns an NFT, transferring it to the burn address.

### `startBattle`

``` solidity
function startBattle(uint256 _tokenIdDef, uint256 _tokenIdAtk, uint256 _randomInt) public isListed(_tokenIdDef) isOwner(_tokenIdAtk, msg.sender)
```

Starts a battle between two NFTs.

### `getAttributes`

``` solidity
function getAttributes(uint256 _tokenId) public view returns (NFTAttributes memory)
```

Gets the attributes of an NFT.

### `getWinnerTokenId`

``` solidity
function getWinnerTokenId() public view returns (uint256)
```

Gets the Token ID of the winner in the last battle.

### `getLoserTokenId`

``` solidity
function getLoserTokenId() public view returns (uint256)
```

Gets the Token ID of the loser in the last battle.

### `checkTerraria`

``` solidity
function checkTerraria() public view returns (uint256)
```

Gets the current terraria type chosen for the battle.

### `claimYield`

``` solidity
function claimYield(address recipient, uint256 amount) external onlyOwner
```

Claims yield for the specified amount on behalf of the contract.

### `claimAllYield`

``` solidity
function claimAllYield(address recipient) external onlyOwner
```

Claims all available yield on behalf of the contract.

### `claimAllGas`

``` solidity
function claimAllGas(address recipient) external onlyOwner
```

Claims all available gas on behalf of the contract.

### `claimMaxGas`

``` solidity
function claimMaxGas(address recipient) external onlyOwner
```

Claims fully vested gas (at a 0% tax rate) on behalf of the contract.

### `readClaimableYield`

``` solidity
function readClaimableYield() public view returns (uint256)
```

Reads the amount of claimable yield for the contract.

### `readGasParams`

``` solidity
function readGasParams() public view returns (uint256 etherSeconds, uint256 etherBalance, uint256 lastUpdated, GasMode)
```

Reads the gas-related parameters for the contract.

## Constructor

### `NFTBattleArena`

``` solidity
constructor(address _nftCollection) 
```

Constructor initializes the `NFTBattleArena` contract with the address of the NFT collection contract.
