
## Introduction
- https://xord.com/publications/hyperledger-fabric-architecture-a-deep-dive/

## System ChainCodes
- https://medium.com/coinmonks/system-chaincodes-in-hyperledger-fabric-vscc-escc-lscc-cscc-a48db4d24dc3

## Offchain Storage
- https://deeptiman.medium.com/offchain-storage-in-hyperledger-fabric-77e28bd99e0c
- https://hackernoon.com/making-a-case-for-offchain-storage-in-hyperledger-fabric-deep-dive-2f8g32v0

## Preventing Key Collisions
- https://medium.com/@gatakka/how-to-prevent-key-collisions-in-hyperledger-fabric-chaincode-303700716733

## Concurrency in Chaincodes
- Reference >> https://ibm.github.io/hlf-internals/shim-architecture/interaction-flow/transaction-processing/
- Transaction proposals are executed asynchronously leaving main thread to process other requests
- Chaincode initialisation and transaction execution is handled differently
