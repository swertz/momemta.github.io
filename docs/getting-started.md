# Getting started

## The Matrix Element method

## How to install MoMEMta

### Prerequisites

MoMEMta depends on the following libraries and tools:

 - LHAPDF (>=6)
 - CMake (>=3.2)
 - Boost (>=1.54)
 - ROOT (>=5.34.09)
 - A C++11-capable compiler

**Note**: MoMEMta has only been tested on GNU/Linux.

### Compilation

Retrieve the code on [our github repository](https://github.com/MoMEMta/MoMEMta/releases). Unpack the archive and / or go to the `MoMEMta` directory. Next, execute the following:
```
mkdir build
cd build
cmake ..
make -j 4
```
You can now use the library `libmomemta.so` with your own code.

If you have admin rights on your system, you can make MoMEMta (public headers and library) available system-wide using:
```
make install
```

### Build options

The following options are available when configuring the build (when running `cmake ..`):

   * `-DCMAKE_INSTALL_PREFIX=(path)`: Install MoMEMta in a specific location when running `make install` (useful if you don't have admin rights)
   * `-DPROFILING=ON`: Generate debugging symbols and profiling information (requires `gperftools`)
   * `-DBOOST_ROOT=(path)`: Use specific Boost library installation
   * `-DTESTS=ON`: Also compile the test executables
   * `-DEXAMPLES=OFF`: Do not compile the example executables
