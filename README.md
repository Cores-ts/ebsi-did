
![EBSI Logo](https://ec.europa.eu/cefdigital/wiki/images/logo/default-space-logo.svg)

# EBSI DID Library

This library conforms to [ERC-1056](https://github.com/ethereum/EIPs/issues/1056) and is intended to use Ethereum addresses as fully self-managed [Decentralized Identifiers](https://w3c-ccg.github.io/did-spec/#decentralized-identifiers-dids) (DIDs), it allows you to easily create and manage keys for these identities.  

This library can be used to create a new ebsi-did identity.  It allows ebsi-did identities to be represented as an object that can perform actions such as updating its did-document, signing messages, and verifying messages from other dids.

Use this if you are looking for the easiest way to start using ebsi-did identities, and want high-level abstractions to access its entire range of capabilities.

A DID is an Identifier that allows you to lookup a DID document that can be used to authenticate you and messages created by you.

Ebsi-DID provides a scalable identity method for Ethereum addresses that gives any Ethereum address the ability to collect on-chain and off-chain data. Because Ebsi-DID allows any BESU key pair to become an identity.

This particular DID method relies on the [Ethr-Did-Registry](https://github.com/uport-project/ethr-did-registry). The Ethr-DID-Registry is a smart contract that facilitates public key resolution for off-chain (and on-chain) authentication. It also facilitates key rotation, delegate assignment and revocation to allow 3rd party signers on a key's behalf, as well as setting and revoking off-chain attribute data. These interactions and events are used in aggregate to form a DID's DID document using the Ebsi-Did-Resolver.

An example of a DID document resolved using the EBSI-Did-Resolver:

```
{
  '@context': 'https://w3id.org/did/v1',
  id: 'did:ebsi:0xb9c5714089478a327f09197987f16f9e5d936e8a',
  publicKey: [{
       id: 'did:ebsi:0xb9c5714089478a327f09197987f16f9e5d936e8a#key-1',
       type: 'Secp256k1VerificationKey2018',
       controller: 'did:ebsi:0xb9c5714089478a327f09197987f16f9e5d936e8a',
       ethereumAddress: '0xb9c5714089478a327f09197987f16f9e5d936e8a'}],
  authentication: [
       'did:ebsi:0xb9c5714089478a327f09197987f16f9e5d936e8a#key-1'
       ]
}
```

On-chain refers to something that is resolved with a transaction on a blockchain, while off-chain can refer to anything from temporary payment channels to IPFS.

It supports the proposed [Decentralized Identifiers](https://w3c-ccg.github.io/did-spec/) spec from the [W3C Credentials Community Group](https://w3c-ccg.github.io).


## DID Method

A "DID method" is a specific implementation of a DID scheme that is identified by a `method name`. In this case, the method name is `ebsi`, and the method identifier is an BESU address.

To encode a DID for an BESU address, simply prepend `did:ebsi:`


For example:

`did:ebsi:0xf3beac30c498d9e26865f34fcaa57dbb935b0d74`

## Configuration

```js
import EbsiDID from 'ebsi-did'

// Assume web3 object is configured either manually or injected using metamask


const ebsiDid = new EbsiDID({address: '0x...', privateKey: '...', provider})
```

| key | description| required |
|-----|------------|----------|
|`address`|Ethereum address representing Identity| yes |
|`registry`| registry address (defaults to `0xdca7ef03e98e0dc2b855be647c39abe984fcf21b`) | no |
|`provider`| web3 provider | no |
|`web3`| preconfigured web3 object | no |
|`rpcUrl`| JSON-RPC endpoint url | no |
|`signer`| [Signing function](https://github.com/uport-project/did-jwt#signer-functions)| either `signer` or `privateKey` |
|`privateKey`| Hex encoded private key | yes* |

**Note**
An instance created using only an address can only be used to encapsulate an external ebsi-did (one where there is no access to the private key).
This instance will not have the ability to sign anything, but it can be used for a subset of actions:

*  provide its own address (`ebsiDid.address`)
*  provide the full DID string (`ebsiDid.did`)
*  lookup its owner `await ebsiDid.lookupOwner()`
*  verify a JWT `await ebsiDid.verifyJwt(jwt)`

## Licensing

Copyright (c) 2019 European Commission  
Licensed under the EUPL, Version 1.2 or - as soon they will be approved by the European Commission - subsequent versions of the EUPL (the "Licence"); 
You may not use this work except in compliance with the Licence. 
You may obtain a copy of the Licence at: 
* https://joinup.ec.europa.eu/page/eupl-text-11-12  

Unless required by applicable law or agreed to in writing, software distributed under the Licence is distributed on an "AS IS" basis, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the Licence for the specific language governing permissions and limitations under the Licence.