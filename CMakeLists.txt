cmake_minimum_required(VERSION 2.8.8)
project(numpycpp)

# set a search path for additional custom cmake modules.
# according to recommendation in https://cmake.org/Wiki/CMake:How_To_Find_Libraries
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/.cmake/modules/")

set(Python_ADDITIONAL_VERSIONS 3)
find_package(PythonInterp REQUIRED)
find_package(PythonLibs REQUIRED)
find_package(Boost REQUIRED COMPONENTS "python")

message("found Boost_VERSION: ${Boost_VERSION}")
#since boost 1.63
message(STATUS "New boost_numpy there for sure, no need to use old piece of code!")
find_package(Boost REQUIRED COMPONENTS "numpy")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")

include_directories(.)

include_directories(${PYTHON_INCLUDE_DIRS})
add_library(demo SHARED demo_boost_numpy.cpp)



#message(STATUS "CMAKE_SOURCE_DIR: ${CMAKE_SOURCE_DIR}")
#add_dependencies(demo "${CMAKE_SOURCE_DIR}/demo.py")
message(STATUS "found Boost_PYTHON_LIBRARY:      ${Boost_PYTHON_LIBRARY}")
message(STATUS "found Boost_LIBRARIES:       ${Boost_LIBRARIES}")

target_link_libraries(demo ${Boost_LIBRARIES})

add_custom_command(TARGET demo POST_BUILD
                   COMMAND ${CMAKE_COMMAND} -E create_symlink ${CMAKE_SOURCE_DIR}/demo.py ${CMAKE_BINARY_DIR}/demo.py)
