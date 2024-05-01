Decentralized ledger

### The empty block chain

- "In the beginning there was nothing, and then Satoshi created the first block."
- "And then people started mining additional blocks, with no transactions."
- "And then they got mining reward for each mined block."
- "And that's how users got Bitcoins."
- "And then they started doing transactions."
- "And then there was light."




- Trust ledger with the highest computation work put
- Fraud -> computationally infeasible


**Proof of work:** _find a special number -> put at the end of list of txs -> create a SHA256 hash with first n digits as zero (probability : 1/2^n) guess and check. quick to verify by running the hash

- Blocks -> list of txs -> with proof of work
- block is valid if it has a proof of work
- block contains the hash of the previous block of header -> can't mess with previous blocks (redoing all the work require -> computationally infeasible)


miners mine -> proof of work -> block creator gets some bitcoin -> total no of ledger dollars increases with each mined block

conflicting chains -> take longer chain
after enough time, chains diverge a lot -> probabistilically safe

| Block |
---------
| prev Hash     | 
| Transactions |
| Nonce          |
| Timestamp   |


Nonce -> random values chosen by miners to get first n leading zeroes in hash
Timestamp ->

### What does it take to double spend

If a tx is in the block chain, can the system double spend its coins?

- forking the block chain is the only way to do this
- can the forks be hidden for long?
- if forks happens, miners will pick either one and continue mining
- when a fork gets longer, everyone switches to it
    - if they stay on the shorter fork, they are likely to be outmined by the others and waste work, so they will have incentive to go on the longer one
    - the tx's on the shorter fork get incorporated in the longer one
    - committed tx's can get undone => people usually wait for a few extra blocks to be created after a tx's block
- this is where the 51% rule comes in: if 51% of the computing power is honest the protocol works correctly
- if more than 51% are dishonest, then they'll likely succeed in mining anything they want
- probably the most clever thing about bitcoin: as long as you believe than more than half the computing power is not cheating, you can be sure there's no double spending



- For a brief period of time there might be double spending in a blockchain due to a fork
- dont believe the blockchain until there is a couple more blocks in the blockchain


**Bitcoin**:

- 10 min to mine each block - so problem becomes harder as new miners join
- block fee halved every (210000 blocks) 4 years (total amount around 21 million)
- tx fee - incentives
- difficulty adjusted every 2016 blocks (~2 weeks) to avoid frequent forking


### Optimization
 
 1. Merkle tree
- leaf nodes has data
- non-leaf nodes has hash of children nodes
- the merkle root hash (32 bytes) is in the block chain header
- this enables fast verification of the chain
    - no need to compute hash for all transaction content.