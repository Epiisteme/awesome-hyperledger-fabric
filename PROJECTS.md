
## Cross Channel Chaincode on FabCar Network

- docker exec cli peer lifecycle chaincode package ./fabtrip.tar.gz --path /home/gbalex/hlf-apps/fabric-samples/chaincode/fabcar/go/ --label fabtrip
- docker exec cli peer lifecycle chaincode package ./fabtrip.tar.gz --path fabric-samples/chaincode/fabcar/go/ --label fabtrip
- docker exec cli peer lifecycle chaincode queryinstalled



Brought up the test network
Enabled ConfigTxgen Path

Creating Channel 1

configtxgen -profile TwoOrgsApplicationGenesis -outputBlock ./channel-artifacts/channel1.block -channelID channel1

Creating Channel 2

configtxgen -profile TwoOrgsApplicationGenesis -outputBlock ./channel-artifacts/channel2.block -channelID channel2



export ORDERER_CA=${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
export ORDERER_ADMIN_TLS_SIGN_CERT=${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/tls/server.crt
export ORDERER_ADMIN_TLS_PRIVATE_KEY=${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/tls/server.key



osnadmin channel join --channelID channel1 --config-block ./channel-artifacts/channel1.block -o localhost:7053 --ca-file "$ORDERER_CA" --client-cert "$ORDERER_ADMIN_TLS_SIGN_CERT" --client-key "$ORDERER_ADMIN_TLS_PRIVATE_KEY"

Status: 201
{
	"name": "channel1",
	"url": "/participation/v1/channels/channel1",
	"consensusRelation": "consenter",
	"status": "active",
	"height": 1
}



osnadmin channel join --channelID channel2 --config-block ./channel-artifacts/channel2.block -o localhost:7053 --ca-file "$ORDERER_CA" --client-cert "$ORDERER_ADMIN_TLS_SIGN_CERT" --client-key "$ORDERER_ADMIN_TLS_PRIVATE_KEY"

Status: 201
{
	"name": "channel2",
	"url": "/participation/v1/channels/channel2",
	"consensusRelation": "consenter",
	"status": "active",
	"height": 1
}

osnadmin channel list -o localhost:7053 --ca-file "$ORDERER_CA" --client-cert "$ORDERER_ADMIN_TLS_SIGN_CERT" --client-key "$ORDERER_ADMIN_TLS_PRIVATE_KEY"

TE_KEY"
Status: 200
{
	"systemChannel": null,
	"channels": [
		{
			"name": "channel1",
			"url": "/participation/v1/channels/channel1"
		},
		{
			"name": "channel2",
			"url": "/participation/v1/channels/channel2"
		}
	]
}

export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051

Peer Joining the Channel 1 

peer channel join -b ./channel-artifacts/channel1.block

2022-03-05 13:19:31.067 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:19:31.318 IST 0002 INFO [channelCmd] executeJoin -> Successfully submitted proposal to join channel


Peer Joining Channel 2

peer channel join -b ./channel-artifacts/channel2.block

2022-03-05 13:19:49.080 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:19:49.373 IST 0002 INFO [channelCmd] executeJoin -> Successfully submitted proposal to join channel





## References 
- https://docs.aws.amazon.com/managed-blockchain/latest/hyperledger-fabric-dev/get-started-chaincode.html
