# Unlocking-the-Future-of-Gaming
Building Avalanche - ETH + AVAX 
---
## Description

For the Degen Gaming platform on the Avalanche network, DegenToken is an ERC20 token. Players can use the token to obtain awards, give tokens to others, and exchange tokens for in-game goods sold in the platform's store. Based on the OpenZeppelin library, this smart contract has functionality including token minting (for the platform owner), token transfers, and token burning. Players can also use their tokens to purchase in-game things from the store.
---
## Getting Started

### Installation Steps

Git-clone the repository into your local machines.
---
### Interacting with the program

Steps: 
```
1. Unleash the contract onto the Ethereal Realm.
2. Forge new tokens, bestowing them upon valiant players as trophies.
3. Players can harness the arcane `transferTokens` spell to pass their tokens to fellow adventurers.
4. Eager adventurers can invoke the `redeem` incantation to exchange tokens for rare in-game artifacts.
5. When the time comes, tokens can be consumed in the sacred fire of the `burn` ritual, freeing their essence back to the cosmos.
```
---
## Code: 
```
/*
Challenge
Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. 
The smart contract should have the following functionality:

Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only owner can mint tokens.
Transferring tokens: Players should be able to transfer their tokens to others.
Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
Checking token balance: Players should be able to check their token balance at any time.
Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

// All necessary imports
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "hardhat/console.sol";

contract DegenToken is ERC20, Ownable {
    constructor() ERC20("Degen", "DGN") {}

    // Store item structure
    struct StoreItem {
        string itemName;
        uint256 price;
    }

    // Store items list
    StoreItem[] public storeItems;

    // Add store items (onlyOwner)
    function addStoreItem(string memory itemName, uint256 price) external onlyOwner {
        storeItems.push(StoreItem(itemName, price));
    }

    // Mint new tokens and distribute them to players as rewards. Only the owner can do this.
    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    // Transfer tokens from the sender's account to another account.
    function transferTokens(address to, uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Not enough Degen Tokens");
        _transfer(msg.sender, to, amount);
    }

    // Redeem tokens for items in the in-game store based on the store item index.
    event RedeemItem(address indexed player, string itemName, uint256 price);

    function redeem(uint256 itemIndex) public virtual {
        require(itemIndex < storeItems.length, "Invalid store item index");
        uint256 price = storeItems[itemIndex].price;
        require(balanceOf(msg.sender) >= price, "Not enough Degen Tokens to redeem this item");

        _burn(msg.sender, price);
        emit RedeemItem(msg.sender, storeItems[itemIndex].itemName, price);
    }

    // Anyone can burn their own tokens that are no longer needed.
    function burn(uint256 amount) public virtual {
        _burn(msg.sender, amount);
    }
}

contract StoreInitializer {
    // Deploy the DegenToken contract and add examples to storeItems
    constructor() {
        DegenToken token = new DegenToken();

        // Some of the storeItems
        token.addStoreItem("Enchanted Amulet of Wisdom", 150);
        token.addStoreItem("Elixir of Eternal Vitality", 75);
        token.addStoreItem("Celestial Orb of Protection", 250);
    }
}

```
---
## Author

Pranav Gupta
Contact: [Email](mailto:pentest.pranav@gmail.com)
---
## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
