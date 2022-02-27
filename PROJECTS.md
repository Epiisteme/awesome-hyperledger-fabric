
## Cross Channel Chaincode on FabCar Network

- docker exec cli peer lifecycle chaincode package ./fabtrip.tar.gz --path /home/gbalex/hlf-apps/fabric-samples/chaincode/fabcar/go/ --label fabtrip
- docker exec cli peer lifecycle chaincode package ./fabtrip.tar.gz --path fabric-samples/chaincode/fabcar/go/ --label fabtrip
- docker exec cli peer lifecycle chaincode queryinstalled

## References 
- https://docs.aws.amazon.com/managed-blockchain/latest/hyperledger-fabric-dev/get-started-chaincode.html
