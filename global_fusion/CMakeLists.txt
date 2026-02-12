cmake_minimum_required(VERSION 2.8.3)
project(global_fusion)

set(CMAKE_BUILD_TYPE "Release")
# set(CMAKE_CXX_FLAGS "-std=c++14")
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#-DEIGEN_USE_MKL_ALL")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

find_package(Ceres REQUIRED)

find_package(OpenMP REQUIRED)
if (OpenMP_CXX_FOUND)
  message(STATUS "OpenMP found, linking to <target_name>")
endif()

add_subdirectory(./ThirdParty/GeographicLib/)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${CERES_INCLUDE_DIRS}
  ./ThirdParty/GeographicLib/include/
)


catkin_package()


add_executable(global_fusion_node 
	src/globalOptNode.cpp
	src/globalOpt.cpp)

# target_link_libraries(global_fusion_node ${catkin_LIBRARIES} ${CERES_LIBRARIES} libGeographiccc) 
target_link_libraries(global_fusion_node ${catkin_LIBRARIES} ${CERES_LIBRARIES} libGeographiccc OpenMP::OpenMP_CXX)

