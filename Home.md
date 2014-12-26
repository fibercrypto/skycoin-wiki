# Skycoin

* [Installation](Installation)

# Standards

* [Skycoin Standard: Brainwallet](BrainWallet)
Visual Representation of Deterministic Wallet Seeds

# Developer Tutorials (FINISH THIS)


Cipher is the Skycoin cryptography library, which standardizes operations across the Skycoin project. This tutorial covers cryptographic operations, such as generating private keys, public keys, deterministic addresses and encrypting data.
* [Cipher Tutorial](cipher)

This tutorial covers stealth address generation and key exchange.
* [Stealth Addresses Tutorial](stealth)

This tutorial covers generating a skycoin service, creating new packet types and sending and receiving packets.
* [Services Tutorial](Services)

This tutorial covers serializing structs and data objects using the Skycoin encoder library. Skycoin uses a standard binary struct serialization format across the whole project.
* [Encoder Tutorial](Encoder)

# Skycoin Infrastructure

This covers infrastructure that needs to be implemented. After implementation, it will be moved to tutorials. Substantial bounties are available for helping with these!

This document overviews the Merkle-DAG distributed file system. This is used for blockchain storage, file sharing and constructing distributed applications. Implementation is extremely high priority.

* [Merkle DAG implementation Documents] (MerkleDAG)

This document overviews the Skywire darknet and mesh network implementation. Implementation is high priority.
* [Darknet Implementation Documentation] (Darknet)

This document overviews types of distributed hash tables required for Skycoin. Implementation is low priority.
* [DHT Implementation Documentation](DHT)

This document overviews the Skycoin asynchronous messaging system. This system allows sending short UDP messages to public keys. This is required for distributed applications. Implementation is medium priority.

* [Messaging Implementation Documentation] (Messaging)

# Skycoin Long Term 

Distributed Application Project Light. This is what we believe can be working in less than a week of effort and without substantial innovation. The light version of the distributed application, exposes sandboxed disc, networking and API functions through javascript. Golang, Python and C can be compiled down from LLVM IR to a asm.js target with Emscripten and interpreted by the javascript interpreter.

This approach works immediately, using an existing tool chain and is acceptable in the immediate term.

* [Distributed Applications Light] (DistributedApplicationLight)

This document overviews Skycoin distributed application architecture. This project is experimental, extremely long term and open ended. The project involves implementation and documentation of a virtual machine implementation, development of a new intermediate language , development of a new compiler and run-time environment. 

The objective of this project is to 
* achieve deterministic builds from source to the intermediate language for C, Python and Golang programs
* be able to run intermediate language programs interpreted or as native code compiled by LLVM
* achieve high security application sandboxing (memory safety, sandboxing of disc access, sandboxing of network access)
* be able to package, deploy, stop, start and move processes between computers (personal cloud). 

This project overlaps with NaCl/pNaCl, Docker, LLVM and Rose. Familiarity with assembly language, writing C compilers, source transformation tools, LLVM backend, CIL, LLVM IR and the XL programming language recommended.

* [Distributed Applications Project] (DistributedApplications)