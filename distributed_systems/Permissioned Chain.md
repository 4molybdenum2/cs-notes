
Hyperledger Fabric:


- first blockchain system that runs distributed applications written in general purpose programming language
- doesn't require smart contracts compared to other blockchain platforms written in domain specific languages
- permissioned blockchains run a blockchain amongst a set of known or identified participants
- In existing blockchains we require all the operations to be deterministic for smart contracts - otherwise the blockchain forks and violates the basic policy of blockchain
- sequential execution in order-execute architecture limits performance - complex measures to prevent DoS attacks ("gas" accounting for runtime in Ethereum)


- Fabric -> execute - order - validate
- fabric typically executes transactions before reaching final agreement 
- confidentiality is preserved by permissioned peers (passive replication)

![execute order validate](execute_order.png)
- fabric tolerates non-deterministic code by execution phase before getting endorsements
- prevents DoS attacks in such way
	





**Working of Hyperledger Fabric:**

1. orderer: responsible for block creation
2. peer: each of which keeps a copy of the ledger, commit the block received from orderers and update its own ledger.
3. CA: issues certificates to components in an organization




peer:
1. blockchains
2. worldstate (LevelDB, CouchDB): latest state of everything stored in ledger stored as k/v pair
3. state is updated once a block is committed to the ledger


channel:
-  ledger is channel specific



chaincode deployment:
- package chaincode
- install chaincode to peers
- approve chaincode definitions (deployment policies) by participating organizations
- commit chaincode defn to channel
- clients can now use chaincode by means of querying or invoking chaincode functions
- if proposal responses are not identical from all peers or do not meet the endorsement polciies, client will not proceed
- assuming everything is correct, client constructs a tx including proposal and simulated result and signatures who made this simulation (endorsement). tx is sent to one of the orderers.
- orderers run raft based servers, checks if tx is valid and ep is satisfied, places tx in a newly generated block.
- sends block to all peers in channel
- peers commit block individually and updates ledger
