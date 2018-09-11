# cmake -DOpenGL_GL_PREFERENCE="GLVND" -DCMAKE_C_COMPILER="/home/$USER/code/install/gcc/gcc-5.5.0/bin/gcc" -DCMAKE_CXX_COMPILER="/home/$USER/code/install/gcc/gcc-5.5.0/bin/g++" /home/$USER/code/src/cuda_samples/particles

# OR

# export CC=/usr/local/bin/gcc
# export CXX=/usr/local/bin/g++
# cmake -DOpenGL_GL_PREFERENCE="GLVND" /home/$USER/code/src/cuda_samples/particles

# OR

# CUDA 9.2 supports Fedora 27, so no need to use ancient compiler.
# cmake -DOpenGL_GL_PREFERENCE="GLVND" /home/$USER/code/src/cuda_samples/particles

cmake_minimum_required(VERSION 2.8)
project(particles)

## CUDA
find_package(CUDA REQUIRED)

## OpenGL
# OPENGL_FOUND, OPENGL_INCLUDE_DIR, OPENGL_LIBRARIES
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIR})

## GLUT
# GLUT_FOUND, GLUT_INCLUDE_DIR, GLUT_LIBRARIES
find_package(GLUT REQUIRED)
include_directories(${GLUT_INCLUDE_DIR})

# GeForce GTX 1080 has Compute Capability 6.1
CUDA_SELECT_NVCC_ARCH_FLAGS(ARCH_FLAGS Auto)
list(APPEND CUDA_NVCC_FLAGS ${ARCH_FLAGS})
message(STATUS "CUDA_NVCC_FLAGS: ${CUDA_NVCC_FLAGS}")

include_directories("${CMAKE_CURRENT_LIST_DIR}/../common/inc")

cuda_add_executable(particles particles.cpp
                              particleSystem.cpp
                              particleSystem_cuda.cu
                              render_particles.cpp
                              shaders.cpp)
target_link_libraries(particles ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES})

