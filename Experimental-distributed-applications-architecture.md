
## Distributed Application Project Light

This is what we believe can be working in less than a week of effort and without substantial innovation. The light version of the distributed application, exposes sandboxed disc, networking and API functions through javascript. Golang, Python and C can be compiled down from LLVM IR to a asm.js target with Emscripten and interpreted by the javascript interpreter.

This approach works immediately, using an existing tool chain and is acceptable in the immediate term.

## Distributed applications architecture

This document overviews Skycoin distributed application architecture. This project is experimental, extremely long term and open ended. The project involves implementation and documentation of a virtual machine implementation, development of a new intermediate language , development of a new compiler and run-time environment. 

The objective of this project is to 
* achieve deterministic builds from source to the intermediate language for C, Python and Golang programs
* be able to run intermediate language programs interpreted or as native code compiled by LLVM
* achieve high security application sandboxing (memory safety, sandboxing of disc access, sandboxing of network access)
* be able to package, deploy, stop, start and move processes between computers (personal cloud). 

This project overlaps with NaCl/pNaCl, Docker, LLVM and Rose. Familiarity with assembly language, writing C compilers, source transformation tools, LLVM backend, CIL, LLVM IR and the XL programming language recommended.