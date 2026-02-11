cmake_minimum_required(VERSION 3.3)
project(tensorrtbuffer)

set(CMAKE_CXX_STANDARD 14)

find_package(CUDA REQUIRED)

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_library(tensorrtbuffer_lib SHARED src/logger.cpp)

target_include_directories(tensorrtbuffer_lib
    PUBLIC
        ${CUDA_INCLUDE_DIRS}
        ${PROJECT_SOURCE_DIR}/include
)

add_library(tensorrtbuffer::lib ALIAS tensorrtbuffer_lib)
