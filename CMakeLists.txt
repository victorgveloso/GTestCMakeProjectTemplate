cmake_minimum_required(VERSION 3.16)
project("YOUR_PROJECT_NAME_HERE")

set(CMAKE_CXX_STANDARD 17)
include_directories(src)

add_subdirectory(src)
add_subdirectory(test)