# Copyright (c) 2013 Andrew Kemp
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

# Find and include HPX settings
if(HPX_ROOT AND NOT HPX_DIR)
  set(HPX_DIR "${HPX_ROOT}/lib/cmake/HPX")
endif()

# Require a recent version of cmake
cmake_minimum_required(VERSION 2.8.4 FATAL_ERROR)

# This project is C++ based.
project(HPXExternalHelloWorld)

# Instruct cmake to find the HPX settings, it looks for HPXConfig.cmake in ${HPX_DIR}
find_package(HPX)

include_directories(${HPX_INCLUDE_DIR})
link_directories(${HPX_LIBRARY_DIR})

# Adjust auxiliary build settings for HPX to work properly
if(NOT MSVC)
  set(HPX_RPATH ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}${HPX_RPATH})
else()
  # enable solution folders for MSVC
  set_property(GLOBAL PROPERTY USE_FOLDERS ON)
  if(HPX_NO_INSTALL)
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/$(Configuration)" CACHE PATH
      "Prefix prepended to install directories." FORCE)
  endif()
endif()

include_directories(external)

# add example executable
set (sources 
    hello_world/main.cpp
    external/EasyBMP/EasyBMP.cpp)

add_hpx_executable(hello_world
             SOURCES ${sources}
             FOLDER 
             COMPONENT_DEPENDENCIES)
