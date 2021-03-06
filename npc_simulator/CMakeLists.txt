cmake_minimum_required(VERSION 2.8.3)
project(npc_simulator)

add_compile_options(-std=c++14)

find_package(catkin REQUIRED COMPONENTS
  message_generation
  roscpp
  sensor_msgs
  std_msgs
  tf2
  tf2_geometry_msgs
  tf2_ros
  autoware_perception_msgs
  autoware_lanelet2_msgs
  uuid_msgs
  unique_id
  lanelet2_extension
  dummy_perception_publisher
)

add_message_files(
  DIRECTORY msg
  FILES
    LaneChangeDir.msg
    LaneFollowMode.msg
    Object.msg
)

add_service_files(
  DIRECTORY srv
  FILES
    GetObject.srv
)

generate_messages(
  DEPENDENCIES
    std_msgs
    geometry_msgs
    autoware_perception_msgs
    uuid_msgs
    dummy_perception_publisher
)

catkin_package(
  INCLUDE_DIRS
    include
  CATKIN_DEPENDS
    message_runtime
    roscpp
    sensor_msgs
    tf2
    tf2_geometry_msgs
    tf2_ros
    autoware_perception_msgs
    autoware_lanelet2_msgs
    pcl_ros
    uuid_msgs
    unique_id
    lanelet2_extension
    dummy_perception_publisher
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_executable(npc_simulator_node
  src/main.cpp
  src/node.cpp
)

add_dependencies(npc_simulator_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(npc_simulator_node
  ${catkin_LIBRARIES}
)

install(
  TARGETS
  npc_simulator_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY
    launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
