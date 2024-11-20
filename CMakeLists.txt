cmake_minimum_required(VERSION 3.13 FATAL_ERROR)

project(ntp
    VERSION     1.0
    DESCRIPTION "cmake cross platform build script"
)

include(toolchain.cmake)
include(FetchContent)

set(CMAKE_INSTALL_PREFIX ${CMAKE_BINARY_DIR}/install)
message(STATUS "CMAKE_INSTALL_PREFIX: ${CMAKE_INSTALL_PREFIX}")

add_subdirectory(openssl)
add_subdirectory(ntp)
