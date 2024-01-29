# Bragon Nft Contract


## BragonNft Contract

### Constructor

This function initializes the WarriorNft contract with the provided mint fee, an array of token URIs, and the maximum supply of tokens.

```solidity
constructor(
    uint256 mintFee,
    string[10] memory genTokenUris,
    uint256 maxSupply
) ERC721("Warrior", "WAIO") {
    _initializeContract(genTokenUris);
    i_mintFee = mintFee;
    s_tokenCounter = 0;
    i_maxSupply = maxSupply;
    i_blast = IBlast(0x4300000000000000000000000000000000000002);
    IBlast(0x4300000000000000000000000000000000000002).configureClaimableYield();
    IBlast(0x4300000000000000000000000000000000000002).configureClaimableGas();
}
```

### `mintNft() public payable`

This function allows users to mint a new NFT by paying the mint fee.

```solidity
function mintNft() public payable {
    // Check for max supply
    if (s_tokenCounter >= i_maxSupply) {
        revert WarriorNft__MaxSupplyReached();
    }
    
    // Check if enough ETH is sent
    if (msg.value < i_mintFee) {
        revert WarriorNft__NeedMoreETHSent();
    }
    
    // Mint NFT
    address genOwner = msg.sender;
    uint256 newItemId = s_tokenCounter;
    s_tokenCounter = s_tokenCounter + 1;
    _safeMint(genOwner, newItemId);
    _setTokenURI(newItemId, s_genTokenUris[s_tokenCounter]);
    nft_health[newItemId] = 6;
    emit NftMinted(newItemId, genOwner);
}
```

### `airdropToken(uint64[] calldata quantity, address[] calldata recipients) external onlyOwner`

This function allows the contract owner to airdrop tokens to multiple recipients.

```solidity
function airdropToken(
    uint64[] calldata quantity,
    address[] calldata recipients
) external onlyOwner {
    // Check for valid input lengths
    uint256 numRecipients = recipients.length; 
    uint256 totalAirdropped; 
    if (numRecipients != quantity.length) revert WarriorNft__InvalidAirdrop(); 

    // Loop through recipients and airdrop tokens
    for (uint256 i = 0; i < numRecipients; ) { 
        for (uint256 k = 0; k < quantity[i]; ) { 
            // Check if supply is sold out
            uint64 updatedAmountMinted = s_tokenCounter + 1;
            if (updatedAmountMinted > i_maxSupply) {
                revert WarriorNft__SoldOut();
            }

            // Airdrop tokens
            s_tokenCounter = updatedAmountMinted;
            totalAirdropped += 1;
            uint256 newItemId = s_tokenCounter;
            _safeMint(recipients[i], newItemId);
            _setTokenURI(newItemId, s_genTokenUris[s_tokenCounter]);

            // Increment counters
            unchecked {
                ++k;
            }
        }
        unchecked {
            ++i;
        }
    }

    emit TokensAirdropped(numRecipients, totalAirdropped);
}
```

### `withdraw() public onlyOwner`

This function allows the contract owner to withdraw the contract's balance.

```solidity
function withdraw() public onlyOwner {
    uint256 amount = address(this).balance;
    (bool success, ) = payable(msg.sender).call{value: amount}("");
    if (!success) {
        revert WarriorNft__TransferFailed();
    }
}
```

### `claimYield(address recipient, uint256 amount) external onlyOwner`

This function allows the contract owner to claim yield on behalf of a recipient.

```solidity
function claimYield(address recipient, uint256 amount) external onlyOwner {
    IBlast(0x4300000000000000000000000000000000000002).claimYield(address(this), recipient, amount);
}
```

### `claimAllYield(address recipient) external onlyOwner`

This function allows the contract owner to claim all yield on behalf of a recipient.

```solidity
function claimAllYield(address recipient) external onlyOwner {
    IBlast(0x4300000000000000000000000000000000000002).claimAllYield(address(this), recipient);
}
```

### `claimAllGas(address recipient) external onlyOwner`

This function allows the contract owner to claim all gas on behalf of a recipient.

```solidity
function claimAllGas(address recipient) external onlyOwner {
    IBlast(0x4300000000000000000000000000000000000002).claimAllGas(address(this), recipient);
}
```

### `claimMaxGas(address recipient) external onlyOwner`

This function allows the contract owner to claim maximum gas on behalf of a recipient.

```solidity
function claimMaxGas(address recipient) external onlyOwner {
    IBlast(0x4300000000000000000000000000000000000002).claimAllGas(address(this), recipient);
}
```

### `readClaimableYield() public view returns (uint256)`

This view function returns the amount of claimable yield for the contract.

```solidity
function readClaimableYield() public view returns (uint256) {
    return IBlast(0x4300000000000000000000000000000000000002).readClaimableYield(address(this));
}
```

### `readGasParams() public view returns (uint256 etherSeconds, uint256 etherBalance, uint256 lastUpdated, GasMode)`

This view function returns gas-related parameters for the contract.

```solidity
function readGasParams() public view returns (uint256 etherSeconds, uint256 etherBalance, uint256 lastUpdated, GasMode) {
    return IBlast(0x430000000000000000000000000000000000000

2).readGasParams(address(this));
}
```

### `getMintFee() public view returns (uint256)`

This view function returns the mint fee required to mint a new NFT.

```solidity
function getMintFee() public view returns (uint256) {
    return i_mintFee;
}
```

### `getgenTokenUris(uint256 index) public view returns (string memory)`

This view function returns the token URI at the specified index in the array.

```solidity
function getgenTokenUris(uint256 index) public view returns (string memory) {
    return s_genTokenUris[index];
}
```

### `getInitialized() public view returns (bool)`

This view function returns whether the contract has been initialized.

```solidity
function getInitialized() public view returns (bool) {
    return s_initialized;
}
```

### `getTokenCounter() public view returns (uint256)`

This view function returns the current token counter.

```solidity
function getTokenCounter() public view returns (uint256) {
    return s_tokenCounter;
}
```
