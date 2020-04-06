# Developing on DSA
We empower third-party developers to build dapps, use-cases, and other integrations on DSA’s platform. That way, users can get curated experience as per their needs, and developers can build their own businesses supporting those users. This virtuous circle creates new opportunities and benefits users, developers, and protocols.

Our SDK abstract all the complex internal (like handling addresses, ABIs etc) and provide developers with a simple interface to interact with DSA platform.

To get started, install the DSA SDK package from NPM:

```bash
npm install dsa-sdk
```

You can also import the build from CDN:

```js
<script src="https://cdn.jsdelivr.net/npm/dsa-sdk@1.1.12/build/dsa.min.js"></script>
```

For production, we recommend linking to a specific version number and build to avoid unexpected breakage from newer versions.

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

Once connected to a web3 client, check if a user (i.e. EOA address) owns any DSA:

```js
const accounts = await web3.eth.getAccounts()
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
An `Array` of `Object` as shown below:

```json
  [
    {
      id: 52, // DSA Number
      account: "0x...", // DSA Address
      version: 1 // DSA version
    },
    ...
  ]
```

web3 equivalent of the above:
```js
const ABI = [{"anonymous":false,"inputs":[{"indexed":false,"internalType":"address","name":"sender","type":"address"},{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":true,"internalType":"address","name":"account","type":"address"},{"indexed":true,"internalType":"address","name":"origin","type":"address"}],"name":"LogAccountCreated","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"_newAccount","type":"address"},{"indexed":true,"internalType":"address","name":"_connectors","type":"address"},{"indexed":true,"internalType":"address","name":"_check","type":"address"}],"name":"LogNewAccount","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"uint256","name":"accountVersion","type":"uint256"},{"indexed":true,"internalType":"address","name":"check","type":"address"}],"name":"LogNewCheck","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"master","type":"address"}],"name":"LogNewMaster","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"master","type":"address"}],"name":"LogUpdateMaster","type":"event"},{"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"account","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"_newAccount","type":"address"},{"internalType":"address","name":"_connectors","type":"address"},{"internalType":"address","name":"_check","type":"address"}],"name":"addNewAccount","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"_owner","type":"address"},{"internalType":"uint256","name":"accountVersion","type":"uint256"},{"internalType":"address","name":"_origin","type":"address"}],"name":"build","outputs":[{"internalType":"address","name":"_account","type":"address"}],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"_owner","type":"address"},{"internalType":"uint256","name":"accountVersion","type":"uint256"},{"internalType":"address[]","name":"_targets","type":"address[]"},{"internalType":"bytes[]","name":"_datas","type":"bytes[]"},{"internalType":"address","name":"_origin","type":"address"}],"name":"buildWithCast","outputs":[{"internalType":"address","name":"_account","type":"address"}],"stateMutability":"payable","type":"function"},{"inputs":[{"internalType":"uint256","name":"accountVersion","type":"uint256"},{"internalType":"address","name":"_newCheck","type":"address"}],"name":"changeCheck","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"_newMaster","type":"address"}],"name":"changeMaster","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"check","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"connectors","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"version","type":"uint256"},{"internalType":"address","name":"query","type":"address"}],"name":"isClone","outputs":[{"internalType":"bool","name":"result","type":"bool"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"list","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"master","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"_master","type":"address"},{"internalType":"address","name":"_list","type":"address"},{"internalType":"address","name":"_account","type":"address"},{"internalType":"address","name":"_connectors","type":"address"}],"name":"setBasics","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"updateMaster","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"versionCount","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"}]
const address = "0x2971AdFa57b20E5a416aE5a708A8655A9c74f723"
const accounts = await web3.eth.getAccounts()
new web3.eth.Contract(ABI, address);
  .getOwnerDetails(accounts[0])
  .call({ from: "0x..." })
  .then((data) => {
    return data
  })
  .catch((error) => {
    return error
  });
```