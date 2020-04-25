
# How to Interact With DSA using Web3
DeFi Smart Accounts (DSA) are contract accounts  **trustlessly owned by the users**. Briefly, DSA is designed to allow developers to build extensible products and business models on top of DeFi with maximum security and composability.

**Our platform is open to all sizes of developers, from solo-coders to a group of hackers to globally-scaled teams**. Build compelling use cases and monetize their models to earn money by serving your users with high reliability.

## Prerequisite
-   [web3](https://github.com/ethereum/web3.js/#installation) installed.
-   Instantiating web3
```javascript
// in browser
if (window.ethereum) {
  window.web3 = new Web3(window.ethereum)
} else if (window.web3) {
  window.web3 = new Web3(window.web3.currentProvider)
} else {
  window.web3 = new Web3(customProvider)
}

// in node.js
const web3 = new Web3(new Web3.providers.HttpProvider(ETH_NODE_URL))
```
## Creating new DSA.

-   Index Contract is the core contract of DSA and is a factory contract to deploy new DSA for users.

### .build()

  ` function build(address _owner, uint accountVersion, address _origin)`

| Parameter | Type | Description|
|--|--|--|
| `_owner` | address | Owner of the new DSA.
| `accountVersion` | uint | Version of the new DSA.
| `_origin` | address | Referral Address.

**Web3 Code Snippet to create new DSA.**
```javascript
let indexAddress = "0x2971adfa57b20e5a416ae5a708a8655a9c74f723"
let indexABI = [] // paste ABI from `https://github.com/InstaDApp/dsa-sdk/blob/master/src/abi/core/index.json`
let owner = "0x...."; // Owner of the DSA.
let version = 1; // Version of DSA.
let origin = "0x...."; // Referral Address for creating DSA.
new web3.eth.Contract(indexABI, indexAddress).methods
  .build(owner, version, origin)
  .send({
    from: owner
  })
  .on("transactionHash", (txHash) => {
    return txHash;
  })
  .on("error", (error) => {
    return error;
  });
```

## Interacting with Your DSA.

 Once DSA is created, You can interact with your DSA.

### .cast()
  `function cast(address[] calldata _targets, bytes[] calldata _datas, address _origin)`

| Parameter | Type | Description|
|--|--|--|
| `_targets` | address[] | Array of connector’s target addresses.
| `_datas` | bytes[] | Array of connector’s function callData.
| `_origin` | address | Referral Address.


**Web3 Code Snippet to interact with DSA.**

> Deposit Asset in Compound Protocol and start earning Interest.
```javascript 	
let DepositLogic = { // Desposit Function ABI of compound connector.
        - inputs: 
        - [
            - {
                - internalType: "address",
                - name: "token",
                - type: "address"
            - },
            - {
                - internalType: "uint256",
                - name: "amt",
                - type: "uint256"
            - },
            - {
                - internalType: "uint256",
                - name: "getId",
                - type: "uint256"
            - },
            - {
                - internalType: "uint256",
                - name: "setId",
                - type: "uint256"
            - }
        - ],
        - name: "deposit",
        - outputs: [ ],
        - stateMutability: "payable",
        - type: "function"
};
// Address of compound Connector.
let CompoundAddress = "0x547f1508a2a1ab0cb84dce4b3e09beb560bb44cb"; 

let DSA_Address = "YOUR_DSA_ADDRESS" // Your DSA Address.
let DSA_ABI = ["Copy and paste ABI here"] // ABI of DSA Contract.

// You need to create callData of a specific function of connector.
let depositArgs = [
    "TOKEN_ADDRESS", // address of the token to deposit.
    "TOKEN_AMOUNT", // amount to deposit(Amount should be in wei).
    "0", // It should be zero
    "0" // It should be zero
  ];

let depositCallData = web3.eth.abi.encodeFunctionCall(DepositLogic, depositArgs);

// Interacting with DSA.
new web3.eth.Contract(DSA_ABI, DSA_Address).methods
  .cast(
    [CompoundAddress], // need to be in array format
    [depositArgs] // need to be in array format
  ).send({
    from: address[0]
  })
  .on("transactionHash", (txHash) => {
    return txHash;
  })
  .on("error", (error) => {
    return error;
  });
```
**Note:** We use a mock address to indicate ETH in connectors: `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`

To interact with other Protocols using DSA, here are the available connector[link-to-connectors.].
