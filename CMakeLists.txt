cmake_minimum_required(VERSION 3.15)

execute_process(
    COMMAND git clone -n https://github.com/BlurringShadow/stdsharp
    COMMAND cd stdsharp
    COMMAND git sparse-checkout init --cone
    COMMAND git sparse-checkout set cmake
    COMMAND git checkout
    COMMAND cd ..
)

include(stdsharp/cmake/Utils.cmake)

project("observable_memory" VERSION 0.1.0 LANGUAGES CXX)

init_proj()

set(FOONATHAN_MEMORY_BUILD_EXAMPLES OFF)
set(FOONATHAN_MEMORY_BUILD_TESTS OFF)
add_subdirectory(foonathan/memory)

config_interface_lib(observable_memory)
target_link_libraries(
    observable_memory
    INTERFACE GSL foonathan_memory stdsharp
)

verbose_message("Successfully added all dependencies and linked against them.")

#
# Install library for easy downstream inclusion
#
target_install(observable_memory VER ${PROJECT_VERSION} NAMESPACE "observable_memory::")

add_subdirectory(tests)