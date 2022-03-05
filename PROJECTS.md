
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


export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051

peer channel join -b ./channel-artifacts/channel1.block

2022-03-05 13:24:33.450 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:24:33.832 IST 0002 INFO [channelCmd] executeJoin -> Successfully submitted proposal to join channel


peer channel join -b ./channel-artifacts/channel2.block

2022-03-05 13:24:52.586 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:24:52.837 IST 0002 INFO [channelCmd] executeJoin -> Successfully submitted proposal to join channel

export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051

peer channel fetch config channel-artifacts/config_block_c1.pb -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com -c channel1 --tls --cafile "$ORDERER_CA"

2022-03-05 13:35:35.518 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:35:35.520 IST 0002 INFO [cli.common] readBlock -> Received block: 0
2022-03-05 13:35:35.520 IST 0003 INFO [channelCmd] fetch -> Retrieving last config block: 0
2022-03-05 13:35:35.521 IST 0004 INFO [cli.common] readBlock -> Received block: 0


configtxlator proto_decode --input config_block_c1.pb --type common.Block --output config_block_c1.json
jq '.data.data[0].payload.data.config' config_block_c1.json > config_c1.json
cp config_c1.json config_c1_copy.json

jq '.channel_group.groups.Application.groups.Org1MSP.values += {"AnchorPeers":{"mod_policy": "Admins","value":{"anchor_peers": [{"host": "peer0.org1.example.com","port": 7051}]},"version": "0"}}' config_c1_copy.json > modified_config_c1.json

configtxlator proto_encode --input config_c1.json --type common.Config --output config_c1.pb
configtxlator proto_encode --input modified_config_c1.json --type common.Config --output modified_config_c1.pb
configtxlator compute_update --channel_id channel1 --original config_c1.pb --updated modified_config_c1.pb --output config_update_c1.pb

configtxlator proto_decode --input config_update_c1.pb --type common.ConfigUpdate --output config_update_c1.json

echo '{"payload":{"header":{"channel_header":{"channel_id":"channel1", "type":2}},"data":{"config_update":'$(cat config_update_c1.json)'}}}' | jq . > config_update_in_envelope_c1.json

configtxlator proto_encode --input config_update_in_envelope_c1.json --type common.Envelope --output config_update_in_envelope_c1.pb

cd ..

peer channel update -f channel-artifacts/config_update_in_envelope_c1.pb -c channel1 -o localhost:7050  --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem"

2022-03-05 13:52:23.820 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:52:23.826 IST 0002 INFO [channelCmd] update -> Successfully submitted channel update





peer channel fetch config channel-artifacts/config_block_c2.pb -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com -c channel2 --tls --cafile "$ORDERER_CA"

2022-03-05 13:35:35.518 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 13:35:35.520 IST 0002 INFO [cli.common] readBlock -> Received block: 0
2022-03-05 13:35:35.520 IST 0003 INFO [channelCmd] fetch -> Retrieving last config block: 0
2022-03-05 13:35:35.521 IST 0004 INFO [cli.common] readBlock -> Received block: 0


configtxlator proto_decode --input config_block_c2.pb --type common.Block --output config_block_c2.json
jq '.data.data[0].payload.data.config' config_block_c2.json > config_c2.json
cp config_c2.json config_c2_copy.json

jq '.channel_group.groups.Application.groups.Org1MSP.values += {"AnchorPeers":{"mod_policy": "Admins","value":{"anchor_peers": [{"host": "peer0.org1.example.com","port": 7051}]},"version": "0"}}' config_c2_copy.json > modified_config_c2.json

configtxlator proto_encode --input config_c2.json --type common.Config --output config_c2.pb
configtxlator proto_encode --input modified_config_c2.json --type common.Config --output modified_config_c2.pb
configtxlator compute_update --channel_id channel2 --original config_c2.pb --updated modified_config_c2.pb --output config_update_c2.pb

configtxlator proto_decode --input config_update_c2.pb --type common.ConfigUpdate --output config_update_c2.json

echo '{"payload":{"header":{"channel_header":{"channel_id":"channel2", "type":2}},"data":{"config_update":'$(cat config_update_c2.json)'}}}' | jq . > config_update_in_envelope_c2.json

configtxlator proto_encode --input config_update_in_envelope_c2.json --type common.Envelope --output config_update_in_envelope_c2.pb

cd ..

peer channel update -f channel-artifacts/config_update_in_envelope_c2.pb -c channel2 -o localhost:7050  --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem"

2022-03-05 14:01:33.219 IST 0001 INFO [channelCmd] InitCmdFactory -> Endorser and orderer connections initialized
2022-03-05 14:01:33.224 IST 0002 INFO [channelCmd] update -> Successfully submitted channel update

Anchor Peer Update for the Organisation 2

export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051





peer channel fetch config channel-artifacts/config_block_c1.pb -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com -c channel1 --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem"




peer channel fetch config channel-artifacts/config_block.pb -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com -c channel2 --tls --cafile "$ORDERER_CA"


## References 
- https://docs.aws.amazon.com/managed-blockchain/latest/hyperledger-fabric-dev/get-started-chaincode.html
