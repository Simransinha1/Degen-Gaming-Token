# Degen Gaming Token

This Solidity program is a smart contract that implements an ERC20 token called "Degen" (DGN) for Degen Gaming. The contract allows for the creation of new tokens, token transfers, token redemption for in-game items, checking token balances, and burning tokens that are no longer needed.

## Functionality

The Degen Gaming Token contract provides the following functionality:

1. *Minting new tokens:* The contract owner can mint new tokens and distribute them to players as rewards.

2. *Transferring tokens:* Players can transfer their tokens to others.

3. *Redeeming tokens:* Players can redeem their tokens for items in the in-game store. The available items and their costs are as follows:
   - M416 Glacier : 400 DGN
   - Pharaoh X Suit : 900 DGN
   - McLaren 570S : 1500 DGN

4. *Checking token balance:* Players can check their token balance at any time.

5. *Burning tokens:* Anyone can burn tokens they own, which are no longer needed.

## Deployment

To deploy the Degen Gaming Token contract on the Avalanche network's Fuji Testnet, follow these steps:

1. Make sure you have a compatible development environment set up, such as Hardhat.


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "hardhat/console.sol";

contract DegenToken is ERC20, Ownable {
    address private _owner;
    address  public minter = 0xb6B5ed54Dc1faBEBfe3358B7F77A287DE9E359d2;
    string public rewards = "1 --> M416 Glacier : 400, 2 -->  Pharaoh X Suit  :  900, 3 --> McLaren 570S :  1500";
    constructor() ERC20("Degen", "DGN") {
        _owner = msg.sender;
    }

    function mint(address account, uint256 amount) public onlyOwner {
        _mint(account, amount);
    }
    
    function burn(uint256 amount) public{
        _burn(msg.sender, amount);
    }

    function transfer(address receiver, uint256 amount) public override returns (bool) {
        require(amount <= balanceOf(msg.sender), "Insufficient balance of tokens");
        _transfer(_owner, receiver, amount);
        return true;
    }

    function choose_item(uint256 item_num) external payable {
        redeem(item_num);
        }

    function redeem(uint256 item_num) public payable {
        require(item_num >= 1 && item_num <= 3, "Invalid item number");

        if (item_num == 1) {
            require(balanceOf(msg.sender) >= 400,"Insufficient balance for M14");
            _transfer(msg.sender, minter, 400);
            console.log("M416 Glacier transfered to your account");
        } 
        else if (item_num == 2) {
            require(
                balanceOf(msg.sender) >= 900,
                "Insufficient balance for pan"
            );
            _transfer(msg.sender, minter, 900);
            console.log("Pharaoh X Suit transfered to your account");

           
        } else if (item_num == 3) {
            require(
                balanceOf(msg.sender) >= 1500,
                "Insufficient balance for  Jeep"
            );
            
            _transfer(msg.sender , minter, 1500);
            console.log(" McLaren 570S transfered to your account");

            
        }

        
    }
  
}

```


2. Copy and paste the provided code into a new Solidity file (e.g., `DegenToken.sol`).

3. In the contract constructor, modify the token name and symbol as desired.

4. Compile the contract using your development environment.

5. Configure your development environment to connect to the Avalanche network's Fuji Testnet. Refer to the documentation or tutorials provided by your development environment for instructions on how to configure the network.

6. Deploy the contract using your development environment, specifying the account that will be the owner of the contract.

7. Take note of the deployed contract address, as it will be needed for interacting with the contract.

## Interacting with the Contract

Once the Degen Gaming Token contract is deployed, players can interact with it using various methods. Here are some examples:

### Minting New Tokens

Only the contract owner can mint new tokens. To mint tokens for a specific player, call the `mint` function, specifying the player's address and the amount of tokens to mint.

### Transferring Tokens

Players can transfer their tokens to others by calling the `transfer` function, specifying the recipient's address and the amount of tokens to transfer.

### Redeeming Tokens

To redeem tokens for in-game items, players should follow these steps:

1. Call the `Show_item` function to view the available items and their costs.

2. Choose an item number (1 for Iron sword, 2 for Golden Sword, 3 for Diamond sword) and call the `choose_item` function, passing the chosen item number as an argument.

### Checking Token Balance

Players can check their token balance by calling the `balanceOf` function, passing their address as an argument.

### Burning Tokens

To burn tokens that are no longer needed, call the `burn` function, specifying the amount of tokens to burn.



## Authors
Simran Kumari



[EMAIL](21BCS3832@cuchd.in)

## License

This project is licensed under the MIT License. See the [LICENSE.md](https://license.md/) file for details.
