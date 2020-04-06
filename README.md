# Developing on DSA
We empower third-party developers to build dapps, use-cases, and other integrations on DSA’s platform. That way, users can get curated experience as per their needs, and developers can build their own businesses supporting those users. This virtuous circle creates new opportunities and benefits users, developers, and protocols.

## Index
* [Get Started](#get-started)
* Get Accounts
* Set Instance
* Build DSA
* Cast Spell

## Get Started

To get started, install the DSA SDK package from NPM:

```bash
npm install dsa-sdk
```

You can also import the build from CDN:

```js
<script src="https://cdn.jsdelivr.net/npm/dsa-sdk@1.2.0/build/dsa.min.js"></script>
```

For production, we recommend linking to a specific version number ([jsdeliver](https://www.jsdelivr.com/package/npm/dsa-sdk)).

Now instantiate Web3 and DSA.

```js
const dsa = new DSA()
if (window.ethereum) {
  window.web3 = new Web3(window.ethereum)
} else if (window.web3) {
  window.web3 = new Web3(window.web3.currentProvider)
} else {
  console.log('Non-Ethereum browser detected. You should consider trying MetaMask!')
}
```


## Get Accounts

Once connected to a web3 client, get all the DSA where a specific address is authorised.

```js
let accounts = await web3.eth.getAccounts()
dsa.getAccounts(accounts[0])
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
    account: "0x...", // DSA Address
    version: 1 // DSA version
  },
  ...
]
```

Web3 equivalent of the above (return value might have different format):
```js
let ABI = [link](https://github.com/InstaDApp/dsa-sdk/blob/master/src/abi/resolvers/core.json)
let contract = "0xD6fB4fd8b595d0A1dE727C35fe6F1D4aE5B60F51"
let accounts = await web3.eth.getAccounts()
new web3.eth.Contract(ABI, contract);
  .getOwnerDetails(accounts[0])
  .call({ from: "0x..." })
  .then((data) => {
    return data
  })
  .catch((error) => {
    return error
  });
```


## Set Instance

(Optional) Once you get the DSA(s), set some common values so you don't have to pass similar arguments in further calls.

```js
let accounts = await web3.eth.getAccounts();
let dsaAccount = dsa.getAccounts(accounts[0])
dsa.setInstance({
  id: dsaAccount[0].id,
  account: dsaAccount[0].account,
  origin: "0x..." // optional
})
```

### Parameters
1. `Object`
   * `id` - `Number`: The number of DSA.
   * `account` - `String`: The address of DSA.
   * `origin` - `String`: The address to track the transaction origination (affiliates).


## Build DSA

Build a new DSA.

```js
dsa.build()
  .then(data => {
    return data
  })
  .catch(error => {
    return error
  })
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

Web3 equivalent of the above (return value might have different format):
```js
let ABI = [link](https://github.com/InstaDApp/dsa-sdk/blob/master/src/abi/core/index.json)
let contract = "0xD6fB4fd8b595d0A1dE727C35fe6F1D4aE5B60F51"
let accounts = await web3.eth.getAccounts()
let owner = "0x..."
let version = 1;
let origin = "0x..."
new web3.eth.Contract(ABI, contract).methods
  .build(owner, version, origin)
  .send({
    from: accounts[0]
  })
  .on("transactionHash", (txHash) => {
    resolve(txHash);
  })
  .on("error", (err) => {
    reject(err);
  });
```

Our team is super active to assist you with all of your queries at our [TG developer group](https://t.me/instadevelopers) or [discord channel](https://discord.gg/83vvrnY).