cmake_minimum_required(VERSION 3.5)

# Project Name and C++ Standard
project(playback)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

# Find PCL (Point Cloud Library)
find_package(PCL 1.13.1 REQUIRED COMPONENTS common kdtree search sample_consensus filters segmentation visualization)

# Include PCL headers and link libraries  
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# Find the boost libraries that are actually available on the system
find_package(Boost REQUIRED COMPONENTS system filesystem iostreams)

# Create executable
add_executable(environment 
    src/environment.cpp 
    src/render/render.cpp 
)

# Link against PCL libraries and system boost libraries
target_link_libraries(environment 
    ${PCL_LIBRARIES}
    ${Boost_LIBRARIES}
)

# Set runtime library path and linker flags to handle missing OpenNI symbols
set_target_properties(environment PROPERTIES
    INSTALL_RPATH "/usr/lib/x86_64-linux-gnu:/usr/local/lib"
    BUILD_WITH_INSTALL_RPATH TRUE
    LINK_FLAGS "-Wl,--unresolved-symbols=ignore-all"
)