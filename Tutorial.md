## Token
![GEITAAmWYAAnDeg](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/f89a5a3e-8a2e-4847-a731-211595026607)

🔍 In cryptocurrency, a token is a digital unit of value, on a blockchain.

It can represent ownership of a decentralized application, digital assets, real world assets, access rights, specific functionality, and much more.

If it has value, it can have a token. 

🏦 In the past, we’ve relied on institutions like banks or governments to keep detailed records when value is transferred.

When you transfer or sell stock, you do it through a bank. When you transfer ownership of a vehicle or real estate, you do so with government records. 

✨ When that value is held on a blockchain instead, tokens are the units that represent it.

Rather than an institution like a bank or government maintaining those records, the blockchain does, & your token is like your deed, or certificate of ownership. 

🧠 Imagine having a key to a vast digital kingdom!

* Utility tokens are just that - they provide access to services or features in a project's ecosystem. They're the gateway to exciting possibilities in dapps & beyond. 🙌 6/14

Here are some examples of utility tokens:

The *ICP token* is a utility token used by the Internet Computer. It serves several functions within the protocol:
* Governance: ICP holders can stake their tokens to participate in the governance of the Internet Computer. This allows them to vote on or submit governance proposals and earn voting rewards.
* Resource Payment: ICP can be converted into cycles, which are used to power computation, communication, and storage costs of canisters (akin to smart contracts). This is part of a "reverse gas" model, where smart contracts pay for their own computation. They must be pre-charged with cycles to run, similar to how an electric car must be pre-charged with electricity to drive.
* Node Provider Payment: ICP tokens are used to remunerate node providers who operate machines running the Internet Computer protocol.
Participation in DAOs: ICP can be used to participate in token swaps of decentralized autonomous organizations (DAOs) on the Internet Computer.
* Medium of Exchange: ICP can also be used as a medium of exchange to pay for goods and services such as NFTs, subscriptions, etc.
The Internet Computer maintains a floating conversion rate so that 1 Trillion cycles costs approximately 1 IMF SDR in ICP. This ensures that end-users of dapps, systems, and services can interact with them over the web without needing tokens to pay for the computations that they initiate.
 
Read more about the tokenomics of the Internet computer here:https://internetcomputer.org/how-it-works/tokenomics


 * 🏦 Security tokens are like digital stocks or bonds.

They represent ownership in assets (like real estate) or earnings (like dividends), & are subject to regulation, just like traditional securities.💲 



## How to mint your tokens
The Bitfinity Testnet supports minting testnet tokens which users can pay for gas fees when deploying their dapps.

We support minting testnet tokens in a number of ways.

* Discord
* Web Interface
* JSON RPC
* Internet Computer Interface

![Screenshot from 2024-01-18 04-03-18](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/f107e812-19e8-43b2-9ab2-486d0ee6c7e9)


## Installation
To get started clone the project
git clone https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM.git
cd hardhat/erc20/

```bash
echo "BITFINITY_PRIVATE_KEY=YOUR_BITFINITY_PRIVATE_KEY_HERE" >> .env
yarn install

```
![Screenshot from 2024-01-18 03-01-10](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/05239040-e00d-47b2-9657-4deac9a53afd)
![Screenshot from 2024-01-18 03-01-34](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/dd109a63-0ea3-4b45-bceb-9c5d4514f50
![Screenshot from 2024-01-18 03-02-07](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/ecd033ac-c104-4d39-a01b-c689b9821eea)
0)
![Screenshot from 2024-01-18 03-02-37](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/f3e62629-7bbf-4aac-9ce4-db06bfd73099)


## Deploying our  ERC-20 Token 
The ERC-20 example is about a native Watermelon token 🍉. You can exchange them into actual Watermelons 🍉🍉🍉. The total supply is 1000000, the minter is the contract deployer address, and the decimals are 0 (One token --> One watermelon).

To deploy the ERC-20 token contract, use the following command:


```bash
$ make deploy NETWORK=testnet_bitfinity
yarn hardhat run scripts/deploy.js --network testnet_bitfinity
yarn run v1.22.10
Deploying contracts with the account: 0x6A33382de9f73B846878a57500d055B981229ac4
Account balance: 2210010200000000000
WatermelonToken deployed to: 0xD7f2A76F5DA173043E6c61a0A18D835809A07766
✨  Done in 14.96s.

# export the token address
$ export TOKEN_ADDRESS='YOUR OUTPUT FROM DEPLOY (e.g. 0xD7f2A76F5DA173043E6c61a0A18D835809A07766)'


```


![Screenshot from 2024-01-18 03-03-09](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/d3a9342d-2d5b-48fe-b51c-4a009a628baf)
![Screenshot from 2024-01-18 03-10-53](https://github.com/Tevin-Isaac/Introduction-to-Bitfinity-EVM/assets/81568615/a141b1a2-c214-4b8d-9f35-4154728bfd98)


## Hardhat Tasks

The following Hardhat task uses the Web3 plugin to get the account’s balance:
```bash
task("balance", "Prints an account's balance")
  .addParam("account", "The account's address")
  .setAction(async (taskArgs) => {
    const account = web3.utils.toChecksumAddress(taskArgs.account);
    const balance = await web3.eth.getBalance(account);

    console.log(web3.utils.fromWei(balance, "ether"), "ETH");
  });


```

To get the ETH balance, use the following command:

```bash
npx hardhat balance --network testnet_bitfinity --account 0x6A33382de9f73B846878a57500d055B981229ac4
2.2100102 ETH

```

You should notice that `--network` is a global built-in option (parameter)
in HardHat. We will use it for the following commands as well.

### Total Supply

The following task script gets the total supply of the Watermelon ERC-20 token.
First it attachs the
token contract, gets the sender address and finaly retrieves the total supply
by calling `totalSupply()` method in our ERC-20 contract. The `--token`
address is the ERC-20 contract address.

```javascript
task("totalSupply", "Total supply of ERC-20 token")
.addParam("token", "Token address")
.setAction(async function ({ token }, { ethers: { getSigners } }, runSuper) {
  const watermelonToken = await ethers.getContractFactory("WatermelonToken")
  const watermelon = watermelonToken.attach(token)
  const [minter] = await ethers.getSigners();
  const totalSupply = (await (await watermelon.connect(minter)).totalSupply()).toNumber()
  console.log(`Total Supply is ${totalSupply}`);
});
```

To get the `totalSupply`, use the following command:

```bash
$ npx hardhat totalSupply --token $TOKEN_ADDRESS --network testnet_bitfinity
Total Supply is 1000000
```

### Transfer ERC-20

The `transfer` option allows anyone holding an ERC-20 tokens to transfer
them to any Ethereum address. In the following script, the minter address
will mint (implicitly) and transfer `10 WTM` tokens to the `spender` address:

```javascript
task('transfer', 'ERC20 transfer')
  .addParam('token', 'Token address')
  .addParam('spender', 'Spender address')
  .addParam('amount', 'Token amount')
  .setAction(async function (
    { token, spender, amount },
    { ethers: { getSigners } },
    runSuper
  ) {
    const watermelonToken = await ethers.getContractFactory('WatermelonToken');
    const watermelon = watermelonToken.attach(token);
    const [minter] = await ethers.getSigners();

    const tx = await watermelon.populateTransaction.transfer(spender, amount);
    const txResponse = await minter.sendTransaction({
      nonce: await minter.getTransactionCount(),
      ...tx,
    });

    await txResponse.wait();

    console.log(
      `${minter.address} has transferred ${amount} tokens to ${spender}`
    );
  });

```

To call `transfer`, use the following command:

```bash
$ npx hardhat transfer --token $TOKEN_ADDRESS --amount 10 --spender 0x2531a4D108619a20ACeE88C4354a50e9aC48ecfe --network testnet_bitfinity
0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 has transferred 10 tokens to 0x2531a4D108619a20ACeE88C4354a50e9aC48ecfe
```

### BalanceOf ERC-20

We can prove that the `spender` has received the exact amount of tokens
by calling the `balanceOf` as shown below:

```javascript
task("balanceOf", "Total supply of ERC-20 token")
.addParam("token", "Token address")
.addParam("account", "Account address")
.setAction(async function ({ token, account }, { ethers: { getSigners } }, runSuper) {
  const watermelonToken = await ethers.getContractFactory("WatermelonToken")
  const watermelon = watermelonToken.attach(token)
  const [minter] = await ethers.getSigners();
  const balance = (await (await watermelon.connect(minter)).balanceOf(account)).toNumber()
  console.log(`Account ${account} has a total token balance:  ${balance} WTM`);
});
```

To get the `balance`, use the following command:

```bash
$ npx hardhat balanceOf --token $TOKEN_ADDRESS --account 0x6A33382de9f73B846878a57500d055B981229ac4 --network testnet_bitfinity
Account 0x6A33382de9f73B846878a57500d055B981229ac4 has a total token balance:  999970 WTM
```

### Approve ERC-20

In some cases, instead of calling the `transfer` directly, the sender
can approve a specific amount of tokens to be withdrawn from his account
to specific recipient address later. This can be done by calling `approve`
then calling `transferFrom`.

```javascript
task('approve', 'ERC20 approve')
  .addParam('token', 'Token address')
  .addParam('spender', 'Spender address')
  .addParam('amount', 'Token amount')
  .setAction(async function (
    { token, spender, amount },
    { ethers: { getSigners } },
    runSuper
  ) {
    const watermelonToken = await ethers.getContractFactory('WatermelonToken');
    const watermelon = watermelonToken.attach(token);
    const [sender] = await ethers.getSigners();
    const tx = await watermelon.populateTransaction.approve(spender, amount);

    const txResponse = await sender.sendTransaction({
      nonce: await sender.getTransactionCount(),
      ...tx,
    });

    await txResponse.wait();
    console.log(
      `${sender.address} has approved ${amount} tokens to ${spender}`
    );
  });

module.exports = {};
```

To call `approve`, use the following command:

```bash
npx hardhat approve --token $TOKEN_ADDRESS --spender 0x8722C88e82AbCC639148Ab6128Cd63333B2Ad771 --amount 10 --network testnet_bitfinity
0x6A33382de9f73B846878a57500d055B981229ac4 has approved 10 tokens to 0x8722C88e82AbCC639148Ab6128Cd63333B2Ad771
```

### TransferFrom ERC-20

After approving the tokens, a recipient can call `transferFrom` to move
the `allowance` to his account.  

```javascript
task('transferFrom', 'ERC20 transferFrom')
  .addParam('token', 'Token address')
  .addParam('sender', 'Sender address')
  .addParam('amount', 'Token amount')
  .setAction(async function (
    { token, sender, amount },
    { ethers: { getSigners } },
    runSuper
  ) {
    const watermelonToken = await ethers.getContractFactory('WatermelonToken');
    const watermelon = watermelonToken.attach(token);
    const [recipient] = await ethers.getSigners();

    const tx = await watermelon.populateTransaction.transferFrom(
      sender,
      recipient.address,
      amount
    );
    const txResponse = await recipient.sendTransaction({
      nonce: await recipient.getTransactionCount(),
      ...tx,
    });

    await txResponse.wait();

    console.log(
      `${recipient.address} has received ${amount} tokens from ${sender}`
    );
  });
```

To call `transferFrom`, use the following command:

```bash
# export the recipient private key
BITFINITY_PRIVATE_KEY="THE RECIPIENT PRIVATE KEY" npx hardhat transferFrom --token $TOKEN_ADDRESS --sender 0x6A33382de9f73B846878a57500d055B981229ac4  --amount 10 --network testnet_bitfinity
0x8722C88e82AbCC639148Ab6128Cd63333B2Ad771 has received 10 tokens from 0x6A33382de9f73B846878a57500d055B981229ac4
```

Checking the balance of `0x8722C88e82AbCC639148Ab6128Cd63333B2Ad771`:

```bash
npx hardhat balanceOf --token $TOKEN_ADDRESS --account 0x8722C88e82AbCC639148Ab6128Cd63333B2Ad771  --network testnet_bitfinity
Account 0x8722C88e82AbCC639148Ab6128Cd63333B2Ad771 has a total token balance:  10 WTM
```

## Conclusion

In this tutorial we deployed an ERC-20 token using HardHat on the Bitfinity
TestNet, transferred, and approved ERC-20 tokens. Moreover, we added other
utility tasks such as getting the total supply, and the account balance.
The only difference is we changed the Ethereum MainNet to the Bitfinity
RPC endpoint.

