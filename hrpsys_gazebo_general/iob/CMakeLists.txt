#cmake_minimum_required(VERSION 2.4.6)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

set(target hrpIo)

#rosbuild_find_ros_package(hrpsys)
if(NOT CATKIN_INCLUDE_DIRS)
  execute_process(
    COMMAND rospack find hrpsys
    OUTPUT_VARIABLE hrpsys_PACKAGE_PATH
    OUTPUT_STRIP_TRAILING_WHITESPACE
    )

  execute_process(
    COMMAND rospack find hrpsys_gazebo_msgs
    OUTPUT_VARIABLE hrpsys_gazebo_msgs_PACKAGE_PATH
    OUTPUT_STRIP_TRAILING_WHITESPACE
    )
endif()

string (REGEX REPLACE " " ";" LIST_CATKIN_INCLUDE_DIRS "${CATKIN_INCLUDE_DIRS}")
include_directories(BEFORE ${LIST_CATKIN_INCLUDE_DIRS};${hrpsys_PACKAGE_PATH}/include;${hrpsys_gazebo_msgs_PACKAGE_PATH}/msg_gen/cpp/include;${hrpsys_gazebo_msgs_PACKAGE_PATH}/srv_gen/cpp/include)
link_directories("/opt/ros/$ENV{ROS_DISTRO}/lib")
add_library(${target} SHARED iob.cpp)
target_link_libraries(${target} rt roscpp rostime)
target_link_libraries(${target} rt)

install(TARGETS ${target}
  RUNTIME DESTINATION bin CONFIGURATIONS Release Debug
  LIBRARY DESTINATION lib CONFIGURATIONS Release Debug
)
