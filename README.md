# Light Client Specification for Optimistic Layer-2 
This repository serves as a shared document for collaborating 
on the specification of light clients for optimistic rollups 
built on Ethereum, such as Optimism and Arbitrum. Currently, 
rollups checkpoint the state to the L1 (Ethereum) every two weeks, 
which is the time required to perform fraud proofs. It is possible to 
run an Ethereum light client based on the sync protocol to validate this 
stale state of the rollup. In this document, we are exploring potential 
methods to validate a more recent rollup state.

Currently, there are two potential constructions for 
such light clients:
1. **Lazy Light Client:** Treat the transactions posted by the rollup on the
   L1 as a dirty ledger and run a light client for lazy blockchains.
   We can use the construction proposed by Nusret et al., which performs
   an interactive bisection game among a set of all malicious but  
   one honest prover (existential honesty).
2. **Economic Light Client:** We trust the state commitments signed
   by a sequencer or a third party that has locked sufficient stakes
   on the L1. If the attested state differs from the 
   checkpointed L2 state, we slash the attested party on the L1. 


 | metric | Lazy Light Client | Economic Light Client |
 | :----- | :-----: | :-----:|
 | trust assumption | existential honesty among provers | honesty of the bond holder |
 | interactivity | poly log rounds of communication | constant rounds of communication |
 | computational overhead | might require un-packing and running a complete L2 block locally | no additional overhead |
 | delay | can only validate up to the last batch of txs posted on L1 | can validate up to the latest pre-conf issues by the sequencer | 

