## ( Work in Progress )

## Migration Approaches
- Identity the last block using peer channel fetch
- Clone all transaction blocks using peer channel fetch
- Conversion of blocks to JSON
- Using JSON objects to execute transactions in the network

## Migration from 1.4 to 2.0
- Perform all steps to do a live network (in-service) upgrade from 1.4.4 to 2.0. 
- For more details, refer also to file fabric-test/tools/operator/scripts/upgradeNetwork.sh, and look for command "operator -a upgradeNetwork".
- Launch a network with CA, peers, and raft orderers. (Repeat test with kafka.)
- Run transactions using different chaincode languages.
- Stop one orderer at a time, while network remains fully functional, for a live network upgrade. 
- Or stop all at once during a maintenance window for an Out-Of-Service network upgrade. Backup ledger if desired.
- Adjust appropriate config parameters for the orderer. Upgrade the software images/binaries to v2.0. Restart the orderer(s).
- Stop peer(s); do NOT delete the v14 cc container and cc image yet. 
- Execute "peer node upgrade-dbs" (using new v20 binaries) on ledger file system for each peer. 
- Backup ledger if desired. Adjust appropriate config parameters for the peer. 
- Upgrade the software images/binaries to v2.0. 
- Launch peer(s) again with v20; the next TX will trigger recreation of cc container which will still be the v14-shim-compatible version. 
- Update the V2_0 capabilities with a channel config update transaction.
- Note chaincode TXs still use the existing v1.4 version of chaincode. 
- Verify the proposals and transactions that were successful in 1.4 can still run successfully.
- Update the policies for consortium, organizations, app policies, ACLs - on orderersystemchannel and all applic channels. 
- (Mostly required for new lifecycle actions.) 
- Note: If wish to continue with old lifecycle (leaving application capabilities at v1.4.2), then skip this step of updating policies.
- Upgrade all chaincodes in all channels to a version compatible with v20 shim (new lifecycle, commit) with a channel config update. 
- When commit, a v20 cc container should be automatically launched, using new v20 ccenv. Verify:
    "peer chaincode list"  ---  shows v14
    "peer lifecycle chaincode querycommitted"  ---  shows v20  
- Verify the config updates, proposals and transactions using new functionalities (such as new _lifecycle) are successful.
- Cleanup: delete the old v14-compatible versions of cc container and cc image on each peer (which are no longer used).

## References
- https://stackoverflow.com/questions/67984442/issue-while-upgrading-fabric-from-1-4-4-to-2-2
- https://hyperledger-fabric.readthedocs.io/en/release-2.2/upgrade_to_newest_version.html
- https://jira.hyperledger.org/browse/FAB-16986
