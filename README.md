
# Blockchain Wallet API

### Creating a new Blockchain Wallet

Endpoint: `127.0.0.1:3000/api/v2/create`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet api code (required)
  * `priv` - private key to import into wallet as first address (optional)
  * `label` - label to give to the first address generated in the wallet (optional)
  * `email` - email to associate with the newly created wallet (optional)

Sample Response:

```json
{
    "guid": "e560b0f5-0386-401e-9c71-155f525596f2",
    "address": "1NBhiiVJWNiamhnsBw4Nbutxmni9qJTn1Z",
    "warning": "Created non-HD wallet, for privacy and security, it is recommended that new wallets are created with hd=true"
}
```

### Make Payment

Endpoint: `127.0.0.1:3000/merchant/:guid/payment`
e.g 

Query Parameters:

  * `to` - bitcoin address to send to (required)
  * `amount` - amount **in satoshi** to send (required)
  * `password` - main wallet password (required)
  * `second_password` - second wallet password (required, only if second password is enabled)
  * `api_code` - blockchain.info wallet api code (optional)
  * `from` - bitcoin address or account index to send from (optional)
  * `fee` - specify transaction fee **in satoshi**
  * `fee_per_byte` - specify transaction fee-per-byte **in satoshi**

*It is recommended that transaction fees are specified using the `fee_per_byte` parameter, which will compute your final fee based on the size of the transaction. You can also set a static fee using the `fee` parameter, but doing so may result in a low fee-per-byte, leading to longer confirmation times.*

Sample Response:

```json
{
  "to" : ["1A8JiWcwvpY7tAopUkSnGuEYHmzGYfZPiq"],
  "from": ["17p49XUC2fw4Fn53WjZqYAm4APKqhNPEkY"],
  "amounts": [200000],
  "fee": 1000,
  "txid": "f322d01ad784e5deeb25464a5781c3b20971c1863679ca506e702e3e33c18e9c",
  "success": true
}
```


### Send to Many Wallet

Endpoint: `127.0.0.1:3000/merchant/:guid/sendmany`

Query Parameters:

  * `recipients` - a *URI encoded* [JSON object](http://json.org/example.html), with bitcoin addresses as keys and the **satoshi** amounts as values (required, see example below)
  * `password` - main wallet password (required)
  * `second_password` - second wallet password (required, only if second password is enabled)
  * `api_code` - blockchain.info wallet api code (optional)
  * `from` - bitcoin address or account index to send from (optional)
  * `fee` - specify transaction fee **in satoshi**
  * `fee_per_byte` - specify transaction fee-per-byte **in satoshi**

*It is recommended that transaction fees are specified using the `fee_per_byte` parameter, which will compute your final fee based on the size of the transaction. You can also set a static fee using the `fee` parameter, but doing so may result in a low fee-per-byte, leading to longer confirmation times.*

URI Encoding a JSON object in JavaScript:

```js
var myObject = { address1: 10000, address2: 50000 };
var myJSONString = JSON.stringify(myObject);
// `encodeURIComponent` is a global function
var myURIEncodedJSONString = encodeURIComponent(myJSONString);
// use `myURIEncodedJSONString` as the `recipients` parameter
```

Sample Response:

```json
{
  "to" : ["1A8JiWcwvpY7tAopUkSnGuEYHmzGYfZPiq", "18fyqiZzndTxdVo7g9ouRogB4uFj86JJiy"],
  "from": ["17p49XUC2fw4Fn53WjZqYAm4APKqhNPEkY"],
  "amounts": [16000, 5400030],
  "fee": 2000,
  "txid": "f322d01ad784e5deeb25464a5781c3b20971c1863679ca506e702e3e33c18e9c",
  "success": true
}
```

### Fetch Wallet Balance

Endpoint: `127.0.0.1:3000/merchant/:guid/balance`

Query Parameters:

  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet api code (required)

Sample Response:

```json
{ "balance": 10000 }
```


### Fetch Address Balance (deprecated, use the HD API instead)

Endpoint: `127.0.0.1:3000/merchant/:guid/address_balance`

Query Parameters:

  * `address` - address to fetch balance for (required)
  * `password` - main wallet password (required)
  * `api_code` - blockchain.info wallet api code (optional)

Note: unlike the hosted API, there is no option of a `confirmations` parameter for specifying minimum confirmations.

Sample Response:

```json
{ "balance": 129043, "address": "19r7jAbPDtfTKQ9VJpvDzFFxCjUJFKesVZ", "total_received": 53645423 }
```

### Generate Address (deprecated, use the HD API instead)

Endpoint: `127.0.0.1:3000/merchant/:guid/new_address`

Query Parameters:

  * `password` - main wallet password (required)
  * `label` - label to give to the address (optional)
  * `api_code` - blockchain.info wallet api code (optional)

Sample Response:

```json
{ "address" : "18fyqiZzndTxdVo7g9ouRogB4uFj86JJiy" , "label":  "My New Address" }
```


## Steps to run it

  1. Clone this repo using `git clone repo_url`
  2. Run `npm install yarn`
  3. Run `npm install`
  4. Create `.env` file at the project root folder.
  5. Run `yarn start`
  6. Dev server is now running on port 3000
  7. Now check those endpoints using `Postman` using your correct credentials.



### Testing

```sh
$ yarn test
```

### Configuration

Optional parameters can be configured in your `.env` file:

  * `PORT` - port number for running dev server (default: `3000`)
  * `BIND` - ip address to bind the service to (default: `127.0.0.1`)

## Modified by Theejhay&reg;

