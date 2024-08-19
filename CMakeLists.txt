cmake_minimum_required(VERSION 3.21)

# Set the project name and version
project(Template VERSION 1.0.0)

# C++ standard settings
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set visibility and architecture settings
set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

# Add the mod binary
add_library(${PROJECT_NAME} SHARED
    src/main.cpp
    # Add any extra C++ source files here
)

# Find Geode SDK
if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define the GEODE_SDK environment variable to point to the Geode SDK location.")
else()
    message(STATUS "Found Geode SDK at: $ENV{GEODE_SDK}")
endif()

# Add the Geode SDK as a subdirectory
add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)

# Ensure CPM is included before trying to use CPMAddPackage
include(CPM)

# Add tinyxml2 as a dependency
CPMAddPackage("gh:leethomason/tinyxml2@10.0.0")  # Fixed versioning to the latest release

# Link tinyxml2 to the project
target_link_libraries(${PROJECT_NAME} tinyxml2)

# Set up dependencies, resources, and link Geode
setup_geode_mod(${PROJECT_NAME})