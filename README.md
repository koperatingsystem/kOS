# kOS
Welcome to the k Operating System project.  
kOS aims to be an operating system that's both friendly to its users and to the people developing software for and on it.

We currently have no prototypes to present.

## Branding & Name
You may refer to kOS as:
- kOS
- (The) kOS Project
- (The) k Operating System
- (The) k Operating System Project

The k is indeed written as lowercased.  
It has no special meaning - "kOS" was simply the name of the project since the [early days](https://github.com/kosmas12/kOS).

The official kOS logo can be found in the [Branding](Branding) folder.

## Documentation
There are documents on various things used within, by or on kOS within the [Documentation](Documentation) folder.

## Other stuff
### Build instructions
To build kOS' components, you need:
- CMake
- LLVM (clang and LLD)
- Ninja

In each of their directories, you may run:
```sh
cmake -B build -G Ninja # Make sure to specify -DCMAKE_C_COMPILER=$(which clang) in case clang isn't your standard compiler.
cmake --build build
```
