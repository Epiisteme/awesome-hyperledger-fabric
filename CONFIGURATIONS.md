# References

## External Chaincodes
- https://saifworks.hashnode.dev/external-chaincode-launcher-hyperledger-fabric
- https://www.blockchainexpert.uk/blog/external-chaincode-service-in-hyperledger-fabric
- https://dev.to/vanitas92/how-to-implement-hyperledger-fabric-external-chaincodes-within-a-kubernetes-cluster-34de

## Migration from Raft to Kafka
- https://www.altoros.com/blog/consensus-in-hyperledger-fabric-migrating-from-kafka-to-raft/

## Deployment on Multiple Hosts
- https://wahabjawed.medium.com/hyperledger-fabric-on-multiple-hosts-a33b08ef24f

## Multiple Orderer Organisations
- https://stackoverflow.com/questions/61592219/multiple-orderer-organizations

## Adding An Organisation
- https://github.com/happilymarrieddad/k8s-hyperledger-fabric-2.2/blob/master/ADDING_AN_ORG.md

## Certificate Generation
- https://medium.com/geekculture/certificate-generation-msp-tls-in-hyperledger-fabric-blockchain-ef7aef7bcc44

## Security Configurations
- https://espeoblockchain.com/blog/a-practical-guide-to-hyperledger-fabric-security

## Custom Endorsement Policies
- https://hyperledger-fabric.readthedocs.io/en/release-2.2/pluggable_endorsement_and_validation.html

## Block Size and Block Timeout
- Caliper Benchmark on Hyperldger Fabric
- https://github.com/tittuvarghese/Caliper_HLF/blob/master/caliper-benchmarks/networks/fabric/config_raft/configtx.yaml
- https://github.com/bharathreddypanyala/fabric/blob/master/caliper-benchmarks/networks/fabric/config_raft/configtx.yaml
- https://github.com/Karumba-PhD-UNSW/caliper-benchmarks/blob/master/networks/fabric/config_raft/configtx.yaml
- https://github.com/sjlee1125/Multi-Host-Fabric-Network/blob/master/configtx.yaml
- https://github.com/FujitsuLaboratories/ConnectionChain-sample/blob/master/environment/base/cc_env/yaml-files/configtx.yaml

## Important Insights
- https://www.mdpi.com/2076-3417/11/9/3870/htm

### Block Size and Performance
- It is found that the performance of Fabric increases with large Block size
- It is demonstrated that Raft is superior to Kafka

### Challenges for Scheduling Tasks
- Fabric consists of heterogeneous components performing different operations, 
- Hence it is difficult to apply the same scheduling policy to the Fabric components.
- Peers consume 2.3 times the time slice than the orderers 
- Fabric can experience performance degradation when components run with co-located tasks.

## Questions and Answers

### What are the conditions for generating blocks from transactions
### Answer: 
- the number of transactions in the block reaches the threshold
- the block size attains the maximum value in bytes
- certain time has passed since the first transaction of the new block was received. 

### Why does peers take more time slice than orderers and chaincodes ?
### Answer:
- It is because peers perform verification of the client’s transaction proposals and validate new blocks. 

### What is state based endorsement policy?
### Answer :
- The state-based endorsement allows the default chaincode-level endorsement policies
- It can be be overridden by a different policy for the specified keys.
