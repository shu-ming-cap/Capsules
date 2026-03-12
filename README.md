# Capsules:  An Object-Centric Architecture for Local Sovereignty

**Primary Home:** [Codeberg]([https://codeberg.org/shu/repo](https://codeberg.org/shuming/Capsules) | **Mirror:** [GitHub]([https://github.com/user/repo](https://github.com/shu-ming-cap/Capsules))

> "The network is an optimization. Sovereignty and integrity are local to an object."

### Overview

Capsules are a substrate for digital objects that internalize data, logic, and authority. By integrating the application stack into a standardized, cryptographically-owned container, Capsules enable true local sovereignty, making centralized servers and "cloud" providers optional.

### Technical Foundation

This project defines a structural proof for digital objects composed of:
* **Container:** OCF / EPUB 3.3 (Standardized, file-system agnostic)
* **Logic:** WebAssembly / WASI (Deterministic, sandboxed execution)
* **Authority:** Ed25519 / X25519 (Self-sovereign cryptographic keys)
* **History:** Merkle-DAG / CRDTs (Peer-to-peer state recovery and convergence)

### The Specification

The full technical paper, including the lineage of foundational proofs, is available in this repository:

- Link to Paper
### Cryptographic Anchor

The canonical version of the architectural paper is identified by the following SHA-256 hash:

`251c361135aa913dc4ea0f7bb68d309063ca8b4852274f95a983a551aafdaf2a

### Status

This work is a proof of structural possibility. It is offered to the public domain. Implementation details regarding a specific reference readers, CRDT merge strategies, and peer discovery are open for community development.

### License

This work is dedicated to the public domain under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).

---
**Author:** Shu Ming  
**Contact:** keisanmedia [at] proton [dot] me  
**Date:** 2026-03-12
