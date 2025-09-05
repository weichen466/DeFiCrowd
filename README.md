# DeFiCrowd - Decentralized Crowdfunding Platform

## Overview

This project is a decentralized fundraising (crowdfunding) smart contract built on Ethereum using Hardhat development framework. The [`FundMe`](contracts/FundMe.sol) contract allows users to contribute ETH to a fundraising campaign, with built-in mechanisms for either successful fund withdrawal by the owner or refunds to contributors if the funding target is not met.

## Features

- **Decentralized Fundraising**: Users can contribute ETH to the fundraising campaign
- **Price Oracle Integration**: Uses Chainlink Price Feeds to convert ETH to USD for minimum contribution requirements
- **Time-locked Campaign**: Fundraising window with configurable lock time
- **Target-based Logic**: Campaign succeeds if target amount is reached within the time window
- **Automated Refunds**: Contributors can claim refunds if target is not met after the lock period
- **Owner Controls**: Only the contract owner can withdraw funds upon successful campaign completion
- **Comprehensive Testing**: Unit tests and staging tests for all contract functionality

## Project Structure

```
├── contracts/
│   ├── FundMe.sol                    # Main fundraising smart contract
│   └── mocks/
│       └── MockV3Aggregator.sol      # Mock Chainlink price feed for testing
├── deploy/
│   ├── 00-deploy-mocks.js           # Deploy mock contracts for local testing
│   └── 01-deploy-fund-me.js         # Deploy FundMe contract
├── scripts/
│   └── deploy.js                    # Deployment script
├── tasks/
│   ├── deploy-fundme.js             # Hardhat task for deployment
│   ├── interact-fundme.js           # Hardhat task for contract interaction
│   └── index.js                     # Task exports
├── test/
│   ├── staging/
│   │   └── FundMe.staging.test.js   # Integration tests for testnet
│   └── unit/
│       └── FundMe.test.js           # Unit tests for contract functions
├── utils/
│   └── verify.js                    # Contract verification utilities
├── hardhat.config.js                # Hardhat configuration
└── helper-hardhat-config.js         # Network configuration helper
```

## Contract Functionality

### Core Functions

1. **[`fund()`](contracts/FundMe.sol:40)**: Allows users to contribute ETH to the campaign

   - Minimum contribution: $100 USD equivalent (converted using Chainlink price feed)
   - Only available during the fundraising window (before lock time expires)

2. **[`getFund()`](contracts/FundMe.sol:67)**: Owner function to withdraw all funds

   - Only callable by contract owner
   - Requires fundraising window to be closed
   - Target amount must be reached ($1000 USD equivalent)

3. **[`refund()`](contracts/FundMe.sol:87)**: Contributors can claim refunds
   - Available after fundraising window closes
   - Only if target amount was not reached
   - Refunds full contribution amount

### Key Parameters

- **Minimum Contribution**: $100 USD equivalent in ETH
- **Target Amount**: $1000 USD equivalent in ETH
- **Lock Time**: 180 seconds (configurable)
- **Price Feed**: Chainlink ETH/USD oracle for price conversion

## Setup and Installation

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn package manager
- Git

### Installation Steps

1. Clone the repository:

```bash
git clone <repository-url>
cd lesson-5
```

2. Install dependencies:

```bash
npm install
# or
yarn install
```

3. Set up environment variables:
   Create a `.env` file in the root directory with:

```env
SEPOLIA_URL=<your-sepolia-rpc-url>
PRIVATE_KEY=<your-private-key>
PRIVATE_KEY_1=<second-private-key>
ETHERSCAN_API_KEY=<your-etherscan-api-key>
```

## Usage

### Local Development

1. Start a local Hardhat network:

```bash
npx hardhat node
```

2. Deploy contracts to local network:

```bash
npx hardhat deploy --network localhost
```

3. Run tests:

```bash
npx hardhat test
```

### Testnet Deployment (Sepolia)

1. Deploy to Sepolia testnet:

```bash
npx hardhat deploy --network sepolia
```

2. Verify contract on Etherscan:

```bash
npx hardhat verify --network sepolia <contract-address> <lock-time> <price-feed-address>
```

### Hardhat Tasks

The project includes custom Hardhat tasks for easier interaction:

- **Deploy FundMe**: `npx hardhat deploy-fundme`
- **Interact with FundMe**: `npx hardhat interact-fundme`

## Testing

The project includes comprehensive test suites:

### Unit Tests

- Owner validation
- Price feed integration
- Fund function validation (window open/closed, minimum amounts)
- Get fund functionality (owner only, target reached, window closed)
- Refund mechanism (target not reached, contributor validation)

### Staging Tests

- Integration tests for testnet deployment
- Real price feed integration tests

Run all tests:

```bash
npx hardhat test
```

Run specific test suite:

```bash
npx hardhat test test/unit/FundMe.test.js
```

## Network Configuration

The project supports multiple networks:

- **Local/Hardhat**: For development and testing
- **Sepolia**: Ethereum testnet for staging
- **Mainnet**: Production Ethereum (configure as needed)

Network-specific configurations are managed in [`helper-hardhat-config.js`](helper-hardhat-config.js).

## Security Considerations

- **Access Control**: Only contract owner can withdraw funds
- **Reentrancy Protection**: Uses checks-effects-interactions pattern
- **Input Validation**: Comprehensive require statements for all functions
- **Price Oracle**: Reliable Chainlink price feeds for USD conversion
- **Time Locks**: Fundraising window prevents premature withdrawals

## Events

The contract emits two key events:

- **FundWithdrawByOwner**: When owner successfully withdraws funds
- **RefundByFunder**: When a contributor claims a refund

## Dependencies

- **Hardhat**: Ethereum development environment
- **@chainlink/contracts**: Chainlink oracle interfaces
- **@nomicfoundation/hardhat-toolbox**: Hardhat plugin collection
- **ethers.js**: Ethereum library for blockchain interaction
- **hardhat-deploy**: Deployment management plugin

## License

This project is licensed under the MIT License.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit a pull request

## Support

For questions or issues, please open an issue in the repository or contact the development team.
