cmake_minimum_required(VERSION 3.19)

project(fluidsGL CXX)

option(WITH_OPTIMUS "Assume compute (CUDA/HIP) and graphics (OpenGL) are handled by two different GPUs" ON)

find_package(GLUT REQUIRED)
find_package(OpenGL REQUIRED)

include(CheckLanguage)

check_language(CUDA)

if (CMAKE_CUDA_COMPILER)
enable_language(CUDA)
find_package(CUDAToolkit REQUIRED)

add_executable(${PROJECT_NAME} src/${PROJECT_NAME}.cu src/${PROJECT_NAME}_kernels.cu)
target_link_libraries(${PROJECT_NAME} CUDA::cudart CUDA::cufft)
endif()

check_language(HIP)

if (CMAKE_HIP_COMPILER)
enable_language(HIP)
if(NOT DEFINED HIP_PATH)
    if(NOT DEFINED ENV{HIP_PATH})
        set(HIP_PATH "/opt/rocm/hip" CACHE PATH "Path to which HIP has been installed")
    else()
        set(HIP_PATH $ENV{HIP_PATH} CACHE PATH "Path to which HIP has been installed")
    endif()
endif()
if(NOT DEFINED ROCM_PATH)
    if(NOT DEFINED ENV{ROCM_PATH})
        set(ROCM_PATH "/opt/rocm" CACHE PATH "Path to which ROCm has been installed")
    else()
        set(ROCM_PATH $ENV{ROCM_PATH} CACHE PATH "Path to which ROCm has been installed")
    endif()
endif()

# Remember this is the only proper way to propagate debugging options
# down into the GPU device code compiler. This is because the HIP frontend compiler
# is lang++, it and it invokes backend compilers (which are also clang++) by itself.
# Therefore, the frontend clang++ needs -Xclang to forward the options down to the backends.
set(CMAKE_HIP_FLAGS_DEBUG "-ggdb -fstandalone-debug -Xclang -O0 -Xclang -gcodeview" ${CMAKE_HIP_FLAGS})

list(APPEND CMAKE_PREFIX_PATH "${HIP_PATH}" "${ROCM_PATH}")
find_package(HIP REQUIRED)
find_package(rocfft REQUIRED)

add_executable(${PROJECT_NAME} src/${PROJECT_NAME}.cu src/${PROJECT_NAME}_kernels.cu)
target_link_libraries(${PROJECT_NAME} roc::rocfft hip::device)
set_source_files_properties(src/${PROJECT_NAME}.cu PROPERTIES LANGUAGE HIP)
set_source_files_properties(src/${PROJECT_NAME}_kernels.cu PROPERTIES LANGUAGE HIP)
endif()

if (WITH_OPTIMUS)
target_compile_definitions(${PROJECT_NAME} PRIVATE OPTIMUS)
endif()
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include ${GLUT_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME} ${GLUT_LIBRARIES} ${OPENGL_LIBRARIES})

