---
sidebar_position: 2
---

# RPCh ethers adapter

## Description

The RPCh ethers adaptor is an extension of the original `JsonRpcProvider`, which allows clients to use drop-in and replace so that they can send their RPC requests through the RPCh network.

## How to use RPCh ethers adaptor
You must have Node.js and npm/yarn installed on your computer. You can download them from their official website or use a package manager like Homebrew (for Mac) or Chocolatey (for Windows).

```
yarn add @rpch/crypto @rpch/ethers
```

You can create an instance of this adaptor by passing in the required options and key-value store functions:
```TypeScript
import * as RPChCrypto from "@rpch/crypto";
import { RPChProvider } from "@rpch/ethers";

const PROVIDER_URL = 'https://primary.gnosis-chain.rpc.hoprtech.net';
const TIMEOUT = 10000;
const DISCOVERY_PLATFORM_API_ENDPOINT = 'https://staging.discovery.rpch.tech';

const provider = new RPChProvider(
  PROVIDER_URL,
  {
    crypto: RPChCrypto,
    client: 'trial',
    timeout: TIMEOUT,
    discoveryPlatformApiEndpoint: DISCOVERY_PLATFORM_API_ENDPOINT,
  },
 setKeyValFunction,
 getKeyValFunction
);
```

- PROVIDER_URL: a string representing the URL of the provider we want to connect to.
- HoprSdkOps: an object containing the options for the RPCh SDK instance. It includes:
  - crypto: The RPChCrypto module is required for cryptographic operations. Learn more about what module to pass [here](./RPCh-crypto.md)
  - client: A string that identifies the client using the SDK. This is used for statistics and logging.
  - timeout: The timeout for requests in milliseconds.
  - discoveryPlatformApiEndpoint: The URL for the discovery platform API.
- setKeyVal: a function that sets a key-value pair in storage.
- getKeyVal: a function that retrieves the value corresponding to a key from storage.

Once we have constructed the RPChProvider instance, we can use it to send requests to the provider using the send method. For example, to get the current block number, we can call: 
```TypeScript
provider.send('eth_blockNumber', [])
```

## Example Integrations

### BlockWallet

BlockWallet is a crypto wallet, and the following example is an integration of RPCh into its browser extension. The integrated repository can be found [here.](https://github.com/Rpc-h/extension-block-wallet)

The prominent amendment can be found [here.](https://github.com/Rpc-h/extension-block-wallet/blob/d5bacfd024e75ad579636a69cce919d7e1a2f7a8/packages/background/src/controllers/NetworkController.ts#L526)

| repository       | example                                                                                                                                                                                                                   |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [BlockWallet](https://github.com/Rpc-h/extension-block-wallet) | [Integration 1](https://github.com/Rpc-h/extension-block-wallet/blob/d5bacfd024e75ad579636a69cce919d7e1a2f7a8/packages/background/src/controllers/NetworkController.ts#L526) |


The example approach can be closely followed for all wallets using ethers. simply:

1. Find where the wallet is instantiating its providers in the codebase
2. Create an instance of this adaptor by passing in the required options and key-value store functions
3. Integrate it as a useable network on the UI (in this example, this is done simply by adding a new network for gnosis over RPCh)

Depending on how the wallet interacts with the provider, you will likely have to adjust your approach for other parts of the integration. But the majority of the integration can be achieved with the above.
