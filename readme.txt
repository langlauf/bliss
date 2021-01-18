bliss is an open-source tool for computing
canonical labelings and automorphism groups of graphs.
For more information about bliss, please see the bliss homepage at
https://users.aalto.fi/tjunttil/bliss

Compiling
---------

In Linux and macOS, one can use GNU Make to compile the "bliss" executable,
as well as the static and shared libraries, with

make -f Makefile-manual

By default, the automorphism group size computation uses
limited precision floating point numbers.
To enable precise size computation, one should have
the GNU Multiple Precision Arithmetic Library installed
and compile with 

make -f Makefile-manual gmp


On Linux and macOS with CMake installed, one can also use

cmake .
cmake --build .

To enable GNU GMP support, compile with 

cmake -D USE_GMP=ON .
cmake --build .


On Windows with Visual Studio and CMake, use 

cmake .
cmake --build . --config Release
