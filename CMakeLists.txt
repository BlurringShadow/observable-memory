cmake_minimum_required(VERSION 3.15)

include(stdsharp/cmake/Utils.cmake)

set(CMAKE_CXX_STANDARD 23)

#
# Project details
#
project("observable_memory" VERSION 0.1.0 LANGUAGES CXX)

init_proj()

config_interface_lib(observable_memory)

#
# Model project dependencies
#
find_package(Microsoft.GSL CONFIG REQUIRED)

target_link_libraries(
    observable_memory
    INTERFACE Microsoft.GSL::GSL
)

verbose_message("Successfully added all dependencies and linked against them.")

#
# Install library for easy downstream inclusion
#
target_install(observable_memory VER ${PROJECT_VERSION} NAMESPACE "observable_memory::")

add_subdirectory(tests)