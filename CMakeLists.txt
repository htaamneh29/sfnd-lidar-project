cmake_minimum_required(VERSION 3.5)

# Project Name and C++ Standard
project(playback)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

# Find PCL (Point Cloud Library)
find_package(PCL 1.13.1 REQUIRED)

# Include PCL headers and link libraries
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# Optional: Remove vtkproj4 if it causes linking issues
list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")

# Create executable
add_executable(environment 
    src/environment.cpp 
    src/render/render.cpp 
)

# Link against PCL libraries
target_link_libraries(environment ${PCL_LIBRARIES})