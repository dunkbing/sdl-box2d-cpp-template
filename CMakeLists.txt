cmake_minimum_required(VERSION 3.7)

set(PROJECT_NAME SDL_Box2d_template)
project(${PROJECT_NAME} LANGUAGES CXX)

# Set build type to debug by default.
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()

# find_package(SDL2 REQUIRED)
# include_directories(${SDL2_INCLUDE_DIRS})
# Define the executable.
add_executable(${PROJECT_NAME} main.cpp)

# Set C++ standard.
set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

# Add third party libraries.
add_subdirectory(vendor)

# Set compile flags.
target_compile_options(${PROJECT_NAME} PRIVATE
    # Clang
    $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:AppleClang>>:-Weverything>
    # GCC
    $<$<CXX_COMPILER_ID:GNU>:-Wall -Wextra -Wpedantic -fdiagnostics-color=always>
    # Visual Studio
    $<$<CXX_COMPILER_ID:MSVC>:/W4>
)

# Find dependencies.
find_package(OpenGL COMPONENTS OpenGL REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE OpenGL::GL SDL2-static)
target_link_libraries(${PROJECT_NAME} PRIVATE box2d)