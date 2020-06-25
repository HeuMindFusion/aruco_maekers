cmake_minimum_required(VERSION 2.4)
project(aruco_markers)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 OLD)
	cmake_policy(SET CMP0015 OLD)
endif(COMMAND cmake_policy)

set (CMAKE_CXX_STANDARD 11)
find_package(OpenCV REQUIRED)


#include_directories(${PROJECT_SOURCE_DIR}/include)

file(GLOB ARUCO_OPENCL_SRC_FILES ../src/*.cpp ../src/*.hpp ../src/opencv2/aruco/* ../src/opencv2/*)

set(generate_marker_src
    src/create_marker.cpp
   )
add_executable(generate_marker ./src/create_marker.cpp ${ARUCO_OPENCL_SRC_FILES})
target_link_libraries(generate_marker 
    ${OpenCV_LIBRARIES}
    )

target_compile_options(generate_marker
    PRIVATE -O3 -std=c++11
    )


set(generate_board_src
    src/create_board.cpp
   )
add_executable(generate_board ${generate_board_src} ${ARUCO_OPENCL_SRC_FILES})
target_link_libraries(generate_board 
    ${OpenCV_LIBRARIES}
    )

target_compile_options(generate_board
    PRIVATE -O3 -std=c++11
    )
