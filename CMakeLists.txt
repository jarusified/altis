cmake_minimum_required(VERSION 3.16)

project(Altis LANGUAGES CXX CUDA)

option(WITH_CALIPER "Build Altis with Caliper" TRUE)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_C_COMPILER "/usr/tce/packages/gcc/gcc-8.3.1/bin/gcc")
set(CMAKE_CXX_COMPILER "/usr/tce/packages/gcc/gcc-8.3.1/bin/g++")

set(CMAKE_THREAD_LIBS_INIT "-lpthread")
set(CMAKE_HAVE_THREADS_LIBRARY 1)
set(CMAKE_USE_WIN32_THREADS_INIT 0)
set(CMAKE_USE_PTHREADS_INIT 1)
set(THREADS_PREFER_PTHREAD_FLAG ON)

if (WITH_CALIPER)
  find_package(caliper REQUIRED)
  if(caliper_FOUND)
    message(STATUS "Found Caliper: " ${caliper_DIR})
    #find_path(caliper_DIR caliper/cali.h
     # HINTS ${caliper_DIR}/include)
    include_directories(${caliper_DIR}/include)
    add_definitions("-DUSE_CALIPER")
    set(CALIPER_CMAKE_MSG "Yes, using Caliper")
    list(APPEND EXTERNAL_LIBS caliper)
  else()
    message(STATUS "Caliper not found!")
  endif()
endif()

add_subdirectory(src)
message(STATUS "External libs: " ${EXTERNAL_LIBS})
target_link_libraries(bfs ${EXTERNAL_LIBS})
