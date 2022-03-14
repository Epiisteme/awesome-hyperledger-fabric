
## Introduction
- https://xord.com/publications/hyperledger-fabric-architecture-a-deep-dive/

## Peer State Management
- http://www.bcmentors.com/fabric-peer-state-management/

## Signing Transactions Offline
- https://hyperledger.github.io/fabric-sdk-node/release-2.2/tutorial-sign-transaction-offline.html

## Wallets
- https://stackoverflow.com/questions/55358598/what-is-the-difference-between-a-cryptokeystore-and-a-wallet-in-hyperledger-fabr
- https://github.com/FahimDev/Private-Blockchain-SampleWallet
- https://github.com/IBM/fabric-postgres-wallet

## Composite Keys in Chaincodes
- https://stackoverflow.com/questions/44918611/composite-key-functions-in-hyperledger

## Tokens in Fabric System
- https://medium.com/coinmonks/the-secret-gem-of-hyperleder-fabric-fabtoken-aa5c37159247
- https://medium.com/@blockchain_simplified/creating-tokens-on-hyperledger-fabric-2-0-using-fabtoken-management-system-3c9689c0a99d

## Event Listners
- https://medium.com/@jushuspace/hyperledger-fabric-event-listener-tutorial-2484614a9e4
- https://medium.com/coinmonks/tutorial-chaincode-event-listener-on-hyperledger-fabric-java-sdk-557304f1fe28
- https://stackoverflow.com/questions/48558344/hyperledger-fabric-chaincode-events
- https://stackoverflow.com/questions/46462151/how-to-listen-to-the-eventcommit-event-in-hyperledger-fabric
- https://ibm.github.io/hlf-internals/shim-architecture/interaction-flow/chaincode-events/

## Iterators and Chaincodes
- https://hyperledger.github.io/fabric-chaincode-node/release-2.2/api/tutorial-using-iterators.html

## Interchaincode invocation
- https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/chaincodenamespace.html#cross-chaincode-access
- https://stackoverflow.com/questions/67070302/how-to-use-invokechaincode-api

## Cross Channel Chaincodes
- https://github.com/asararatnakar/fabric_v1_Chaincode_instructions/blob/master/call-chaincode-to-chaincode-nondefault-chain.md
- https://github.com/thaonguyentien/BC/blob/d1104a2ba5c9bb5b605d607e4c9f0d9317934524/fabric-k8s/chaincode/crosschaincode/crosschaincode.go
- https://stackoverflow.com/questions/58163924/is-there-a-best-practice-to-invoke-cross-channel-transaction
- https://arnabkaycee.github.io/fabric-cc-best-practices-golang/
- https://github.com/lushena/fabric-cross-agent
- https://jira.hyperledger.org/browse/FAB-1788

## External Chaincodes
- https://www.blockchainexpert.uk/blog/external-chaincode-service-in-hyperledger-fabric
- https://hyperledger-fabric.readthedocs.io/en/release-2.2/cc_launcher.html
- https://github.com/vanitas92/fabric-external-chaincodes

## System ChainCodes
- https://medium.com/coinmonks/system-chaincodes-in-hyperledger-fabric-vscc-escc-lscc-cscc-a48db4d24dc3

## Offchain Storage
- https://deeptiman.medium.com/offchain-storage-in-hyperledger-fabric-77e28bd99e0c
- https://hackernoon.com/making-a-case-for-offchain-storage-in-hyperledger-fabric-deep-dive-2f8g32v0

## Preventing Key Collisions
- https://medium.com/@gatakka/how-to-prevent-key-collisions-in-hyperledger-fabric-chaincode-303700716733

## Concurrency in Chaincodes
- https://blog.softwaremill.com/concurrent-smart-contracts-in-hyperledger-fabric-blockchain-part-1-bba32b2c0e08
- https://blog.softwaremill.com/concurrent-smart-contracts-in-hyperledger-fabric-blockchain-part-2-fc09af32e48d
- Reference >> https://ibm.github.io/hlf-internals/shim-architecture/interaction-flow/transaction-processing/
- Transaction proposals are executed asynchronously leaving main thread to process other requests
- Chaincode initialisation and transaction execution is handled differently

## Lifecycle of Chaincode
- Chaincode setup, gRPC communication, protocol execution, interfacing, configuration, business logic
- deserialisation (unmarshalling) of the ChaincodeInput instance from the ChaincodeMessage payload;
- creation of a new instance of the ChaincodeStub and configuration of the transaction context 
- It comprises of channel identifier, transaction identifier, input (i.e. ChaincodeInput) and signed proposal;
- invocation of the Chaincode.Init(ChaincodeStub) or Chaincode.Invoke(ChaincodeStub) method.

## Chaincode and gRPC
- bidirectional communication stream with the peer

## Chaincode and Shim concepts
- The shim is responsible for starting and coordinating the interaction between the Chaincode implementation and the peer processes. 
- Structs are required to be marshalled to JSON string before putting into chaincode state and unmarshalled after receiving from state.

## Defining Anchor Peers
- Once a peer is defined as an anchor peer for a channel - it automatically is anchor peer for all other channels that it participates in. 
- The anchor tracking is being done at org msp level, even thouh it is defined at channel level.

## On Fabric Gateway
- The Gateway runs as a gRPC server 
- The client credentials are never passed to the Gateway

## On Fabric Client
- Client applications are responsible for managaing their user keys
- Client applications can use the SDK Wallets for this purpose

## On Ledger Snapshot
- A set of files containing all the necessary information for OSN to join a channel from an arbitrary block.

## On Ledger Checkpointing
- It is the construction of a snapshot of a channel at a particular block height
- Using this peers can join a channel using snapshot as a starting date
- This is the ability to bootstrap a peer from latest channel configurations

## On Private Data Purge
- a new operation to purge the private data via chaincode shim that allows the deletion of private data keys from the statedb
- It also enable purging of all the historical versions of the supplied keys from the private data store permanently.

## On Range Queries
- Range queries supported by fabric chaincode use Merkle trees to optimize storage of a range of keys read in a transaction.

## On Locking Mechanisms
- Hyperledger Fabric uses an Optimistic Locking Model while committing transactions.
