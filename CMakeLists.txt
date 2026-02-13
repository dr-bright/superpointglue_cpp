cmake_minimum_required(VERSION 3.3)
project(superpointglue_cpp)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_BUILD_TYPE "release")
add_definitions(-w)

add_subdirectory(${PROJECT_SOURCE_DIR}/3rdparty/tensorrtbuffer)


find_package(OpenCV 4.2 REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(CUDA REQUIRED)
find_package(yaml-cpp REQUIRED)


add_library(${PROJECT_NAME}_lib SHARED
  src/super_point.cpp
  src/super_glue.cpp
  src/read_config.cpp
  include/superpointglue_cpp/read_config.h
  include/superpointglue_cpp/super_glue.h
  include/superpointglue_cpp/super_point.h
  include/superpointglue_cpp/utils.h
  )

target_include_directories(
  ${PROJECT_NAME}_lib
  PUBLIC
  ${PROJECT_SOURCE_DIR}/include
  ${OpenCV_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  ${CUDA_INCLUDE_DIRS}
  ${YAML_CPP_INCLUDE_DIR}
  )

target_link_libraries(${PROJECT_NAME}_lib
  nvinfer
  nvonnxparser
  ${OpenCV_LIBRARIES}
  ${CUDA_LIBRARIES}
  yaml-cpp
  tensorrtbuffer_lib
)

target_compile_definitions(${PROJECT_NAME}_lib
  PUBLIC
      "SUPERPOINTGLUE_CPP_CONFIG_DEFAULT_PATH=\"${PROJECT_SOURCE_DIR}/config/config.yaml\""
)

add_library(${PROJECT_NAME}::lib ALIAS ${PROJECT_NAME}_lib)

# install(TARGETS )