

🏰 DeFi Empire: A Simple DeFi Kingdom Clone on Avalanche 🌐

🔎 Overview

DeFi Empire is a decentralized finance (DeFi) 🌟 application built as a clone of the popular DeFi Kingdom game 🎮. Leveraging the Avalanche blockchain ⛓️ and its custom EVM subnets, this project allows users to engage in activities such as collecting 🛡️, battling ⚔️, and trading 💱 digital assets, earning rewards 🏆 through gameplay. The game integrates smart contracts 📜 deployed on a custom subnet to ensure low fees 💰 and high scalability 🚀, making it an engaging blockchain-based experience.

✨ Features

🛠️ Custom EVM subnet on Avalanche

💰 Deployment of ERC20 token for in-game currency

📦 Vault contract for staking and withdrawing assets

🔗 Integration with MetaMask for seamless user interaction

🎲 Fully customizable gameplay features including battling ⚔️, exploring 🌍, and trading 💱

🛠️ Tools and Technologies

Avalanche CLI: For deploying and managing EVM subnets

Solidity: Smart contract development 📜

Remix IDE: For deploying and testing smart contracts

MetaMask: To interact with the blockchain 🔗

Web Browser: For using Remix and interacting with MetaMask 🌐

🚀 Project Setup

Step 1: Deploy the EVM Subnet

Use the Avalanche CLI 🛠️ to deploy a custom EVM subnet.

Define your native currency 💵 for in-game transactions.

Step 2: Add Subnet to MetaMask

Open MetaMask 🔗 and configure the network settings to connect to your subnet.

Ensure the subnet is selected as the active network ✅.

Step 3: Connect Remix to MetaMask

Use the "Injected Provider" option in Remix 🛠️ to link it to your MetaMask wallet.

Step 4: Deploy Smart Contracts

ERC20 Token Contract:

Implements functionality for minting 🪙, burning 🔥, transferring 🔄, and approving tokens ✅.

Vault Contract:

Enables depositing 📥 and withdrawing 📤 tokens using a share-based system 📊.

Deploy both contracts through Remix using your configured MetaMask wallet.

Step 5: Test the Application 🧪

Interact with the deployed contracts in Remix 🛠️.

Perform operations like minting tokens 🪙, transferring tokens 🔄, depositing into the vault 📥, and withdrawing from the vault 📤.

📜 Smart Contract Details

🪙 ERC20 Token Contract

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "DeFi Empire Token";
    string public symbol = "DFET";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint amount) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}

📦 Vault Contract

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint _amount) external {
        uint shares = (totalSupply == 0) ? _amount : (_amount * totalSupply) / token.balanceOf(address(this));
        totalSupply += shares;
        balanceOf[msg.sender] += shares;
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        totalSupply -= _shares;
        balanceOf[msg.sender] -= _shares;
        token.transfer(msg.sender, amount);
    }
}

✅ Submission Requirements

GitHub Repository:

Include all source code with a detailed README.md file 📄.

Provide a public GitHub link 🔗.

Video Walkthrough:

Record a video 🎥 explaining your code and demonstrating its functionality.

Ensure the video is no longer than 5 minutes ⏱️.

Transaction/Application ID:

Provide the transaction or application ID 🆔 for the deployment.

📚 Resources

Avalanche Documentation


