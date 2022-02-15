
## Questions and Answers

### What are the recommended approaches to build a dashboard to collect data from Hyperledger Fabric
- you can query an off-chain database that replicates the data from your blockchain network. 
- This will allow you to query and analyze the blockchain data in a data store optimized for your needs, 
- Applications may use block or chaincode events to write transaction data to an off-chain database or analytics engine. 
- For each block received, the block listener application would iterate through the block transactions 
- build a data store using the key/value writes from each valid transaction’s read write set. 
- The Peer channel-based event services provide replayable events to ensure the integrity of data stores.
