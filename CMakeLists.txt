cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

project(Altis LANGUAGES CXX CUDA)

find_package(caliper)
include_directories(${caliper_DIR})
add_definitions("-DUSE_CALIPER")
set(CALIPER_CMAKE_MSG "Yes, using Caliper")
list(APPEND LULESH_EXTERNAL_LIBS caliper)

add_subdirectory(src)
target_link_libraries(bfs ${LULESH_EXTERNAL_LIBS})
