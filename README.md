# Solana Phantom Wallet Clone - Chrome Extension

A Chrome extension clone of [Phantom Wallet](https://phantom.app/) for Solana blockchain, built for **educational purposes only**.

> **WARNING**: This is a learning project. Do NOT use it with real funds. Always use devnet or testnet.

## Features

### Key Management
- Generate 12-word BIP39 mnemonic seed phrases
- Derive Solana keypairs using BIP44 derivation (`m/44'/501'/0'/0'`)
- Import existing wallets via seed phrase
- Export seed phrase with password verification
- AES-256 encrypted storage with PBKDF2 key derivation

### Wallet Operations
- View SOL and SPL token balances
- Send SOL and SPL tokens
- View transaction history
- Request devnet/testnet airdrops

### Auto-Approve (Jupiter Wallet Style)
- Configurable auto-approve for transactions below threshold
- Priority fee levels: None, Low, Medium, High, Turbo
- Instant transaction sending without confirmations

### Network Support
- Solana Devnet (default)
- Solana Testnet

## Tech Stack

- **Chrome Extension** - Manifest V3
- **React 18** - Popup UI
- **TypeScript** - Type safety
- **Vite** - Build tool
- **Tailwind CSS** - Styling
- **@solana/web3.js** - Blockchain interactions
- **@solana/spl-token** - Token support
- **bip39** - Mnemonic generation
- **crypto-js** - AES encryption

## Installation

### Prerequisites
- Node.js 18+
- npm
- Chrome browser

### Build the Extension

```bash
# Clone the repository
git clone <repository-url>
cd wallet

# Install dependencies
npm install

# Build the extension
npm run build
```

### Load in Chrome

1. Open Chrome and navigate to `chrome://extensions/`
2. Enable **Developer mode** (toggle in top right)
3. Click **Load unpacked**
4. Select the `dist` folder from this project
5. The extension icon will appear in your toolbar

## Project Structure

```
wallet/
├── public/
│   └── manifest.json         # Chrome extension manifest (V3)
├── src/
│   ├── background/
│   │   └── index.ts          # Service worker (wallet logic)
│   ├── popup/
│   │   ├── index.tsx         # Popup entry point
│   │   ├── App.tsx           # Main app component
│   │   ├── styles.css        # Tailwind styles
│   │   └── screens/          # UI screens
│   │       ├── CreateWallet.tsx
│   │       ├── UnlockWallet.tsx
│   │       ├── Dashboard.tsx
│   │       ├── SendScreen.tsx
│   │       └── SettingsScreen.tsx
│   ├── services/
│   │   ├── keyManagement.ts  # Mnemonic & keypair handling
│   │   └── solana.ts         # Solana RPC interactions
│   ├── hooks/
│   │   └── useWallet.ts      # React hook for wallet state
│   ├── utils/
│   │   ├── crypto.ts         # Encryption utilities
│   │   └── storage.ts        # Chrome storage wrapper
│   └── types/
│       └── index.ts          # TypeScript definitions
├── scripts/
│   └── postbuild.js          # Post-build script
├── popup.html                # Extension popup HTML
├── vite.config.ts            # Vite configuration
└── package.json
```

## Architecture

### Chrome Extension Components

1. **Popup** (`popup.html` + React)
   - User interface displayed when clicking extension icon
   - Communicates with background service worker via messages

2. **Background Service Worker** (`background/index.ts`)
   - Handles all wallet operations (key management, transactions)
   - Maintains in-memory keypair when unlocked
   - Processes messages from popup

3. **Storage** (`chrome.storage.local`)
   - Encrypted wallet data
   - Settings and preferences

### Message Flow

```
Popup UI  →  chrome.runtime.sendMessage()  →  Background Service Worker
    ↑                                               ↓
    ←───────────  sendResponse()  ←─────────────────
```

## Usage Guide

### Creating a New Wallet
1. Click the extension icon
2. Click "Create New Wallet"
3. Create a strong password
4. **Write down the 12-word seed phrase** (never share it!)
5. Verify 3 random words
6. Your wallet is ready

### Importing an Existing Wallet
1. Click "Import Wallet"
2. Enter your seed phrase
3. Create a password
4. Wallet imported

### Getting Devnet SOL
1. Unlock your wallet
2. Click "Airdrop" button
3. Receive 1 SOL on devnet

### Sending SOL
1. Click "Send"
2. Enter recipient address
3. Enter amount
4. Select priority fee
5. Click "Send"

## Auto-Approve Feature

Enable in Settings to automatically send transactions below your threshold:
- **Max Amount**: Maximum SOL for auto-approve
- **Priority Level**: Default fee level for auto-approved transactions

## Security Notes

### What This Project Does
- Encrypts seed phrases with AES-256 in chrome.storage
- Uses PBKDF2 with 100,000 iterations
- Never stores plaintext seeds or passwords
- Service worker clears keypair on lock

### Production Wallets Should Also
- Use Web Crypto API (more secure)
- Implement hardware wallet support
- Add 2FA/biometric authentication
- Use secure enclave if available
- Implement proper audit logging
- Add transaction simulation

### Best Practices
1. **Never share your seed phrase**
2. **Write it on paper, not digitally**
3. **Use a strong password**
4. **Only use devnet/testnet**
5. **Clear extension data after testing**

## Development

### Watch Mode
```bash
npm run dev
```
Rebuilds on file changes. Reload extension in Chrome to see updates.

### Type Checking
```bash
npm run type-check
```

### Clean Build
```bash
npm run clean
npm run build
```

## Troubleshooting

### Extension not loading
- Ensure you selected the `dist` folder, not `src`
- Check Chrome developer console for errors
- Verify manifest.json is in dist folder

### Balance not showing
- Check you're on devnet (green badge)
- Try clicking refresh button
- Check Chrome extension errors: `chrome://extensions/` → Errors

### Airdrop failing
- Devnet has rate limits, wait and retry
- Check Solana devnet status

### Transactions failing
- Ensure sufficient balance for fees
- Check recipient address is valid
- Try increasing priority fee

## API Reference

### useWallet Hook

```typescript
const {
  // State
  isInitialized,  // Wallet exists
  isLocked,       // Wallet locked
  isLoading,      // Operation pending
  publicKey,      // Wallet address
  balance,        // SOL balance
  tokens,         // Token accounts
  transactions,   // Recent txs
  cluster,        // Network
  autoApprove,    // Auto-approve settings

  // Actions
  createWallet,   // Create new wallet
  importWallet,   // Import from mnemonic
  unlock,         // Unlock wallet
  lock,           // Lock wallet
  sendSol,        // Send SOL
  sendToken,      // Send SPL token
  requestAirdrop, // Get devnet SOL
  refreshBalance, // Refresh data
  setCluster,     // Change network
  setAutoApprove, // Update settings
  exportMnemonic, // Export seed
} = useWallet();
```

## License

MIT License - Educational use only.

## Disclaimer

This software is for educational purposes only. The authors are not responsible for any loss of funds. Do not use with real cryptocurrency.

## Acknowledgments

- [Phantom Wallet](https://phantom.app/)
- [Jupiter Wallet](https://jup.ag/)
- [Solana Web3.js](https://solana-labs.github.io/solana-web3.js/)
