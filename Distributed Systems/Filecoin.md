
Offchain storage:
1. How to prove each storage server actually stores the data - verifier cannot verify the entire file
2. Attacks - not storing the file, blank link, generating proofs dynamically - sybil attack pretend to store more copies than there actually is - outsourcing to external storage
3. zero knowledge proof
4. avoiding sybil attack - multiple rounds of encryption (AES)
5. avoid outsourcing - require verification to finish within timeout


Filecoin:

_Turns cloud storage into an algorithmic market

- Miners earn filecoin by providing storage to clients
- Clients spend filecoin hiring miners to store and distribute data
- blockchain works as trusted party 
	- bid: alice -> market for storage
	- ask Bob -> market for storage
	- deal: Bob -> alice within a time bound

Put procedure:
- client: add bid order to ledger L
- network: match order and find ask
- client: sign deal, send deal (bid, ask), piece (with pk, vk, aes) to miner storage server
- miner: receive deal, piece, store the piece, sign


Get procedure:
- client can lie -> can't be prevented 
- on chain - too much overhead
- off chain - micropayment (signed by client) miner will send grouped micropayments to L after the data transfer is over
- threshold for client balance