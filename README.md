# A Template for Developing Fortran Application

## About
This repository is a template repository for developing applications in Fortran.
To use it click "Use this template" on top page of this repository.

This template provides
- a simple "hello, world" application using openmp and mpi libraries.
- .fypp file providing a few simple macros.
- cmake files which automate preprocessing and compiling.

## Getting started
### Requirements
This template requires
- a Fortran compiler
- cmake version 3.21 or newer
- fypp preprocesser

You can install them by package manager (brew, conda, etc.).

The tested enviroments are
- gfortran 12.2.0 (arm64)
- cmake 3.24.2
- fypp 3.1

### Build with cmake
Configure the build with
```
cmake -B build
```
You can pass additional options to CMake to customize the build.
If you want to say goodbye, run
```
cmake -B build -DI_WANT_TO_GOODBYE=1
```

To build the hello world application, run
```
cmake --build build
```

Now, you can use application by
```
./build/src/hello_world
mpirun -n 4 ./build/src/hello_world
```

To install the application to ```location```, run
```
cmake --install build --prefix location
```

This hello world application is useless, so I strongly recommend that you make sure that ```location``` can be deleted immediately.
