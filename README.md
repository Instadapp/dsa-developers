# Developing on DSA
We empower third-party developers to build DeFi dapps, use-cases, and other integrations on DSA’s platform. That way, users can get curated experience as per their needs, and developers can build their own businesses supporting those users. This virtuous circle creates new opportunities and benefits users, developers, and protocols.

[Refer here for an overview of DSAs](Overview.md)

Our team is super active to assist with your queries at our [TG developer group](https://t.me/instadevelopers) or [discord channel](https://discord.gg/83vvrnY).

## Get Started

**Node**
To get started, install the DSA SDK package from NPM:

```bash
npm install dsa-sdk
```

**For Browser**

 via jsDelivr CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/dsa-sdk@1.2.3/build/dsa.js"></script>
```

For production, we recommend linking to a specific version number ([jsdeliver](https://www.jsdelivr.com/package/npm/dsa-sdk)).

## Usage
Now instantiate DSA.
```js
// in node.js
const dsa = require('dsa-sdk');

const dsa = new DSA();
```

DSA only works with web3 library so you also have to instantiate [Web3](https://github.com/ethereum/web3.js/#installation).

```js
if (window.ethereum) {
  window.web3 = new Web3(window.ethereum)
} else if (window.web3) {
  window.web3 = new Web3(window.web3.currentProvider)
} else {
  window.web3 = new Web3(customProvider)
}
```

## Get Accounts

Once connected to a web3 client, get all the DSA where a specific address is authorised.

```js
let address = "0x...";
dsa.getAccounts(address)
  .then(data => {
    return data
  })
  .catch(error => {
    return error
  })
```

### Parameters
`address` - An ethereum address.

### Returns
`Array` of `Object` with all the DSA where `address` parameter is authorised.

```js
[
  {
    id: 52, // DSA Number
    address: "0x...", // DSA Address
    version: 1 // DSA version
  },
  ...
]
```

Web3 equivalent of the above (return value might have different format):
```js
let ABI = []; // => https://github.com/InstaDApp/dsa-sdk/blob/master/src/abi/resolvers/core.json
let contract = "0xD6fB4fd8b595d0A1dE727C35fe6F1D4aE5B60F51";
let account = "0x...";
new web3.eth.Contract(ABI, contract);
  .getOwnerDetails(account)
  .call({ from: "0x..." })
  .then((data) => {
    return data
  })
  .catch((error) => {
    return error
  });
```


## Set Instance

Once you get the DSA(s), set some common values so you don't have to pass similar arguments in further calls.

```js
let address = "0x...";
let dsaAccount = dsa.getAccounts(address);
dsa.setInstance({
  id: dsaAccount[0].id,
  address: dsaAccount[0].account,
  origin: "0x..." // optional
})
```

### Parameters
1. `Object`
   * `id` - `Number`: The number of DSA.
   * `address` - `String`: The address of DSA.
   * `origin` - `String`: The address to track the transaction origination (affiliates).


## Build DSA

Build a new DSA.

```js
dsa.build()
  .then(data => {
    return data // transaction hash
  })
  .catch(error => {
    return error
  });
```

### Parameters
1. `Object` (ALL optional)
   * `owner`: The authorised address which will be authorised on DSA (defaulted to selected address).
   * `origin` - `String`: The address to track the transaction origination (affiliates).
   * `from` - `String`: The address transactions should be made from (defaulted to selected address).
   * `gasPrice` - `String`: The gas price in wei to use for transactions.
   * `gas` - `Number`: The maximum gas provided for a transaction (gas limit).

### Returns
`String`: Transaction hash `0x.....`.

Web3 equivalent of the above:
```js
let ABI = [] // => https://github.com/InstaDApp/dsa-sdk/blob/master/src/abi/core/index.json
let contract = "0xD6fB4fd8b595d0A1dE727C35fe6F1D4aE5B60F51";
let address = await web3.eth.getAccounts();
let owner = "0x...";
let version = 1;
let origin = "0x...";
new web3.eth.Contract(ABI, contract).methods
  .build(owner, version, origin)
  .send({
    from: address[0]
  })
  .on("transactionHash", (txHash) => {
    return txHash;
  })
  .on("error", (error) => {
    return error;
  });
```

## Interact with DSA

Once the DSA is build, use the following function to make calls to your DSA. This is where you'll interact with other smart contracts like DeFi protocols.

Create a new instance.
```js
let spells = dsa.Spell();
```

Add the series of transactions details in the instance.
```js
spells.add({
 connector: "basic", // name
 method: "deposit", // method
 args: [dsa.token.usdc.address, 1000000, 0, 1] // method arguments
});

spells.add({
 connector: "basic",
 method: "withdraw",
 args: [dsa.token.usdc.address, 0, "0x03d70891b8994feB6ccA7022B25c32be92ee3725", 1, 0]
});
```

Note: You can get the specific input interface by calling `dsa.getInterface(connector, method)`

Send the transaction to blockchain. CAST YOUR SPELL.

```js
dsa.cast(spells) // or dsa.cast({spells:spells})
  .then(data => {
    return data // transaction hash
  })
  .catch(error => {
    return error
  });
```

### Parameters
1. `Instance` - The spell instance.
OR
1. `Object`
   * `spells` - The spell instance.
   * `from` - `String` (optional): The address transactions should be made from (defaulted to selected address).
   * `gasPrice` - `String` (optional): The gas price in wei to use for transactions.
   * `gas` - `Number` (optional): The maximum gas provided for a transaction (gas limit).

### Returns
`String`: Transaction hash `0x.....`.

Web3 equivalent of the above:
```js
let ABI = []; // => https://github.com/InstaDApp/dsa-sdk/blob/master/src/abi/core/account.json
let contract = "0x..."; // DSA address
let origin = "0x...";
let address = await web3.eth.getAccounts();
new web3.eth.Contract(ABI, contract).methods
  .cast(
    ["0x...", "0x..."], // Array of target addresses
    ["0x......", "0x......"], // Array of encoded function call, check encodeFunctionCall
    origin
  )
  .send({
    from: address[0]
  })
  .on("transactionHash", (txHash) => {
    return txHash;
  })
  .on("error", (error) => {
    return error;
  });
```

Our team is super active to assist with your queries at our [TG developer group](https://t.me/instadevelopers) or [discord channel](https://discord.gg/83vvrnY).
