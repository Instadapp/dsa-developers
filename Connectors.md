# Connectors
Connectors are used for the interaction between DSA and Protocols / other contracts. Connectors are called by the help of `cast` function in DSA.

Note: Most of the connectors function have 2 extra parameters as `setId` and `getId`. They will be used to set/store data from one connector and use it in another connector which will make cross protocol transaction possible without the need of solidity.


## Basics

Used to do basic operations like depositing and withdrawing tokens from DSA.

Connector address - `0x9370236a085A99Aa359f4bD2f0424b8c3bf25C99`

### deposit()

To Deposit ETH/tokens into the DSA. There are two ways to deposit in DSA:-
1. Normal ETH/Token transfer to DSA address.
2. Give `Allowance` of Tokens to DSA and deposit by using the below function.

```js
let spells = dsa.Spell();
spells.add({
 connector: "basic", // name
 method: "deposit", // method
 args: ["token_address", "amount", "getId", "uint setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token to deposit. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token to deposit. Eg:- if user wants to deposit `1` ETH then `amount` will be `1000000000000000000`.
`getId` (uint) - Used to get data stored in particular Id. If not set then it'll return `0`. If not needed then send `0`.
`setId` (uint) - Used to set data stored in particular Id. If not needed then send `0`.


### withdraw()

To Withdraw ETH/tokens from the DSA.

```js
let spells = dsa.Spell();
spells.add({
 connector: "basic", // name
 method: "withdraw", // method
 args: ["token_address", "amount", "withdraw_address", "getId", "uint setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token to withdraw. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token to withdraw. Eg:- if user wants to withdraw `1` ETH then `amount` will be `1000000000000000000`.
`withdraw_address` (address) - Address on which tokens needs to be withdrawn. Note:- `Address should be an Authority`.
`getId` (uint) - Used to get data stored in particular Id. If not set then it'll return `0`. By default keep it `0`.
`setId` (uint) - Used to set data stored in particular Id. If not needed then send `0`.


## Auth

Used to add/remove authority from DSA.

Connector address - `0x627cd0DbD5eE33F8456Aa8143aCd68a13d641588`

### addModule()

To Add Auth module (Authority) - an address. It can be Owner, Guardian, manager, etc.

```js
let spells = dsa.Spell();
spells.add({
 connector: "auth", // name
 method: "addModule", // method
 args: ["authority"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`authority` (address) - Address of new authority.


### removeModule()

To remove Auth module (Authority) - an address.

```js
let spells = dsa.Spell();
spells.add({
 connector: "auth", // name
 method: "removeModule", // method
 args: ["authority"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`authority` (address) - Address of authority to remove.


## Compound

For Compound Protocol interaction. Eg:- deposit, borrow, etc.

Connector address - `0x547f1508A2a1AB0cB84DCe4b3e09beB560Bb44Cb`

### deposit()

To deposit/supply tokens into Compound as Lending or Collateral.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "deposit", // method
 args: ["token_address", "amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token. Send `"-1"` to deposit 100%.
`getId` (uint) - Id will be used to get the amount of token to deposit.
`setId` (uint) - Id will be used to set the amount of token deposited.


### withdraw()

To withdraw tokens from Compound that you've supplied.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "withdraw", // method
 args: ["token_address", "amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token. Send `"-1"` to withdraw 100%.
`getId` (uint) - Id will be used to get the amount of token to withdraw.
`setId` (uint) - Id will be used to set the amount of token withdrawn.


### borrow()

To borrow tokens from Compound.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "borrow", // method
 args: ["token_address", "amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token.
`getId` (uint) - Id will be used to get the amount of token to borrow.
`setId` (uint) - Id will be used to set the amount of token borrowed.


### payback()

To payback tokens from Compound.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "payback", // method
 args: ["token_address", "amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token. Send `"-1"` to payback 100%.
`getId` (uint) - Id will be used to get the amount of token to payback.
`setId` (uint) - Id will be used to set the amount of token payed back.


### Extras functions

### depositCToken()

To deposit/supply tokens into Compound as Lending or Collateral. The only difference between this and deposit is it'll set in CToken amount.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "depositCToken", // method
 args: ["token_address", "amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token. Eg:- if user wants to deposit `1` ETH then `amount` will be `1000000000000000000`.
`getId` (uint) - Id will be used to get the amount of token to deposit.
`setId` (uint) - Id will be used to set the amount of ctoken minted.

### withdrawCToken()

To withdraw tokens from Compound. The only difference between this and withdraw amount and getId.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "withdrawCToken", // method
 args: ["token_address", "ctoken_amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`ctoken_amount` (uint) - Amount of token to withdraw in terms of CToken. Eg:- if user wants to deposit `1` ETH then `amount` will be `1000000000000000000`.
`getId` (uint) - Id will be used to get the amount of ctoken to withdraw. (You'll recieve tokens)
`setId` (uint) - Id will be used to set the amount of token withdrawn.


### paybackBehalf()

To payback tokens on behalf of someone else.

```js
let spells = dsa.Spell();
spells.add({
 connector: "compound", // name
 method: "paybackBehalf", // method
 args: ["borrower", "token_address", "amount", "getId", "setId"] // method arguments
});
dsa.cast(spells);
```

### Parameters
`borrower` (address) - Address of which Ctokens needs to be payed back.
`token_address` (address) - Address of token. You can get tokens details from `dsa.tokens.info`.
`amount` (uint) - Amount of token. Send `"-1"` to payback 100%.
`getId` (uint) - Id will be used to get the amount of token to payback.
`setId` (uint) - Id will be used to set the amount of token payed back.
