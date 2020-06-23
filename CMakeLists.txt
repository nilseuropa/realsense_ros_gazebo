cmake_minimum_required(VERSION 2.8.3)
project(realsense_ros_gazebo)

find_package(catkin REQUIRED COMPONENTS roscpp std_msgs std_srvs geometry_msgs nav_msgs tf dynamic_reconfigure message_generation)
include_directories(include ${catkin_INCLUDE_DIRS})

if(POLICY CMP0054)
  cmake_policy(SET CMP0054 NEW)
endif()

find_package(gazebo REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")
include_directories(${GAZEBO_INCLUDE_DIRS})
link_directories(${GAZEBO_LIBRARY_DIRS})

find_package(Boost REQUIRED COMPONENTS thread)
include_directories(${Boost_INCLUDE_DIRS})

add_service_files(
  FILES
  SetBias.srv
)

generate_messages(
  DEPENDENCIES geometry_msgs
)

generate_dynamic_reconfigure_options(
  cfg/SensorModel.cfg
)

catkin_package(
    CATKIN_DEPENDS roscpp std_msgs geometry_msgs nav_msgs tf message_runtime
    INCLUDE_DIRS include
    LIBRARIES
)

###########
## Build ##
###########

add_library(realsense_gazebo_plugin src/RealSensePlugin.cpp src/gazebo_ros_realsense.cpp)
target_link_libraries(realsense_gazebo_plugin ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES})
add_dependencies(realsense_gazebo_plugin ${catkin_EXPORTED_TARGETS})

#############
## Install ##
#############

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

install(TARGETS
  realsense_gazebo_plugin
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY meshes
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(DIRECTORY xacro
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
