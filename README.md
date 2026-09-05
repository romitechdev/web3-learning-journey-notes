# 📘 Web3 Learning Progress - Day 1

## 🧠 Material Learned

- **Web3** is the concept of the next generation of the internet that is decentralized.
- Data and control are not dominated by a single party (such as large corporations), but are distributed across the user network.
- One of the core technologies of Web3 is **Blockchain**, a system for recording transaction data securely, transparently, and immutably.
- Blockchain stores data on many computers (nodes), ensuring network security and resilience.

## 💻 Today's Practice

### Trying Out a Blockchain Demo
I tried visiting [Blockchain Demo](https://blockchaindemo.io/) for a visual blockchain simulation.

## 📜 First Smart Contract: A Digital Coffee Machine

### 🧩 Basic Concept

I started by creating a simple analogy: **a smart contract-based digital coffee machine**. This concept helps understand how business logic can be automated on top of a blockchain.

### 💡 How It Works:
- A user sends a certain amount of ether as payment.
- If the amount sent matches or exceeds the price of the coffee (e.g., `1 ether`), then the coffee is "dispensed" (simulated via an event or token).
- If the amount is less than required, the smart contract triggers an error and does not process the transaction.
- The money is automatically refunded if the condition is not met.



### 🔗 Link to Try It Out:
👉 [Try it on Remix Ethereum IDE](https://remix.ethereum.org/)

### 🧑‍💻 Complete Solidity Code Example:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoffeeMachine {
    uint256 public price = 1 ether;

    event CoffeeDispensed(address indexed buyer);

    function buyCoffee() public payable {
        require(msg.value >= price, "Uang tidak cukup!");

        emit CoffeeDispensed(msg.sender);
    }

    function refund() public {
        payable(msg.sender).transfer(address(this).balance);
    }
}
```

Here is how the smart contract looks in Remix IDE:

![Smart Contract Coffee Machine](day1/coffee-contract.png)

---


# 📘 Web3 Learning Progress - Day 2

## 🧠 Material Learned
- Created a simple ERC-20 token using Solidity.
- Learned the concepts of `totalSupply`, `balanceOf`, `transfer`, and the `Transfer` event.
- Simulated deploying and using the smart contract on [Remix IDE](https://remix.ethereum.org/).

---

## 🚀 KentangCoin Project 🥔

### 💡 Description
Today I created a token named **KentangCoin (KENTANG)** based on the **ERC-20** standard on the Ethereum network. This token has basic features:
- Initial total supply: **1,000,000 KENTANG**
- Wallet-to-wallet transfer function
- Event log every time a transfer occurs

---

## 📄 Simulation Steps in Remix IDE

### 1. Open Remix IDE
👉 Access: [https://remix.ethereum.org/](https://remix.ethereum.org/)

### 2. Create a New File
- Click the **File Explorer** icon on the left.
- Click the **+** button to create a new file.
- File name: `KentangCoin.sol`

### 3. Copy the Smart Contract Code
Copy the following code into the `KentangCoin.sol` file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
}

contract KentangCoin is IERC20 {
    string public constant name = "KentangCoin";
    string public constant symbol = "KENTANG";
    uint8 public constant decimals = 18;

    uint256 private _totalSupply = 1_000_000 * (10 ** uint256(decimals));
    mapping(address => uint256) private _balances;

    constructor() {
        _balances[msg.sender] = _totalSupply;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(_balances[msg.sender] >= amount, "Saldo tidak mencukupi");
        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }
}
```

### 4. Compile the Smart Contract
- Select the **Solidity Compiler** tab in the left panel.
- Click **Compile KentangCoin.sol**.
- Make sure there are no errors.

### 5. Deploy the Smart Contract
- Select the **Deploy & Run Transactions** tab.
- In the **Environment** section, select **JavaScript VM** (local simulation).
- Click **Deploy**.

> After a successful deployment, the contract will automatically assign all tokens (1,000,000 KENTANG) to the first account.

### 6. Check the Account Balance
- Under the **Deployed Contracts** section, click the **balanceOf** button.
- Enter the deployer account address (e.g., `0x5AEDA56215b84a05ff784d9e7f3af3E3c3fd9cf7`) → click **call**.
- A large value like `1000000000000000000000000` should appear.

### 7. Test the Transfer Function
- Click the **transfer** button.
- Enter:
  - `_to`: destination address (e.g., `0xAbc...`)
  - `_amount`: number of tokens to send (e.g., `1000000000000000000000` = 1,000 KENTANG)
- Click **transact**

> If successful, you can see the `Transfer` event appear in the Remix console.

### 8. Check the Destination Balance
- Use the `balanceOf` function again for the destination address.
- The balance should have increased by the amount transferred.

---

## 🖼️ Screenshot of Deployment Results

![KentangCoin Deployment Result on Remix](day2/kentangcoin.png)

---

## 📌 Important Notes
- Currently, tokens can only be transferred by the owner.
- Approval functions (`approve`, `allowance`) are not yet available — they will be created on subsequent days.
- For now, all tokens are distributed to the deployer account via the constructor.
