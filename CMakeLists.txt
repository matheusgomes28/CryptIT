# ================================ #
# === Set up build requirements == #
# ================================ #
cmake_minimum_required(VERSION 3.28.1)
project(CryptIT LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN On)

set(TARGET_NAME ${PROJECT_NAME})

if (BUILD_TESTS)
    # Print out the build type
    message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
    enable_testing()
endif ()

# ================================ #
# === Find required modules   ==== #
# ================================ #
# Modules that need's to be in root (Local projects)
find_package(Qt6 REQUIRED)
find_package(cryptopp REQUIRED)


# ================================ #
# === Create build targets ======= #
# ================================ #
add_subdirectory(src)
