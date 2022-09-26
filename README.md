# fluidsGL - Fluids (OpenGL Version)

## Description

An example of fluid simulation using CUDA/HIP and CUFFT/, with OpenGL rendering.

## Key Concepts

Graphics Interop, CUFFT Library, Physically-Based Simulation

## Supported SM Architectures

[SM 3.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 3.7 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 5.3 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 6.1 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.2 ](https://developer.nvidia.com/cuda-gpus)  [SM 7.5 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.0 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.6 ](https://developer.nvidia.com/cuda-gpus)  [SM 8.7 ](https://developer.nvidia.com/cuda-gpus)

## Supported OSes

Linux, Windows

## Supported CPU Architecture

x86_64, armv7l

## CUDA APIs involved

### [CUDA Runtime API](http://docs.nvidia.com/cuda/cuda-runtime-api/index.html)
cudaFree, cudaGraphicsMapResources, cudaFreeArray, cudaGraphicsGLRegisterBuffer, cudaGraphicsResourceGetMappedPointer, cudaDestroyTextureObject, cudaMallocPitch, cudaCreateTextureObject, cudaMalloc, cudaMallocArray, cudaGraphicsUnregisterResource, cudaGraphicsUnmapResources, cudaMemcpy, cudaGetDeviceProperties

## Prerequisites

```
sudo apt install freeglut3-dev
```

## Building

```
mkdir build
cd build
cmake ..
make
```

## References (for more details)

[whitepaper](./doc/fluidsGL.pdf)
