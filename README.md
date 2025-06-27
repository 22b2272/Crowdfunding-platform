# Crowdfunding-platform
# Blockchain Crowdfunding Platform

A step-by-step implementation of a decentralized crowdfunding platform using Ethereum smart contracts, covering fundamental blockchain concepts from Week 1 to Week 4.

![Crowdfunding Demo](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📋 Project Overview
This project implements a Solidity smart contract for a decentralized crowdfunding platform, where:
- Campaign creators can set funding goals and deadlines
- Contributors can pledge ETH to campaigns
- Funds are released only if goals are met
- Refunds are available if goals aren't met

## 🛠️ Tech Stack
- **Blockchain**: Ethereum
- **Smart Contracts**: Solidity (0.8.0)
- **Development**: Hardhat (Testing), Web3.py (Python integration)
- **Frontend**: (Optional) React/Next.js

## 📅 Weekly Progress

### Week 1: Introduction to Blockchain & Ethereum
**Tasks Completed**:
- Set up development environment (MetaMask, Remix/Hardhat)
- Learned basic Solidity syntax
- Created and deployed a "Hello World" smart contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract HelloWorld {
    string public greeting;
    
    constructor() {
        greeting = "Hello, World!";
    }
    
    function setGreeting(string memory _greeting) public {
        greeting = _greeting;
    }
}




Week 2: Campaign Structure & Storage
Tasks Completed:

Defined Campaign struct with core properties

Implemented campaign creation logic

Used mappings for campaign storage

solidity
struct Campaign {
    address owner;
    string title;
    uint256 goal;
    uint256 deadline;
    uint256 totalFunds;
}

mapping(uint256 => Campaign) public campaigns;
uint256 public campaignCount;

function createCampaign(string memory _title, uint256 _goal, uint256 _days) public {
    campaignCount++;
    campaigns[campaignCount] = Campaign({
        owner: msg.sender,
        title: _title,
        goal: _goal,
        deadline: block.timestamp + (_days * 1 days),
        totalFunds: 0
    });
}
Week 3: Contribution Logic
Tasks Completed:

Implemented ETH contribution system

Tracked contributors and amounts

Used msg.sender and msg.value securely

solidity
struct Contribution {
    address contributor;
    uint256 amount;
}

mapping(uint256 => Contribution[]) public contributions;

function contribute(uint256 _campaignId) public payable {
    require(_campaignId <= campaignCount, "Invalid campaign");
    Campaign storage c = campaigns[_campaignId];
    require(block.timestamp < c.deadline, "Campaign ended");
    
    c.totalFunds += msg.value;
    contributions[_campaignId].push(Contribution(msg.sender, msg.value));
}
Week 4: Funding & Refund Logic
Tasks Completed:

Added goal-based fund release

Implemented automatic refunds

Used time-based conditions with block.timestamp

solidity
function withdrawFunds(uint256 _campaignId) public {
    Campaign storage c = campaigns[_campaignId];
    require(msg.sender == c.owner, "Only owner");
    require(block.timestamp >= c.deadline, "Not ended");
    require(c.totalFunds >= c.goal, "Goal not reached");
    
    payable(msg.sender).transfer(c.totalFunds);
}

function claimRefund(uint256 _campaignId) public {
    Campaign storage c = campaigns[_campaignId];
    require(block.timestamp >= c.deadline, "Not ended");
    require(c.totalFunds < c.goal, "Goal reached");
    
    // Refund logic here
}
🚀 Getting Started
Prerequisites
Node.js (v16+)

Python (for Web3.py integration)

MetaMask (for testing)

Installation
Clone the repo:

bash
git clone https://github.com/yourusername/blockchain-crowdfunding.git
Install dependencies:

bash
npm install
pip install web3 py-solc-x
Running Tests
bash
npx hardhat test
📂 Project Structure
text
├── contracts/           # Solidity smart contracts
│   ├── Crowdfunding.sol # Main contract (Week 2-4)
│   └── HelloWorld.sol   # Week 1 contract
├── tests/               # Test scripts
├── scripts/             # Deployment scripts
└── README.md            # This file

🌟 Features
Create crowdfunding campaigns

Contribute ETH to campaigns

Automatic fund release/refund

Time-based campaign expiration




