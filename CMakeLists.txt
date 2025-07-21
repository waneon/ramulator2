cmake_minimum_required(VERSION 3.24)

project(
  Ramulator
  VERSION 2.0
  LANGUAGES CXX)

# cmake settings
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# c++ settings
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# libraries
include(cmake/CPM.cmake)

cpmaddpackage("gh:jbeder/yaml-cpp#2f86d13775d119edbb69af52e5f566fd65c6953b")
cpmaddpackage("gh:gabime/spdlog@1.11.0")
cpmaddpackage("gh:p-ranav/argparse@2.9")

# HACK: This is very hacky. But due to ramulator2 code structure, we need to do
# this....
include_directories(${spdlog_SOURCE_DIR}/include)
include_directories(${argparse_SOURCE_DIR}/include)
include_directories(${yaml-cpp_SOURCE_DIR}/include)

# targets
include_directories(${CMAKE_SOURCE_DIR}/src)

add_library(ramulator SHARED)
target_link_libraries(ramulator PUBLIC yaml-cpp spdlog)
target_include_directories(ramulator PUBLIC src)

add_executable(ramulator-exe)
target_link_libraries(ramulator-exe PUBLIC ramulator argparse)
set_target_properties(ramulator-exe PROPERTIES OUTPUT_NAME ramulator2)

add_subdirectory(src)
