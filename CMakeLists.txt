cmake_minimum_required(VERSION 3.5)
project(realsense_ros_gazebo)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -Wno-inconsistent-missing-override")
endif()

set(FastRTPS_INCLUDE_DIR /opt/ros/humble/include)
set(FastRTPS_LIBRARY_RELEASE /opt/ros/humble/lib/libfastrtps.so)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(image_transport REQUIRED)
find_package(camera_info_manager REQUIRED)
find_package(sensor_msgs REQUIRED)

find_package(gazebo REQUIRED)

include_directories(
    SYSTEM
    include
    ${GAZEBO_INCLUDE_DIRS}
)

link_directories(
  ${GAZEBO_LIBRARY_DIRS}
)

add_library(realsense_gazebo_plugin SHARED src/RealSensePlugin.cpp src/gazebo_ros_realsense.cpp)
ament_target_dependencies(realsense_gazebo_plugin
    rclcpp
    gazebo_ros
    image_transport
    camera_info_manager
    sensor_msgs
)
target_link_libraries(realsense_gazebo_plugin ${GAZEBO_LIBRARIES})


install(TARGETS realsense_gazebo_plugin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

# install(DIRECTORY include/
#   DESTINATION include/
# )

install(
  DIRECTORY meshes xacro
  DESTINATION share/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

# ament_export_include_directories(include)
# ament_export_libraries(realsense_gazebo_plugin)
ament_package()
