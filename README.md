# web3project


Here's a project idea that you can build using **Ethers.js**: 

### **Project: Crypto Wallet and Token Transfer Application**

This project will allow users to connect their Ethereum wallet (e.g., MetaMask), view their balance, and send ERC-20 tokens to another address. You'll interact with the Ethereum blockchain using **Ethers.js**.

### **Features**:
- **Connect Wallet**: Connect to MetaMask or any Web3 wallet.
- **View Wallet Balance**: Display Ethereum balance and ERC-20 token balances.
- **Send Tokens**: Enable users to send ERC-20 tokens to a different Ethereum address.
- **Transaction History**: Display the list of recent transactions (using `ethers.js` to fetch data from the blockchain).
  
### **Tech Stack**:
- **React** (Frontend framework)
- **TypeScript** (For type safety)
- **Ethers.js** (For interacting with the Ethereum blockchain)
- **MetaMask** (For wallet connection)

### **Steps**:

#### 1. **Set Up React Project**
Create a new React project with TypeScript.
```bash
npx create-react-app crypto-wallet --template typescript
cd crypto-wallet
```

#### 2. **Install Ethers.js**
Install **Ethers.js** to interact with Ethereum.
```bash
npm install ethers
```

#### 3. **Connect to MetaMask**
Create a component that connects to MetaMask when the user clicks a "Connect Wallet" button.

```tsx
import React, { useState } from "react";
import { ethers } from "ethers";

const WalletConnect: React.FC = () => {
  const [address, setAddress] = useState<string | null>(null);
  const [provider, setProvider] = useState<ethers.providers.Web3Provider | null>(null);

  const connectWallet = async () => {
    if (window.ethereum) {
      const _provider = new ethers.providers.Web3Provider(window.ethereum);
      const _signer = _provider.getSigner();
      const _address = await _signer.getAddress();
      setProvider(_provider);
      setAddress(_address);
    } else {
      alert("Please install MetaMask!");
    }
  };

  return (
    <div>
      {address ? (
        <p>Connected as: {address}</p>
      ) : (
        <button onClick={connectWallet}>Connect Wallet</button>
      )}
    </div>
  );
};

export default WalletConnect;
```

#### 4. **Get Wallet Balance**
Add a function to fetch the Ethereum balance of the connected wallet.

```tsx
import React, { useState, useEffect } from "react";
import { ethers } from "ethers";

const WalletBalance: React.FC<{ provider: ethers.providers.Web3Provider }> = ({ provider }) => {
  const [balance, setBalance] = useState<string>("");

  useEffect(() => {
    const fetchBalance = async () => {
      const signer = provider.getSigner();
      const _balance = await signer.getBalance();
      setBalance(ethers.utils.formatEther(_balance));
    };
    fetchBalance();
  }, [provider]);

  return <p>Balance: {balance} ETH</p>;
};

export default WalletBalance;
```

#### 5. **Send Tokens (ERC-20 Transfer)**
Add functionality to send ERC-20 tokens from one wallet to another.

First, you’ll need a basic ERC-20 contract address (e.g., USDT or any token). For this example, we’ll use an ERC-20 token contract ABI (Application Binary Interface) to interact with it.

```tsx
const sendTokens = async (toAddress: string, amount: string) => {
  if (!provider) return;

  const signer = provider.getSigner();
  const tokenAddress = "YOUR_ERC20_TOKEN_ADDRESS"; // Replace with actual token address
  const tokenABI = [
    "function transfer(address recipient, uint256 amount) public returns (bool)",
  ];
  
  const tokenContract = new ethers.Contract(tokenAddress, tokenABI, signer);
  
  try {
    const tx = await tokenContract.transfer(toAddress, ethers.utils.parseUnits(amount, 18)); // 18 decimals for most ERC-20 tokens
    await tx.wait();
    alert("Tokens sent!");
  } catch (error) {
    console.error(error);
    alert("Transaction failed");
  }
};
```

You can call this function in your component, adding an input for the recipient address and token amount.

#### 6. **Transaction History**
To fetch transaction history, you can use **Ethers.js** to get the wallet's past transactions.

```tsx
const fetchTransactionHistory = async (address: string) => {
  const txs = await provider.getHistory(address);
  return txs;
};
```

This will allow you to fetch the transaction history for the connected wallet and display it in your frontend.

### **Final File Structure:**
- `src/`
  - `components/`
    - `WalletConnect.tsx`
    - `WalletBalance.tsx`
    - `SendTokens.tsx`
    - `TransactionHistory.tsx`

### **Resources**:
- **Ethers.js Documentation**: [Ethers.js Docs](https://docs.ethers.io/v5/)
- **ERC-20 Token Standard**: [ERC-20 Standard](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/)
- **MetaMask Documentation**: [MetaMask Docs](https://metamask.io/docs/)

### **Running the App**:
1. Run `npm start` to launch the app locally.
2. Ensure you have MetaMask installed in your browser.
3. Click "Connect Wallet" to connect with MetaMask.
4. View your balance and send tokens from your connected wallet.

This project will help you understand how to connect to the Ethereum blockchain, interact with smart contracts, and perform blockchain transactions using **Ethers.js**. Let me know if you need more specific help on any step!
