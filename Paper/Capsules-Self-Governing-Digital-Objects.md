An Architecture for Complete Local Sovereignty

Shu Ming
keisan.media@proton.me
2026-03-12
## ABSTRACT

Digital objects are currently bounded by their host environments. This paper defines the Capsule: a self-governing digital object that internalizes data, logic, and authority. By shifting the consistency boundary, Capsules enable complete local sovereignty at the object level, making servers optional; and by unifying data, logic, and authority, Capsules offer a new standard for persistence.

## DISCLAIMER

This work is a proof of structural possibility. It demonstrates that functions historically separated across files, programs/applications, identity systems, and networks can be coherently unified within a single, cryptographically owned object. If one believes the separations are necessary, the question is which function(s) cannot be internalized, and why.


## 1 SUMMARY

Prevailing digital architecture forces a choice between owning static files and participating in interactive networks. Capsules resolve this by introducing digital objects that embed an executable stack based on standard web technologies: an EPUB container (Open Container Format), a WebAssembly binary (Wasm), a cryptographic key set (Ed25519/X25519), and append-only logs (Merkle trees). The result is a self-governing digital object capable of cryptographically updating and recovering state without reliance on centralized infrastructure.

## 2 INTRODUCTION

The local-first community showed that servers are an optional service for coordination.  As that community focuses on improving performance, their research and development largely maintains silos between data, authorization, and logic.  

Capsules remove these silos by defining self-governing, sovereign digital objects. Connectivity is treated as a convenience, not a requirement. Governance—the rules for who can view/publish/edit, what execution logic is valid, and how state evolves—is embedded in the object itself, alongside the data and history.  

## 3 CAPSULE STRUCTURE

Capsules utilize the EPUB 3.3 container as a base file format. The EPUB specification provides a structured, zipped manifest supporting HTML5, CSS, and JavaScript. To this, Capsules embed three critical primitives by leveraging the Foreign Resource provision in the EPUB specifications.  These primitives enable the following operational behaviors:
### 3.1 Access Governance: Key Set (Registry) 

An Ed25519/X25519 key set (based on Curve25519) defines the governing public keys and their associated permissions (Ed25519 for signatures; X25519 for key agreement). Possession of a corresponding private key is required to perform authorized actions. The registry functions as the authoritative state for key-based permissions and is updated only according to rules defined within the Capsule.   

The original publisher has discretion over which permission types to include, with the scope of authorities determined by the Capsule’s intended use.  
### 3.2 Data Integrity: Append-Only Logs (History)

Updates are recorded as append-only logs maintained per author key and structured as Merkle trees. This provides an auditable history of actions. No global ordering is required; verification is performed locally based on the Capsule’s internal governance rules.
### 3.3 Programmable Executions: WebAssembly (Logic)

A WebAssembly binary (Wasm) is included to manage and govern executable logic. Wasm ensures deterministic execution across host environments.  Wasm allows breadth of programmable options through execution determinism, operation sandboxing, and multi-language support. Additionally, Wasm offers more robust security than relying on EPUB’s built-in support for JavaScript execution.

A Capsule’s Wasm binary can be updated as long as** the update is signed by a key with authority to edit the embedded logic.  This allows the object to evolve its own transition and application-specific functions.
### 3.4 Semantic Merging and Pruning

- Causal Ordering: Each key’s append-only log uses a Merkle tree structure to establish happened-before relationships.

- Deterministic Convergence: Peers identify common ancestors and merge divergent branches using conflict-free replicated data types (CRDT) merge semantics. 

- Surgical Pruning: To mitigate CRDT state-bloat, a Capsule can perform a snapshot where a privileged key signs a state leaf (a flattened result of history), allowing old branches to be pruned while retaining both the new snapshot root and the pre-snapshot root as cryptographic proof of the pruned history.  Additionally, any peer can decide if they accept or reject updates from a given peer.
### 3.5 Forking as a Feature

Capsules treat the ability to copy bytes as fundamental to any digital process.  Therefore, authorized key holders of a Capsule can:

- Duplicate existing container contents and continue development under a new key set;
- Create derivative works where licensing enforcement relies on external legal mechanisms; or
- Roll back to any prior state in the Merkle history.

This is intentional. Forking serves as a check on governance abuse:

- If a key with logic authority rewrites governance maliciously, any peer can reject that branch and continue from the prior valid state.
- If a snapshot prunes history in a contested way, other key holders can fork to maintain the full history.
- Forks can establish clear provenance if the forked Capsule’s history includes the Merkle root of the parent state at the point of divergence, creating a cryptographic link to its origin.

The canonical version of a Capsule is resolved by peer communities deciding which fork to trust.

See Appendices for an example manifest and key registry.  
## 4 PROPAGATION

Updates to a Capsule propagate through any peer-to-peer mechanism (e.g. gossip protocols, wireless sync (Bluetooth), or physical interfaces (USB)). All updates are evaluated locally; if an update does not satisfy the internal rules, it is ignored regardless of its source. Servers can be designated for real-time services and/or convenience but possess no authority beyond what is explicitly granted in the local key set.

Capsules treat networks as an optimization. 
## 5 SECURITY AND THREAT MODEL 

- Integrity vs. Scarcity: Capsules do not prevent copying of content. They focus on preserving the integrity of data and gating future interactions (i.e. preventing unauthorized peers from publishing updates or altering an object’s state).
- The Reader Boundary: A capability-based model (like WASI) is required to sandbox the Wasm logic.  WASI provides a capability-based interface that restricts a Wasm binary’s access to system resources, preventing malicious code from accessing data outside the Capsule. The reader application serves as a ***lens***, surfacing divergent states to users and enabling rollback of state to abandon malicious branches.
- Key Loss: Loss of all governing keys renders a Capsule immutable. This is an accepted outcome, analogous to losing all keys to a locked safe.
## 6 NON-GOALS

- Providing Global Consensus: Capsules avoid the overhead of a single canonical state shared across all peers. Consistency is local and determined by trust.
- Resolving CRDT Convergence Limits: This is an area of active research and Capsules can adopt improved merge strategies without changing the container format (i.e. through updates to a Capsule’s Wasm binary).
- Supporting Publicly Verifiable Scarcity: Capsules are not designed for public ledgers, tokenization, or payments.
- Circumventing the CAP Theorem: Capsules prioritize Availability and Partition Tolerance, relaxing global linearizability in favor of causal consistency. 
- Solving Identity Bootstrapping: Discovery and initial key distribution occur out-of-band (e.g. QR codes, trusted directories, manual verification).  Capsules are designed for peers to establish initial trust through external channels.
## 7 CONCLUSION

Capsules shift the consistency boundary from the network to the object. Authority is scoped to an individual EPUB container, verification is performed locally, execution logic travels with the file, and participation in a network remains optional. This substrate supports the long-term persistence of digital objects through open standards without reliance on centralized infrastructure while allowing users to connect with trusted peers.   

Optimal CRDT selection, UX for conflict resolution, performance characteristics, and peer discovery mechanisms are all implementation concerns once it is accepted that a unifying digital object is possible. Implementation concerns are future work.  
## 8 AUTHOR'S NOTE

This work is the result of an exploration to how digital ownership can extend beyond static possession. The architecture is offered without restriction for anyone to implement, critique, modify, or build upon. 

It is is dedicated to the public domain and no rights are reserved.
## 9 RESOURCES

Primary material at: [https://codeberg.org/shuming/Capsules](https://codeberg.org/shuming/Capsules)

Mirrored at: [https://github.com/shu-ming-cap/Capsules](https://codeberg.org/shuming/Capsules)

Author Cryptographic Identity:

`ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIA28W/qdAr1JSsyXZ+SGDb5IvZpNUDSllcrkjZ9Bfzx4`

Fingerprint:

`SHA256:vT3oSKlgQd6iJrjrU3YqvQ0GEKmiG4CRcslqfZWCDwY**

## 10 FOUNDATIONAL PROOFS 

The following works provide the technical and structural proofs for this architecture: 
 
NAKAMOTO, S. (2008), Bitcoin: A Peer-to-Peer Electronic Cash System (Append-only logs using cryptographic hash chaining).

KLEPPMANN, M., WIGGINS, A., VAN HARDENBERG, P., MCDANIEL, M. (2019), Local-first Software: You Own Your Data, in Spite of the Cloud (User-owned data applying CRDT-based synchronization). 

SHAPIRO, M., PREGUIÇA, N., BAQUERO, C., ZAWIRSKI, M. (2011), A Comprehensive Study of Convergent and Commutative Replicated Data Types (The technical foundations of CRDT solutions). 

TORVALDS, L. et al. (2005), Git: Distributed Version Control System. https://git-scm.com (Merkle-DAG based version control).

BENET, J. (2014), IPFS – Content Addressed, Versioned, P2P File System (Content-addressable, immutable object graphs).  

BERNSTEIN, D., DUIF, N., LANGE, T., SCHWABE, P., YANG, B.-Y. (2012), High-Speed High-Security Signatures (Efficient ED25519 elliptic-curve signatures). 

HAAS, A. et al. (2017), Bringing the Web up to Speed with WebAssembly (Portable, size-efficient stack virtual machines).

CLARK, L. (2019), Standardizing WASI: A System Interface to Run WebAssembly Outside the Web (Sandboxed, portable system interface abstraction). 

W3C EPUB Working Group (2023), EPUB 3.3 Specification (Standardized container and foreign resource model).

CHAUM, D. (1981), Untraceable Electronic Mail, Return Addresses, and Digital Pseudonyms(Cryptographic mix networks for unlinkable routing).

CLARKE, I., SANDBERG, O., WILEY, B., HONG, T. (2001), Freenet: A Distributed Anonymous Information Storage and Retrieval System (Censorship-resistant decentralized storage network).

## 11 APPENDIX A: EXAMPLE CAPSULE MANIFEST

### 11.1 Directory Structure

```bash
my_capsule.epub/
│
├── mimetype│
├── META-INF/
│   ├── container.xml
├── OEBPS/
│   ├── content.opf
│   ├── content.xhtml
│   ├── cover.jpg
│   └── capsule/
│       ├── logic.wasm
│       ├── keysregistry.json
│       ├── history.merkle
│       └── content.dat
```

### 11.2 content.opf

```xml
\<?xml version="1.0" encoding="UTF-8"?\>`
 \<package`
    xmlns="http://www.idpf.org/2007/opf"`
    version="3.0"`
    unique-identifier="pub-id"\>`
  \<metadata xmlns:dc="http://purl.org/dc/elements/1.1/"\>`
    \<dc:identifier id="pub-id"\>urn:uuid:12345678-1234-1234-	1234-123456789abc\</dc:identifier\>`
    \<dc:title\>My Capsule Publication\</dc:title\>`
    \<dc:language\>en\</dc:language\>`
  \</metadata\>`
  \<manifest\>`
    \<!-- Core reading content --\>`
    \<item id="content"`
          href="content.xhtml"`
          media-type="application/xhtml+xml"/\>`
    \<item id="cover"`
          href="cover.jpg"`
          media-type="image/jpeg"`
          properties="cover-image"/\>`
    \<!-- Capsule primitives (foreign resources) --\>`
    \<item id="capsule-logic"`
          href="capsule/logic.wasm"`
          media-type="application/wasm"/\>`
    \<item id="capsule-registry"`
          href="capsule/keysregistry.json"`
          media-type="application/json"/\>`
    \<item id="capsule-history"`
          href="capsule/history.merkle"`
          media-type="application/octet-stream"/\>`
    \<item id="capsule-payload"`
          href="capsule/content.dat"`
          media-type="application/octet-stream"/\>`
  \</manifest\>`
  \<spine\>`
    \<itemref idref="content"/\>`
  \</spine\>`
\</package\>`
```

## 12 APPENDIX B: EXAMPLE KEY REGISTRY

```json
{  
 "registry_version": "1.0",  
 "capsule_id": "urn:uuid:12345678-1234-1234-1234-123456789abc",  
 "keys": [  
	{  
	 "key_id": "alice",  
	 "role": "publisher",  
	 "ed25519_public": "a1b2c3d4e5f6...",  
	 "x25519_public": "f7e8d9c0b1a2...",  
	 "permissions": [  
	 	"update_content",  
	 	"update_logic",  
	 	"manage_keys",  
	 	"snapshot_history"  
	 ]  
	},  
	{  
	 "key_id": "eve",  
	 "role": "governance",  
	 "ed25519_public": "e5f6a7b8c9d0...",  
	 "x25519_public": "b1a2f3e4d5c6...",  
	 "permissions": [  
		"update_logic",  
		"manage_keys",  
		"snapshot_history"  
	 ]  
	},  
	{  
	 "key_id": "issuer",  
	 "role": "key_authority",  
	 "ed25519_public": "aa11bb22cc33...",  
	 "x25519_public": "dd44ee55ff66...",  
	 "permissions": [  
		"manage_keys"  
	 ]  
	},  
	{  
	 "key_id": "bob",  
	 "role": "editor",  
	 "ed25519_public": "b2c3d4e5f6a7...",  
	 "x25519_public": "e8d9c0b1a2f3...",  
	 "permissions": [  
		"update_content"  
	 ]  
	},  
	{  
	 "key_id": "carol",  
	 "role": "editor_plus",  
	 "ed25519_public": "c3d4e5f6a7b8...",  
	 "x25519_public": "d9c0b1a2f3e4...",  
	 "permissions": [  
	 	"update_content"
	 	"append_comment"
	 ]  
	},  
	{  
	 "key_id": "david",  
	 "role": "commenter",  
	 "ed25519_public": "d4e5f6a7b8c9...",  
	 "x25519_public": "c0b1a2f3e4d5...",  
	 "permissions": [  
		"append_comment"  
	]  
	}  
 ]  
}
```