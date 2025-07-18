cmake_minimum_required (VERSION 2.8)
project (ACMH)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(CUDA REQUIRED) # For Cuda Managed Memory and c++11
find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED core imgproc imgcodecs)

include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(.)

set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};-O3 --use_fast_math --maxrregcount=128 --ptxas-options=-v -std=c++20 --compiler-options -Wall)

# if(CMAKE_COMPILER_IS_GNUCXX)
#     add_definitions(-std=c++20)
#     add_definitions(-pthread)
#     add_definitions(-Wall)
#     add_definitions(-Wextra)
#     add_definitions(-pedantic)
#     add_definitions(-Wno-unused-function)
#     add_definitions(-Wno-switch)
#     set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O3 -ffast-math -march=native") # extend release-profile with fast-math
# endif()

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()


# For compilation ...
# Specify target & source files to compile it from
cuda_add_executable(
    ACMH
    main.h
    ACMH.h
    ACMH.cpp
    ACMH.cu
    main.cpp
    )

# For linking ...
# Specify target & libraries to link it with
target_link_libraries(ACMH
    ${OpenCV_LIBS}
    )
