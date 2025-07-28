# Autheo Reward Distribution

A Solidity smart contract for managing and distributing rewards to various stakeholders in the Autheo ecosystem.

## Overview

The Autheo Reward Distribution contract manages the fair distribution of rewards across different user categories:

- Bug bounty participants (low, medium, and high severity)
- Smart contract developers
- dApp users

The contract enables precise tracking and distribution of rewards based on predefined allocation percentages and eligibility criteria.

## Features

- **Multiple Reward Categories**: Support for different reward types with specific allocation percentages
- **Security Features**: Built with ReentrancyGuard, Pausable functionality, and ownership controls
- **Registration System**: Admin-controlled registration for different user categories
- **Uptime Bonuses**: Additional rewards for dApp users with good uptime metrics
- **Emergency Controls**: Pause functionality and emergency withdraw options

## Token Allocation

| Category | Percentage | Description |
|----------|------------|-------------|
| Bug Bounty | 60% | For security researchers who identify vulnerabilities |
| dApp Rewards | 4% | For active users of Autheo dApps |
| Developer Rewards | 2% | For smart contract developers |

## Bug Bounty Structure

Bug bounty rewards are further divided based on vulnerability severity:

- Low severity: 5% of bug bounty allocation
- Medium severity: 35% of bug bounty allocation
- High severity: 60% of bug bounty allocation

## Reward Amounts

- **Monthly dApp Reward**: 5,000 tokens
- **Monthly Uptime Bonus**: 500 tokens (awarded to users with more than three smart contracts deployed and more than fifteen transactions)
- **Developer Deployment Reward**: 1,500 tokens (monthly)

## Contract Usage

### Prerequisites

- Autheo token address must be provided during contract deployment
- Owner account has administrative privileges

### Deployment

```solidity
constructor(address _autheoToken)
```

### Administrative Functions

```solidity
// Set authorized addresses
function setAuthorized(address _address, bool _status) external onlyOwner

// Toggle testnet status
function setTestnetStatus(bool _status) external onlyOwner

// Set claim amount for contract deployers
function setClaimPerContractDeployer(uint256 _claimAmount) external onlyOwner

// Set claim amount for dApp users
function setClaimPerDappUser(uint256 _claimAmount) external onlyOwner

// Register bug bounty users by severity level
function registerLowBugBountyUsers(address[] calldata _lowBugBountyUsers) external onlyOwner
function registerMediumBugBountyUsers(address[] calldata _mediumBugBountyUsers) external onlyOwner
function registerHighBugBountyUsers(address[] calldata _highBugBountyUsers) external onlyOwner

// Register contract deployment users
function registerContractDeploymentUsers(address[] calldata _contractDeploymentUsers) external onlyOwner

// Register dApp users and their uptime status
function registerDappUsers(address[] calldata _dappRewardsUsers, bool[] calldata _userUptime) external onlyOwner
```

### User Functions

```solidity
// Claim rewards (must be authorized and when testnet is inactive)
function claimReward(bool _contractDeploymentClaim, bool _dappUserClaim, bool _bugBountyClaim) external
```

### View Functions

```solidity
// Calculate remaining claimed amount
function calculateRemainingClaimedAmount() external view returns (uint256)

// Get all whitelisted contract deployment users
function getWhitelistedContractDeploymentUsers() external view returns (address[] memory)

// Get contract balance
function getBalance() external view returns (uint256)

// Calculate remaining contract deployment reward allocation
function calculateRemainingContractDeploymentReward() external view returns (uint256)

// Get all whitelisted dApp reward users
function getWhitelistedDappRewardUsers() external view returns (address[] memory)

// Calculate remaining dApp rewards allocation
function calculateRemainingDappRewards() external view returns (uint256)

// Get all registered users across all categories
function getAllUsers() external view returns (address[] memory)

// Get current deployment multiplier for a user
function getCurrentDeploymentMultiplier(address _user) public view returns (uint256)
```

### Emergency Controls

```solidity
// Pause contract
function pause() external onlyOwner

// Unpause contract
function unpause() external onlyOwner

// Emergency withdraw any accidentally sent tokens
function emergencyWithdraw(address token) external onlyOwner

// Withdraw native tokens from contract
function withdraw() external onlyOwner
```

## Events

- `WhitelistUpdated`: Emitted when a user is added to a whitelist
- `Claimed`: Emitted when a user claims their rewards
- `ClaimAmountUpdated`: Emitted when claim amounts are updated
- `EmergencyWithdraw`: Emitted during emergency token withdrawals
- `TestnetStatusUpdated`: Emitted when testnet status changes
- `Received`: Emitted when the contract receives native tokens
- `Withdrawal`: Emitted when native tokens are withdrawn from the contract

## Security Features

- **ReentrancyGuard**: Protection against reentrancy attacks
- **Pausable**: Ability to pause contract functionality in emergency situations
- **Ownable**: Access control for administrative functions
- **SafeERC20**: Safe token transfer operations
- **Custom Error Handling**: Specific error messages for better debugging

## Dependencies

- OpenZeppelin Contracts v4.x:
  - `IERC20.sol`
  - `IERC20Metadata.sol`
  - `SafeERC20.sol`
  - `Ownable.sol`
  - `ReentrancyGuard.sol`
  - `Pausable.sol`
  - `Strings.sol`

## License

SPDX-License-Identifier: MIT