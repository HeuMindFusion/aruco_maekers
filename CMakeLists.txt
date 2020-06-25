cmake_minimum_required(VERSION 3.5)

project(aruco_markers)

set(CMAKE_CXX_STANDARD 14)

set(WINDOWS TRUE CACHE INTERNAL "")
set(PLATFORM_PREFIX "win64" CACHE INTERNAL "")
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
if (CMAKE_HOST_WIN32)
    set(HOME_DIR $ENV{HOMEDRIVE}$ENV{HOMEPATH} CACHE INTERNAL "")
else()
    set(HOME_DIR $ENV{HOME} CACHE INTERNAL "")
endif()
if ("TEST$ENV{MAKE_DEPS_WORKING_DIR}" STREQUAL "TEST")
    set(MAKE_DEPS_WORKING_DIR ${HOME_DIR}/makedeps)
else()    
    set(MAKE_DEPS_WORKING_DIR $ENV{MAKE_DEPS_WORKING_DIR})	
endif()

if (NOT ROOT_DIR_DEFINED)
    add_definitions(-DROOT_DIR="${PROJECT_SOURCE_DIR}")
    set(ROOT_DIR_DEFINED ON)
endif()

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

include(${MAKE_DEPS_WORKING_DIR}/${PLATFORM_PREFIX}/deps.cmake)


find_package(OpenCV 4.0 REQUIRED)
#find_package(OpenCV CONFIG PATHS ${OpenCV_DIR} NO_DEFAULT_PATH)

include_directories(${OpenCV_INCLUDE_DIRS})



# CMake Package Collection


find_package(Boost REQUIRED)
find_package(PCL REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(realsense2 REQUIRED)

find_package(PCL REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
add_definitions(${PCL_DEFINITIONS})
link_directories(${PCL_LIBRARY_DIRS})

include_directories(${OpenCV_INCLUDE_DIRS})

add_subdirectory(create_markers)

add_subdirectory(detect_marker)

add_subdirectory(pose_estimation)
add_subdirectory(camera_calibration)



