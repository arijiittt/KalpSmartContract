# 🚀 Build a Frontend to Interact with Your Deployed Smart Contract on the Kalp Blockchain

## Airdrop Machine Challenge

---

## What is this challenge about?

This challenge involves developing a frontend application that interacts with a smart contract deployed on the Kalp blockchain. You'll use **React** and **TypeScript** to build a user interface that communicates with your smart contract via API endpoints generated by Kalp Studio. The goal is to simulate real-world scenarios where decentralized applications (dApps) require a frontend to allow users to interact with blockchain smart contracts seamlessly.

## What will you learn?

**By participating in this challenge, you will:**

- Gain hands-on experience with **React** and **TypeScript**.
- Understand how to interact with blockchain smart contracts from a frontend application.
- Learn how to deploy smart contracts on the **Kalp blockchain** and generate API endpoints.
- Enhance your skills in building decentralized applications (**dApps**).
- Explore concepts such as **API integration**, **state management**, and **asynchronous operations** in React.
- Improve your problem-solving and coding abilities in a practical environment.

---

## Ready to Get Started?

### Let's understand connecting a frontend to the blockchain first!

Imagine you're using a web application that allows you to check your bank balance, transfer money, or view transaction history. Behind the scenes, this application communicates with a backend server to perform these operations. In the world of blockchain, we replace the traditional backend with **smart contracts** deployed on a blockchain network.

Connecting a frontend application to a blockchain involves interacting with smart contracts through API endpoints or blockchain nodes. This enables users to perform actions like claiming tokens, checking balances, or transferring assets directly from the web interface.

---

## Checkpoint 0: 📦 Installation

#### Before you begin, ensure you have the following:

- **Step 1: 🖥 [Download Node.js and npm](https://nodejs.org/en/download/)**
  - Ensure you have **Node.js** version `>=14.x` and **npm** version `>=6.x`.

---

**To start the project, follow these steps:**

1. **Clone the repository:**

   ```sh
   git clone https://github.com/Build-Hackathon/junior.git
   ```

2. **Navigate to the project directory:**

   ```sh
   cd frontend
   ```

3. **Install the dependencies:**

   ```sh
   npm install
   ```

---

#### After executing the above commands, your folder structure should look like this:

```
kalp-dapp-frontend
├── src
│   ├── app
│   │   ├── favicon.ico
│   │   ├── globals.css
│   │   └── page.tsx
│   ├── hooks
│   │   └── useKalpApi.ts   (Your API hook)
│   ├── pages
├── public
├── package.json
├── tsconfig.json
└── README.md
```

---

## Checkpoint 1: 🏗 Frontend Code Walkthrough

Let's **open the file `useKalpApi.ts`** and dive deep into how the frontend interacts with the Kalp blockchain.

### 1. Setting Up the API Hook

The **`useKalpApi`** hook encapsulates all the API calls to your smart contract. It manages loading states, errors, and provides functions to interact with the contract.

```typescript
import { useState } from 'react';

export const useKalpApi = () => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<any>(null);

  const apiKey = process.env.NEXT_PUBLIC_API_KEY;

  // ... rest of the code
};
```

- **State Management:**

  - `loading`: Indicates whether an API call is in progress.
  - `error`: Holds any error that occurs during API calls.

- **API Key:**

  - `apiKey`: Your Kalp Studio API key stored in environment variables for security.

### 2. Making API Calls

The **`callApi`** function handles making HTTP requests to the Kalp blockchain API.

```typescript
const callApi = async (endpoint: string, args: { [key: string]: any }) => {
  setError(null);
  setLoading(true);

  const params = {
    network: 'TESTNET',
    blockchain: 'KALP',
    walletAddress: 'your-wallet-address',
    args: args,
  };

  try {
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey!,
      },
      body: JSON.stringify(params),
    });

    const data = await response.json();

    if (!response.ok) {
      throw new Error(data.message || 'Something went wrong');
    }

    setLoading(false);
    return data;
  } catch (err: any) {
    setError(err);
    setLoading(false);
    throw err;
  }
};
```

- **Parameters:**

  - `endpoint (string)`: The API endpoint URL.
  - `args (object)`: The arguments to pass to the smart contract function.

- **API Request:**

  - Sends a POST request with the required headers and body.
  - Handles the response and error states.

### 3. Interacting with Smart Contract Functions

#### a) Claim Tokens

```typescript
const claim = async (address: string) => {
  const endpoint = 'https://your-api-endpoint/Claim';
  const args = {
    amount: 100,
    address: address,
  };
  return callApi(endpoint, args);
};
```

- **Purpose:** Allows a user to claim tokens by calling the `Claim` function of your smart contract.

#### b) Check Balance

```typescript
const balanceOf = async (account: string) => {
  const endpoint = 'https://your-api-endpoint/BalanceOf';
  const args = {
    account: account,
  };
  return callApi(endpoint, args);
};
```

- **Purpose:** Retrieves the token balance of a specific account.

#### c) Get Total Supply

```typescript
const totalSupply = async () => {
  const endpoint = 'https://your-api-endpoint/TotalSupply';
  const args = {};
  return callApi(endpoint, args);
};
```

- **Purpose:** Gets the total supply of tokens from your smart contract.

### 4. Returning the API Functions

```typescript
return { claim, balanceOf, totalSupply, loading, error };
```

- **Exports:** The API functions and state variables for use in your components.

---

## Checkpoint 2: 📀 Deploying the Smart Contract and Generating API Endpoints

Before connecting your frontend to the smart contract, you need to deploy your smart contract on the Kalp blockchain and obtain your API endpoints.

#### Steps:

1. **Sign Up and Log In to Kalp Studio Platform**

   - Follow the guide: [Sign Up and Log In to Kalp Studio Platform](https://docs.kalp.studio/Getting-started/Onboarding/How-to-Sign-Up-and-Log-In-to-Kalp-Studio-Platform/)

2. **Deploy Your Smart Contract**

   - Use the smart contract code provided in the previous challenge or your own.
   - Follow the guide: [Deploy a Smart Contract on Kalp Studio](https://docs.kalp.studio/Dev-documentation/Kalp-DLT/Smart-Contract-Write-Test-Deploy-Interact/Deploy-the-smart-contract/)

3. **Generate API Endpoints**

   - After deploying, Kalp Studio will provide you with API endpoints for each function of your smart contract.
   - **Example:**

     ![API Endpoint Example](images/api-endpoint-example.png)

4. **Generate Your API Key**

   - In Kalp Studio, navigate to the API key generation section.
   - Generate a new API key to authenticate your API requests.
   - **Example:**

     ![Generate API Key](images/generate-api-key.png)

---

## Checkpoint 3: 🔧 Configuring the Frontend with Your API Endpoints

Now that you have your API endpoints and API key, you need to replace the placeholder endpoints and configure your API key in the frontend code.

### 1. Update Environment Variables

- **Create a `.env.local` file in your project root:**

  ```sh
  touch .env.local
  ```

- **Add your API key to the `.env.local` file:**

  ```env
  NEXT_PUBLIC_API_KEY=your-kalp-api-key
  ```

  - **Note:** Prefixing the variable with `NEXT_PUBLIC_` makes it accessible in the browser.

### 2. Replace API Endpoints in `useKalpApi.ts`

- **Locate the endpoints in your code and replace them with your generated endpoints:**

  ```typescript
  const claim = async (address: string) => {
    const endpoint = 'https://your-api-endpoint/Claim';
    const args = {
      amount: 100,
      address: address,
    };
    return callApi(endpoint, args);
  };

  const balanceOf = async (account: string) => {
    const endpoint = 'https://your-api-endpoint/BalanceOf';
    const args = {
      account: account,
    };
    return callApi(endpoint, args);
  };

  const totalSupply = async () => {
    const endpoint = 'https://your-api-endpoint/TotalSupply';
    const args = {};
    return callApi(endpoint, args);
  };
  ```

- **Replace `https://your-api-endpoint/FunctionName` with the actual endpoints provided by Kalp Studio.**

  - **For example:**

    ```typescript
    const claim = async (address: string) => {
      const endpoint = 'https://gateway-api.kalp.studio/v1/contract/kalp/invoke/YourContractID/Claim';
      const args = {
        amount: 100,
        address: address,
      };
      return callApi(endpoint, args);
    };
    ```

### 3. Update the Wallet Address (if necessary)

- **In the `callApi` function, update the `walletAddress` parameter:**

  ```typescript
  const params = {
    network: 'TESTNET',
    blockchain: 'KALP',
    walletAddress: 'your-wallet-address',
    args: args,
  };
  ```

- **Replace `'your-wallet-address'` with the address you want to use for transactions.**

---

## Checkpoint 4: ▶️ Running the Frontend Application

Now that you've configured your API endpoints and API key, you're ready to run the application.

### Steps:

1. **Start the Development Server:**

   ```sh
   npm run dev
   ```

2. **Open the Application in Your Browser:**

   - Navigate to `http://localhost:3000` in your web browser.

3. **Interact with the Application:**

   - Use the interface to claim tokens, check balances, and view the total supply.
   - The application will communicate with your deployed smart contract via the configured API endpoints.

---

## Conclusion

Congratulations! You've successfully built a frontend application that interacts with your smart contract on the Kalp blockchain. By completing this challenge, you've learned how to:

- Deploy a smart contract and generate API endpoints using Kalp Studio.
- Configure a React frontend to communicate with blockchain APIs.
- Manage state and handle asynchronous API calls in React.
- Securely store and use API keys in a frontend application.

For additional details and advanced configurations, refer to the [Kalp SDK documentation](https://docs.kalp.studio/).

---

**Feel free to explore further functionalities and enhance your frontend application. Consider adding features like user authentication, transaction history, or integrating with wallets for a more comprehensive dApp experience. Good luck, and happy coding!**

---