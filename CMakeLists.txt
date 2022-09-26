cmake_minimum_required(VERSION 3.19)

project(fluidsGL)

enable_language(CUDA)
find_package(CUDAToolkit REQUIRED)
find_package(GLUT REQUIRED)
find_package(OpenGL REQUIRED)

option(WITH_OPTIMUS "Assume compute (CUDA/HIP) and graphics (OpenGL) are handled by two different GPUs" ON)

add_executable(${PROJECT_NAME} src/${PROJECT_NAME}.cpp src/${PROJECT_NAME}_kernels.cu)
if (WITH_OPTIMUS)
target_compile_definitions(${PROJECT_NAME} PRIVATE OPTIMUS)
endif()
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include ${GLUT_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME} CUDA::cudart CUDA::cufft ${GLUT_LIBRARIES} ${OPENGL_LIBRARIES})

