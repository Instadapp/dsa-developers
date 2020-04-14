# Resolvers

Resolvers functions for Protocols & balances. These function will provide you all the essential protocol details to move forward with the integration. Details like:-

## Balances

### .getBalances()

Get the balances for the tokens of a specific Ethereum address.

```js
dsa.balances.getBalances(address, type)
  .then(data => {
    return data
  })
  .catch(error => {
    return error
  })
```

### Parameters
`address` (optional) - An ethereum address. Eg:- "0xa7...3423". Default:- DSA address selected at the time of setup.
`type` (optional) - Balances of what type of tokens. Default:- "token". List of types of tokens currently available:-
* "token" - Normal Tokens. Eg:- ETH, DAI, USDC, etc.
* "ctoken" - Compound interest bearing CTokens. Eg:- CETH, CDAI, CUSDC, etc.
* "all" - Get Balances of all the tokens in the list.

### Returns
`List` of tokens, where the key is the symbol and the value is the formatted balance {eth: 2, dai: 50.1}


## MakerDAO

### .getVaults()

Get all the vaults details needed for address in one call.

```js
dsa.balances.getVaults(address)
  .then(data => {
    return data
  })
  .catch(error => {
    return error
  })
```

### Parameters
`address` (optional) - An ethereum address. Eg:- "0xa7...3423". Default:- DSA address selected at the time of setup.

### Returns
`List` of all things Compound. Eg:-
* `owner` - Owner of the Vault. Same as address parameter.
* `colName` - Collateral type. Eg:- ETH-A, BAT-A, USDC-A, etc.
* `token` - Collateral token. Eg:- ETH, BAT, USDC, etc.
* `col` - Collateral Deposited.
* `debt` - DAI Borrowed.
* `liquidatedCol` - Remaining colletral after Vault got liquidated. (unlock collateral - still in vault but not being used as collateral)
* `rate` - Borrow APY.
* `price` - Collateral Token Price from Maker's oracle.
* `status` - Debt / Collateral ratio.
* `liquidation` - Point at which vault will break. When `status` > `liquidation` vault breaks.
* `urn` - Vault Ethereum address.
```js
{ '7903':
  { owner: '0x981C549A74Dc36Bd82fEd9097Bc19404E8db14f3',
    colName: 'ETH-A',
    token: 'ETH',
    col: 0.4,
    debt: 25.001729254489607,
    liquidatedCol: 0,
    rate: 0.5001431422658964,
    price: 154.168734,
    status: 0.4054280106900535,
    liquidation: 0.6666666666666667,
    urn: '0xcC0e5E76eC81aD472D6Df9fC83eaE22E1000Fe53' },
  { ... } ...
}
```

### .getCollateralInfo()

Get all the Collerals details needed in one call.

```js
dsa.balances.getCollateralInfo()
  .then(data => {
    return data
  })
  .catch(error => {
    return error
  })
```

### Returns
`List` of all Maker's Collateral Types. Eg:-
* `ETH-A'` - Collateral Name. Eg:- ETH-A, BAT-A, USDC-A, etc.
* `token` - Collateral token. Eg:- ETH, BAT, USDC, etc.
* `rate` - Borrow APY.
* `price` - Collateral Price from Maker's oracle.
* `ratio` - Liquidation point.
```js
{ 'ETH-A':
  { token: 'ETH',
    rate: 0.5001431422658964,
    price: 154.168734,
    ratio: 0.6666666666666667 }
  { ... } ...
}
```


## Compound Finanace

### .getPosition()

Get all the details needed for Compound integration in one call.

```js
dsa.balances.getBalances(address, key)
  .then(data => {
    return data
  })
  .catch(error => {
    return error
  })
```

### Parameters
`address` (optional) - An ethereum address. Eg:- "0xa7...3423". Default:- DSA address selected at the time of setup.
`key` (optional) - key for the object is it going to return. Default:- "token". Eg:- {eth: {...}} or {ceth: {...}} or {"0x..23": {...}}. List of key available.
* "token" - key as underlying token symbol of ctoken. For ceth it's eth. Eg:- {eth: {...}}
* "ctoken" - key as ctoken symbol. Eg:- {ceth: {...}}
* "address" - key as underlying token address of ctoken. Eg:- {"0x...24": {...}}
* "caddress" - key as ctoken address. Eg:- {"0x...32": {...}}

### Returns
`List` of all things Compound. Eg:-
* `priceInEth` - Tokens price from Compound oracle contracts. Prices are w.r.t "ETH" not "USD".
* `exchangeRate` - CToken to token conversion rate. `CToken balance` * `exchangeRate` = `token balance`.
* `supply` - token balance supplied/deposited.
* `borrow` - token balance borrowed.
* `supplyRate` - Supply APR.
* `supplyYield` - Supply APY.
* `borrowRate` - Borrow APR.
* `borrowYield` - Borrow APY.
* `borrowYield` - Borrow APY.
* `totalSupplyInEth` - Total supply in ETH not in USD. 
* `totalBorrowInEth` - Total borrow in ETH not in USD.
* `maxBorrowLimitInEth` - Max borrow limit in ETH not in USD.
* `status` - `totalBorrowInEth` / `totalSupplyInEth`.
* `liquidation` - `maxBorrowLimitInEth` / `totalSupplyInEth`.
```js
{ eth:
  { priceInEth: 1,
    exchangeRate: 200117674.2320281,
    supply: 0.20000024558102408,
    borrow: 0,
    supplyRate: 0.0098989117776,
    supplyYield: 0.009899400394752789,
    borrowRate: 2.05355820729888,
    borrowYield: 2.0747298274359505 },
  { ... } ...,
  totalSupplyInEth: 0.20000024558102408,
  totalBorrowInEth: 0.08513266988268917,
  maxBorrowLimitInEth: 0.15000018418576805,
  status: 0.4256628267398813,
  liquidation: 0.7499999999999999
}
```
