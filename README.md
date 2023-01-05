# AthenaK

Experimental version of Athena++ with Kokkos.

## How to clone

The code resides in a private GitLab repo and can only be accessed with two-factor authentication. So first you must create a personal access token for your GitLab account, and then clone recursively:
```
git clone --recursive https://TOKEN_NAME:TOKEN@gitlab.com/theias/hpc/jmstone/athena-parthenon/athenak.git
```
The recursive option clones the Kokkos repository along with Athena. If you clone the code without Kokkos, you must get Kokkos manually in the root directory containing the code:
```
git submodule init
git submodule update
```

## How to build

The code uses cmake to manage builds.  In-source builds are not allowed; you must create a new build directory:
```
mkdir build
cd build
```

Then run `cmake` (version 3.0 or later) for the specific target architecture in the build subdirectory as follows:

### Default build for CPU
```
cmake3 ../
```

### Build for CPU with custom problem generator (located in `/src/pgen/name.cpp`)
```
cmake3 -D PROBLEM=name ../
```

### To build in debug mode, add
```
cmake3 -D CMAKE_BUILD_TYPE=Debug ../
```

### Default build for CPU with MPI
```
cmake3 -D Athena_ENABLE_MPI=ON ../
```

### Default build for Intel Broadwell CPU with Intel C++ compiler and GCC C compiler 
```
cmake3 -DCMAKE_CXX_COMPILER=icpc -D Kokkos_ARCH_BDW=On -D Kokkos_ENABLE_OPENMP=ON \
   -D CMAKE_CXX_FLAGS="-O3 -inline-forceinline -qopenmp-simd -qopt-prefetch=4 -diag-disable 3180 " \
   -D CMAKE_C_FLAGS="-O3 -finline-functions" ../
```

### Default build for NVIDIA V100 GPU (requires GCC and CUDA Toolkit)
E.g. `apollo` at IAS
```
cmake3 -DKokkos_ENABLE_CUDA=On -DKokkos_ARCH_VOLTA70=On -DCMAKE_CXX_COMPILER=${path_to_code}/athenak/kokkos/bin/nvcc_wrapper ../
```


### Debug build for NVIDIA A100 GPU (requires GCC and CUDA Toolkit)
E.g. `apollo` at IAS:
```
cmake3 -DKokkos_ENABLE_CUDA=On -DKokkos_ARCH_AMPERE80=On -DCMAKE_CXX_COMPILER=${path_to_code}/athenak/kokkos/bin/nvcc_wrapper -D CMAKE_BUILD_TYPE=DEBUG ../
```
