# Light Client Specification for Optimistic Layer-2 
This repository serves as a shared document for collaborating 
on the specification of light clients for optimistic rollups 
built on Ethereum, such as Optimism and Arbitrum. Currently, 
rollups checkpoint their state to the L1 (Ethereum) every two weeks, 
which is the maximum time required to perform fraud proofs. It is possible to 
run an Ethereum light client based on the [sync protocol](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/light-client/sync-protocol.md) 
to validate this stale state of the rollup. In this document, we are exploring potential 
methods to validate a more recent rollup state.

Currently, there are two potential constructions for 
such light clients:
1. **Lazy Light Client:** Treat the transactions posted by the rollup on the
   L1 as a dirty ledger and run a light client for lazy blockchains.
   We can use the construction proposed by [Nusret et al.](https://arxiv.org/abs/2203.15968), 
   which performs an interactive bisection game among a set of all malicious but  
   one honest prover (existential honesty).
2. **Economic Light Client:** We trust the state commitments signed
   by a sequencer or a third party that has locked sufficient stakes
   on the L1. If the attested state differs from the 
   checkpointed L2 state, we slash the attested party on the L1. 


 | Metric | Lazy Light Client | Economic Light Client |
 | :----- | :-----: | :-----:|
 | Trust assumptions | Existential honesty among the provers | Honesty of the staker |
 | Interactivity | Polylog rounds of communication | Constant rounds of communication |
 | Computational overhead | Requires running L1 light-client + might require un-packing and running a complete L2 block locally | Unclear |
 | Delay | Can only validate up to the last batch of txs posted on L1 | Can validate up to the latest pre-conf issues by the sequencer | 

