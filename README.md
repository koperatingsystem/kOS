# kOS
Welcome to the k Operating System project.  
kOS aims to be an operating system that's both friendly to its users and to the people developing software for and on it.

We currently have no prototypes to present.

## Build instructions
To build kOS' components, you need:
- CMake
- LLVM (clang and LLD)
- Ninja

In each of their directories, you may run:
```sh
cmake -B build -G Ninja # Make sure to specify -DCMAKE_C_COMPILER=$(which clang) in case clang isn't your standard compiler.
cmake --build build
```
